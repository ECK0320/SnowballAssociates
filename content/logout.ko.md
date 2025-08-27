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
    const goLogin = () => window.location.replace('/ko/login/');

    if (!window.netlifyIdentity) {
      // 위젯이 없으면 그냥 로그인 페이지로
      goLogin();
      return;
    }

    // 위젯 초기화 후 현재 상태 확인
    window.netlifyIdentity.on('init', function (user) {
      if (user) {
        // 로그인 상태면 바로 로그아웃 실행
        window.netlifyIdentity.logout();
      } else {
        // 이미 비로그인 상태면 곧장 로그인 페이지로
        goLogin();
      }
    });

    // 로그아웃 완료되면 로그인 페이지로
    window.netlifyIdentity.on('logout', function () {
      goLogin();
    });

    // 혹시 수동 버튼을 쓰고 싶다면 (지금은 display:none)
    const btn = document.getElementById('logout-button');
    if (btn) {
      btn.addEventListener('click', function () {
        window.netlifyIdentity.logout();
      });
    }
  })();
</script>