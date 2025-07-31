---
date: 2025-07-31 16:31:00+09:00
categories: ["framework"]
subcategories: ["numbers"]
title: "네이버 뉴스 기사 댓글 삭제 스크립트"
studies: ["STEM"] # 해당 분야 선택 (이공학)
tools: ["JavaScript"] # 사용된 프로그래밍 언어 & 패키지
skills: ["Web Automation", "Scripting", "Web Scraping", "Browser Developer Tools", "DOM Manipulation", "Automation Scripting", "Problem Solving", "Data Cleaning"] # 발행 전 AI 돌려서 링크드인 공식 카테고리 및 skill taxonomy 기준으로 핵심 quantitative/qualitative/technical/academic skill set 만 ["skill1", "skill2", ...] 1열 형태로 추출
tags: ["Cognitive Framework"] # 세부 분야: book report, lecture, class, data science, data analytics, mathematics, statistics, Python, R, SQL, Linux, Ubuntu, DB, algorithm, ML, AI, LaTeX, Hugo
---

몇년 동안 안 쓰던 네이버 아이디에 로그인 하니까 과거의 내가 댓글을 많이도 써놨더라.

하나하나 읽어보면서 선별적으로 지우다가 굳이 뉴스 댓글까지 검토하고 분석할 가치는 없는 것 같아 일괄 삭제하기로 했다.

2~30개쯤 수동으로 지우고 있자니 <span class="quote">"이건 뭔가 잘못됐다"</span>라는 생각이 강하게 들었다. 수백 개를 언제 다 지우고 있어.

GPT 4.1의 도움을 받아 간단한 자바스크립트 코드를 만들어봤다.

<br>

1. 네이버 로그인 후 아무 뉴스 기사나 들어가서 댓글을 하나 작성한다.

![](https://i.imgur.com/RPHktGb.png)

2. 본인의 '뉴스 댓글모음' 창을 띄워놓은 후 `F12`로 개발자 모드를 켜서 `Console`로 진입해 아래 코드를 복붙한 후 `ENTER`

```
window.confirm = () => true;
let commentList = [];
const getCommentList = () => commentList = [...document.querySelectorAll('#cbox_module_wai_u_cbox_content_wrap_tabpanel a.u_cbox_btn_delete')];
getCommentList();

const intervalId = setInterval(() => {
    if (!commentList.length) {
        const moreBtn = document.querySelector('#wa_allcomments .u_cbox_paginate.is_more_button > a');
        if (moreBtn) {
            moreBtn.click();
            getCommentList();
        } else {
            clearInterval(intervalId);
        }
    } else {
        commentList.shift().click();
    }
}, 500);
```

3. 작성 댓글의 목록 끝까지 다 지우고 나면 <span class="quote">"해당되는 댓글이 없습니다"</span>라는 알림이 두 번---<span class="append">현재 리스트 한 번, 전체 리스트 한 번</span>---연달아 뜬다. `OK` 두 번 눌러주고 새로고침하면 다 삭제돼있는 걸 확인할 수 있다.