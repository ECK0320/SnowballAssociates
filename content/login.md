---
url: "/login/"
layout: "single"
_build:
  render: always
  list: never
hidemeta: true
---

<div style="text-align: center;">

<p data-netlify-identity-button></p>
<button onclick="netlifyIdentity.open('signup')">Sign Up</button> | 
<button onclick="netlifyIdentity.open('login')">Login</button>

접근이 제한된 콘텐츠입니다.
This content is restricted to authorized users.

</div>

<script>
  if (window.netlifyIdentity) {
    window.netlifyIdentity.on("login", function(user) {
      window.location.href = "/ko/judgment_philosophy/diary/";
    });
  }
</script>