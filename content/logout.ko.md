---
url: "/ko/logout/"
layout: "single"
build:
 render: always
 list: never
hidemeta: true
---

<div style="text-align: center;">

<button class="custom-button" onclick="netlifyIdentity.open('login')">로그아웃</button>

</div>

<script>
  if (!window.netlifyIdentity) {
    window.netlifyIdentity.on("login", function(user) {
      window.location.href = "/ko/login/";
    });
  }
</script>