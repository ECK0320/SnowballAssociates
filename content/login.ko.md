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
  <br>
  <p>회원 공개 글입니다.<br>
  로그인 또는 무료 회원가입 후 진행해주세요.</p>
</div>

<!-- Netlify Identity widget -->
<script src="https://identity.netlify.com/v1/netlify-identity-widget.js"></script>
<script>
(function () {
  function sameOrigin(u) {
    try { return new URL(u, location.origin).origin === location.origin; }
    catch (e) { return false; }
  }

  // 목적지 고정 저장: ?next=가 있으면 그걸 우선
  function rememberReturn() {
    const qs = new URLSearchParams(location.search);
    let dest = qs.get('next') || qs.get('redirect');

    // 보안: 절대 URL이면 동일 출처만 허용, 상대경로는 허용
    if (dest) {
      try {
        const u = new URL(dest, location.origin);
        if (u.origin !== location.origin) dest = null;
        else dest = u.pathname + u.search + u.hash; // 절대→상대화
      } catch (e) { dest = null; }
    }

    // next가 없으면 referrer로 보강 (동일 출처 & login/logout 제외)
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

  function init() {
    const id = window.netlifyIdentity;
    if (!id) return;

    rememberReturn();

    // 버튼 → 위젯 열기
    const btn = document.getElementById('login-btn');
    if (btn) btn.addEventListener('click', function () { id.open('login'); });

    // 이미 로그인 상태로 /ko/login/ 들어온 경우 자동 복귀
    id.on('init', function (user) {
      if (user) {
        const dest = pickDest();
        sessionStorage.removeItem('afterLogin');
        location.replace(dest);
      }
    });

    // 로그인 성공 → 토큰 갱신 → 닫기 → 복귀
    id.on('login', function () {
      (id.refresh ? id.refresh() : Promise.resolve()).finally(function () {
        id.close();
        const dest = pickDest();
        sessionStorage.removeItem('afterLogin');
        location.replace(dest);
      });
    });

    id.on('logout', function () { location.reload(); });

    id.init();
  }

  document.readyState === 'loading'
    ? document.addEventListener('DOMContentLoaded', init)
    : init();
})();
</script>