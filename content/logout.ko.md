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
  // Netlify Identity 위젯이 로드되었는지 확인합니다.
  if (window.netlifyIdentity) {
    // 로그아웃 이벤트 리스너: 로그아웃이 성공적으로 완료되면 자동으로 호출됩니다.
    window.netlifyIdentity.on("logout", function() {
      // 로그아웃 완료 시 즉시 /login/ 페이지로 리디렉션
      window.location.href = "/ko/login/";
    });

    // HTML 버튼 클릭 시 Netlify Identity의 로그아웃 함수를 호출하도록 연결합니다.
    const logoutButton = document.getElementById('logout-button');
    if (logoutButton) {
      logoutButton.addEventListener('click', function() {
        // 실제 로그아웃을 실행하는 함수 호출
        window.netlifyIdentity.logout();
      });
    }
  }
</script>