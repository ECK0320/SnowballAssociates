---
url: "/login/"
layout: "single"
build:
 render: always
 list: never
hidemeta: true
---

<div style="text-align: center;">

<button class="custom-button" onclick="netlifyIdentity.open('signup')">로그아웃</button>
<button class="custom-button" onclick="netlifyIdentity.open('logout')">로그아웃</button>

</div>

<script>
  if (window.netlifyIdentity) {
    window.netlifyIdentity.off("login", function(user) {
      window.location.href = "/ko/login/";
    });
  }
</script>