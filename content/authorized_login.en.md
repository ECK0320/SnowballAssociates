---
url: "/authorized_login/" # 이웃 (모든 접근 허용), 최우수회원 ('일기' 접근 불가), 우수회원 ('일기', '長考' 접근 불가); 회원 ('일기', '長考', '小考' 접근 불가)
layout: "single"
build:
 render: always
 list: never
hidemeta: true
---

<div style="text-align: center;">

<button class="custom-button" onclick="netlifyIdentity.open('login')">Log In</button>

This content is restricted to authorized users only.

If you believe you should have access, please contact <a href="mailto:snowballassociates@gmail.com">the administrator</a> with your registered email address for verification. Your inquiry will be processed within 72 hours.

</div>

<script>
  if (window.netlifyIdentity) {
    window.netlifyIdentity.on("login", function(user) {
      window.location.href = "/logout/";
    });
  }
</script>