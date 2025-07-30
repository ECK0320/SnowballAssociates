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

접근이 제한된 콘텐츠입니다.

가입 시 사용한 닉네임과 이메일 주소를 작성해 <a href="mailto:snowballassociates@gmail.com">인증 요청</a> 후 로그인 해주세요.

</div>

<script>
  if (window.netlifyIdentity) {
    window.netlifyIdentity.on("login", function(user) {
      window.location.href = "/ko/logout/";
    });
  }
</script>