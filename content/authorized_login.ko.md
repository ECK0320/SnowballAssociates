---
url: "/ko/authorized_login/" # 이웃 (모든 접근 허용), 최우수회원 ('일기' 접근 불가), 우수회원 ('일기', '長考' 접근 불가); 회원 ('일기', '長考', '小考' 접근 불가)
layout: "single"
build:
 render: always
 list: never
hidemeta: true
---

<div style="text-align: center;">

<button class="custom-button" onclick="netlifyIdentity.open('login')">로그인</button>

회원 등급에 따라 접근이 제한된 콘텐츠입니다.

해당 등급이 이미 부여되었음에도 접속이 안 되실 경우, 가입 시 사용한 닉네임과 이메일 주소를 <a href="mailto:snowballassociates@gmail.com">관리자</a>에게 보내주시면 72시간 내로 처리해드리겠습니다.

</div>

<!-- Netlify Identity widget -->
<script src="https://identity.netlify.com/v1/netlify-identity-widget.js"></script>

<script>
(function () {
  // 동일 출처만 허용 (오픈 리다이렉트 방지)
  function sameOrigin(u) {
    try { return new URL(u, location.origin).origin === location.origin; }
    catch (e) { return false; }
  }

  // 로그인 후 돌아갈 목적지 기억: ?next, ?redirect, 또는 same-origin referrer
  function rememberReturn() {
    const qs = new URLSearchParams(location.search);
    let dest = qs.get('next') || qs.get('redirect');
    if (dest && !sameOrigin(dest)) dest = null;

    if (!dest && document.referrer && sameOrigin(document.referrer)) {
      const ref = new URL(document.referrer);
      if (!/\/ko\/login\/|\/ko\/authorized-login\/|\/ko\/logout\//.test(ref.pathname)) {
        dest = ref.href;
      }
    }
    if (dest) sessionStorage.setItem('afterLogin', dest);
  }

  function pickDest() {
    return sessionStorage.getItem('afterLogin') || '/ko/';
  }

  // nf_jwt 쿠키 세팅/삭제
  function setJwtCookie(token, maxAgeSec) {
    document.cookie = `nf_jwt=${token}; Path=/; Max-Age=${maxAgeSec}; Secure; SameSite=Lax`;
  }
  function clearJwtCookie() {
    document.cookie = 'nf_jwt=; Path=/; Max-Age=0; Secure; SameSite=Lax';
  }

  function init() {
    const id = window.netlifyIdentity;
    if (!id) return;

    rememberReturn();

    // 위젯 열기
    const btn = document.getElementById('login-btn');
    if (btn) btn.addEventListener('click', function () { id.open('login'); });

    // 로그인 성공 → 토큰 갱신 & nf_jwt 설정 → 위젯 닫기 → 목적지 이동
    id.on('login', async function (user) {
      try {
        // 최신 토큰 확보 (일부 환경에서 refresh 필요)
        if (id.refresh) await id.refresh();
        const token = await user.jwt(); // 새 JWT
        // 1시간 정도로 예시 설정 (환경에 맞게 조정)
        setJwtCookie(token, 3600);
      } catch (e) {
        console.warn('jwt acquire failed:', e);
      } finally {
        id.close();
        const dest = pickDest();
        sessionStorage.removeItem('afterLogin');
        location.replace(dest);
      }
    });

    // 로그아웃 → 쿠키 제거 → 현재 페이지 새로고침
    id.on('logout', function () {
      clearJwtCookie();
      location.reload();
    });

    id.init();
  }

  document.readyState === 'loading'
    ? document.addEventListener('DOMContentLoaded', init)
    : init();
})();
</script>