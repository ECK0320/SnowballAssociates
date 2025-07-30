---
url: "/logout/"
layout: "single"
build:
 render: always
 list: never
hidemeta: true
---

<div style="text-align: center;">

<div style="text-align: center;">

<button class="custom-button" onclick="netlifyIdentity.open('login')">Log Out</button>

</div>

<script>
  // Netlify Identity 위젯이 로드된 후에 실행될 코드를 보장합니다.
  if (window.netlifyIdentity) {
    window.netlifyIdentity.on("init", function(user) {
      // Netlify Identity가 초기화되었을 때 user 객체가 null이면 로그인되지 않은 상태입니다.
      if (!user) {
        // 현재 경로가 이미 /ko/login/이 아니라면 리디렉션합니다.
        // 무한 리디렉션 루프를 방지합니다.
        if (window.location.pathname !== "/login/") {
          window.location.href = "/login/";
        }
      }
    });
  }
</script>