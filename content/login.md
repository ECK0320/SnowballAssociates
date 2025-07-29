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

이 콘텐츠는 승인된 사용자에게만 공개됩니다.

</div>

<script>
  if (window.netlifyIdentity) {
    window.netlifyIdentity.on("login", function(user) {
      window.location.href = "/ko/judgment_philosophy/diary/";
    });
  }
</script>