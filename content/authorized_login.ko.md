---
url: "/ko/authorized_login/" # 이웃 (모든 접근 허용), 최우수회원 ('일기' 접근 불가), 우수회원 ('일기', '長考' 접근 불가); 회원 ('일기', '長考', '小考' 접근 불가)
layout: "single"
build:
 render: always
 list: never
hidemeta: true
---

<div style="text-align: center;">

<button class="custom-button" onclick="netlifyIdentity.open('login')">로그인</button>

회원 등급에 따라 접근이 제한된 콘텐츠입니다.

해당 등급이 이미 부여되었음에도 접속이 안 되실 경우, 가입 시 사용한 닉네임과 이메일 주소를 <a href="mailto:snowballassociates@gmail.com">관리자</a>에게 보내주시면 72시간 내로 처리해드리겠습니다.

</div>

<script>
  if (window.netlifyIdentity) {
    window.netlifyIdentity.on("login", function(user) {
      window.location.href = "/ko/logout/";
    });
  }
</script>