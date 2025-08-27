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

<script src="https://identity.netlify.com/v1/netlify-identity-widget.js"></script>
<script>
  const btn = document.getElementById('login-btn');
  btn?.addEventListener('click', () => {
    netlifyIdentity?.open('login');
  });

  netlifyIdentity?.on('login', () => {
    const next = new URLSearchParams(location.search).get('next') || '/ko/members/';
    location.replace(next);
  });
</script>