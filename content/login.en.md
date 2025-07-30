---
url: "/login/"
layout: "single"
build:
 render: always
 list: never
hidemeta: true
---

<div style="text-align: center;">

<button class="custom-button" onclick="netlifyIdentity.open('login')">Log In</button>

This content is restricted to authorized users only.

Should you had access, please contact <a href="mailto:snowballassociates@gmail.com">the administrator</a> through your email address used for sign-up.


</div>

<script>
  if (window.netlifyIdentity) {
    window.netlifyIdentity.on("login", function(user) {
      window.location.href = "/logout/";
    });
  }
</script>