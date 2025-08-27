---
url: "/ko/logout/"
layout: "single"
build:
 render: always
 list: never
hidemeta: true
---

<div style="text-align: center;">

<button class="custom-button" id="logout-button">로그아웃</button>

</div>

<script src="https://identity.netlify.com/v1/netlify-identity-widget.js"></script>

<script>
  (function () {
    function init() {
      if (!window.netlifyIdentity) return;

      window.netlifyIdentity.on("logout", function () {
        // nf_jwt 쿠키 제거
        document.cookie =
          'nf_jwt=; Max-Age=0; Path=/; SameSite=Lax' +
          (location.protocol === 'https:' ? '; Secure' : '');
        // 로그아웃 시 메인 랜딩 페이지로
        window.location.replace("/ko/");
      });

      // 수동 로그아웃 버튼 연결 (있다면)
      const btn = document.getElementById('logout-button');
      if (btn) btn.addEventListener('click', function () {
        window.netlifyIdentity.logout();
      });

      window.netlifyIdentity.init();
    }

    document.readyState === 'loading'
      ? document.addEventListener('DOMContentLoaded', init)
      : init();
  })();
</script>