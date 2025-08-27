---
url: "/ko/login/"
layout: "single"
build:
 render: always
 list: never
hidemeta: true
---

<div style="text-align: center;">
  <button id="login-btn" class="custom-button">로그인</button>
  <br><br>
  회원 공개 글입니다. <br>
  로그인 또는 무료 회원가입 후 진행해주세요.
</div>

<!-- Netlify Identity widget -->
<script src="https://identity.netlify.com/v1/netlify-identity-widget.js"></script>

<script>
(function () {
  function sameOrigin(u) {
    try { return new URL(u, location.origin).origin === location.origin; }
    catch (e) { return false; }
  }

  // 로그인 후 돌아갈 목적지 기억
  function rememberReturn() {
    const qs = new URLSearchParams(location.search);
    let dest = qs.get('next') || qs.get('redirect');

    // 절대/상대경로 모두 허용하되, 절대경로면 동일 출처만
    if (dest) {
      try {
        const u = new URL(dest, location.origin);
        if (u.origin !== location.origin) dest = null;
        else dest = u.pathname + u.search + u.hash; // 절대→상대
      } catch (e) { dest = null; }
    }

    // next가 없으면 referrer 보강 (동일 출처 & login/logout 제외)
    if (!dest && document.referrer && sameOrigin(document.referrer)) {
      const ref = new URL(document.referrer);
      if (!/\/ko\/login\/|\/ko\/logout\//.test(ref.pathname)) {
        dest = ref.pathname + ref.search + ref.hash;
      }
    }

    if (dest) sessionStorage.setItem('afterLogin', dest);
  }

  function pickDest() {
    return sessionStorage.getItem('afterLogin') || '/ko/';
  }

  // Netlify Edge가 roles 판정할 수 있게 nf_jwt 쿠키 심기
  async function setJwtCookie(user) {
    try {
      // gotrue-js: user.jwt() 가 토큰 문자열 반환
      const token = await user.jwt();
      document.cookie =
        'nf_jwt=' + token +
        '; Path=/' +
        '; Max-Age=1800' +          // 30분
        '; SameSite=Lax' +
        (location.protocol === 'https:' ? '; Secure' : '');
    } catch (e) {
      console.warn('Failed to set nf_jwt cookie:', e);
    }
  }

  function init() {
    const id = window.netlifyIdentity;
    if (!id) return;

    rememberReturn();

    const btn = document.getElementById('login-btn');
    if (btn) btn.addEventListener('click', function () { id.open('login'); });

    // 이미 로그인 상태로 /ko/login/ 들어온 경우: 쿠키 보강 후 next가 있으면 곧장 복귀
    id.on('init', async function (user) {
      if (user) {
        await setJwtCookie(user);
        const qs = new URLSearchParams(location.search);
        if (qs.get('next') || sessionStorage.getItem('afterLogin')) {
          const dest = pickDest();
          sessionStorage.removeItem('afterLogin');
          location.replace(dest);
        }
      }
    });

    // 로그인 성공 → 쿠키 설정 → 위젯 닫기 → 목적지 이동
    id.on('login', async function (user) {
      await setJwtCookie(user);
      id.close();
      const dest = pickDest();
      sessionStorage.removeItem('afterLogin');
      location.replace(dest); // login 히스토리 남기지 않음
    });

    id.on('logout', function () {
      // 쿠키 제거
      document.cookie =
        'nf_jwt=; Max-Age=0; Path=/; SameSite=Lax' +
        (location.protocol === 'https:' ? '; Secure' : '');
      location.reload();
    });

    id.init();
  }

  document.readyState === 'loading'
    ? document.addEventListener('DOMContentLoaded', init)
    : init();
})();
</script>