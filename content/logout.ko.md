---
url: "/ko/logout/"
layout: "single"
build:
 render: always
 list: never
hidemeta: true
---

<div style="text-align: center;">

<button class="custom-button" id="logout-button">로그아웃</button>

</div>

<script src="https://identity.netlify.com/v1/netlify-identity-widget.js"></script>

<script>
(function () {
  function sameOrigin(u) {
    try { return new URL(u, location.origin).origin === location.origin; }
    catch (e) { return false; }
  }

  // 로그인 후 돌아갈 목적지 기억하기
  function rememberReturn() {
    const qs = new URLSearchParams(location.search);
    // 1) /ko/login/?next=/ko/some/private/page 처럼 넘겨온 경우 우선
    let dest = qs.get('next') || qs.get('redirect');

    // 2) 없으면 referrer가 동일 도메인 & login/logout이 아니면 그걸 사용
    if (!dest && sameOrigin(document.referrer)) {
      const ref = new URL(document.referrer);
      if (!/\/ko\/login\/|\/ko\/logout\//.test(ref.pathname)) dest = ref.href;
    }

    // 3) 있으면 세션스토리지에 저장
    if (dest) sessionStorage.setItem('afterLogin', dest);
  }

  function init() {
    const id = window.netlifyIdentity;
    if (!id) return;

    rememberReturn();

    // 로그인 성공 → 쿠키 갱신 → 위젯 닫기 → 원래 페이지로 이동
    id.on('login', function () {
      id.refresh().then(function () {
        id.close();
        const dest = sessionStorage.getItem('afterLogin') || '/';
        sessionStorage.removeItem('afterLogin');
        location.replace(dest); // history에 login 안 남게 replace
      });
    });

    // 로그아웃 → 현재 페이지 권한 상태로 새로고침
    id.on('logout', function () {
      location.reload();
    });

    id.init();
  }

  if (document.readyState === 'loading') {
    document.addEventListener('DOMContentLoaded', init);
  } else {
    init();
  }
})();
</script>