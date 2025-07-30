---
url: "/login/"
layout: "single"
build:
 render: always
 list: never
hidemeta: true
---

<div style="text-align: center;">

<button class="custom-button" onclick="netlifyIdentity.open('login')">Log In</button> <button class="custom-button" onclick="netlifyIdentity.open('signup')">Sign Up</button> 

This content is restricted to members only.

Please log in or sign up for free to read the full article!

</div>

<script>
  if (window.netlifyIdentity) {
    window.netlifyIdentity.on("login", function(user) {
      window.location.href = "/logout/";
    });
  }
</script>