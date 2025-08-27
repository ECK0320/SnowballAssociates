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
    const id = window.netlifyIdentity;
    if (!id) return;

    // 로그인 버튼
    document.getElementById('login-btn')?.addEventListener('click', () => {
      id.open('login');
    });

    // 쿼리로 next(또는 redirect) 넘어오면 저장해뒀다가 로그인 후 복귀
    const qs = new URLSearchParams(location.search);
    const next = qs.get('next') || qs.get('redirect');
    if (next) sessionStorage.setItem('afterLogin', next);

    id.on('login', function () {
      // nf_jwt 쿠키를 확실히 굽도록 refresh한 뒤
      id.refresh().then(function () {
        id.close();
        // 원래 가려던 곳이 있으면 거기로, 없으면 현재 페이지 그대로 재요청
        const dest = sessionStorage.getItem('afterLogin') || location.href;
        sessionStorage.removeItem('afterLogin');
        location.replace(dest);
      });
    });

    id.on('logout', function () {
      // 로그아웃 후엔 게스트 뷰로 갱신
      location.reload();
    });

    id.init();
  }

  if (document.readyState === 'loading') {
    document.addEventListener('DOMContentLoaded', init);
  } else {
    init();
  }
})();
</script>
