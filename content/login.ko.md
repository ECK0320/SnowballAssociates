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

  // 로그인 후 돌아갈 목적지 기억
  function rememberReturn() {
    const qs = new URLSearchParams(location.search);
    let dest = qs.get('next') || qs.get('redirect');
    if (dest && !sameOrigin(dest)) dest = null; // 오픈 리다이렉트 방지

    if (!dest && document.referrer && sameOrigin(document.referrer)) {
      const ref = new URL(document.referrer);
      if (!/\/ko\/login\/|\/ko\/logout\//.test(ref.pathname)) dest = ref.href;
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

    // 위젯 열기
    const btn = document.getElementById('login-btn');
    if (btn) btn.addEventListener('click', function () { id.open('login'); });

    // 로그인 성공 → 토큰 갱신 → 위젯 닫기 → 목적지 이동
    id.on('login', function () {
      (id.refresh ? id.refresh() : Promise.resolve()).finally(function () {
        id.close();
        const dest = pickDest();
        sessionStorage.removeItem('afterLogin');
        location.replace(dest); // history에 로그인 페이지 남기지 않음
      });
    });

    // 로그아웃 → 현재 페이지 상태로 새로고침
    id.on('logout', function () { location.reload(); });

    id.init();
  }

  document.readyState === 'loading'
    ? document.addEventListener('DOMContentLoaded', init)
    : init();
})();
</script>
