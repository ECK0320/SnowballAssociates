---
url: "/ko/login/"
layout: "single"
build:
 render: always
 list: never
hidemeta: true
---

<div style="text-align: center;">

<button class="custom-button" onclick="netlifyIdentity.open('login')">로그인</button>

회원 공개 글입니다.

로그인 또는 무료 회원가입 후 진행해주세요.

</div>

<script>
  if (window.netlifyIdentity) {
    window.netlifyIdentity.on("login", function(user) {
      window.location.href = "/ko/logout/";
    });
  }
</script>