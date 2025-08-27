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
  (function () {
    function init() {
      if (!window.netlifyIdentity) return;
      document.getElementById('login-btn')?.addEventListener('click', function () {
        window.netlifyIdentity.open('login');
      });
      window.netlifyIdentity.on('login', function () {
        window.netlifyIdentity.close();
        // 현재 페이지 그대로 새로고침(리다이렉트 아님)
        window.location.reload();
      });
    }
    if (document.readyState === 'loading') {
      document.addEventListener('DOMContentLoaded', init);
    } else {
      init();
    }
  })();
</script>