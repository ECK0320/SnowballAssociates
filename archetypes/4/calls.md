---
author: "ECK" # 필요하다면 추가
date: {{ .Date }}
lastmod: {{ .Date }}  # 이 줄을 추가합니다
categories: ["market_decision_log"]
subcategories: ["calls"]
title: "{{ replace .Name "-" " " | title }}"
# description: "" 필요하다면 추가
recommendations: [""] # 예상 내역: BUY, SELL, HOLD
asset_classes: [""] # 매매 자산 종류: stock, fixed income, currency, commodities, cypto, futures, options, ETF
trading_tools: [""] # 매매 기법: fundamental analysis, quantitative analysis, technical analysis, event driven, financials, narratives
event_types: [""] # 사건 진원지: monetary policy, fiscal policy, international relations, global economy, domestic politics, domestic economy, industry, company
thought_facets: ["Intuition", "Analysis"] # 직관 or 분석
tools: ["Python", "R", "SQL"] # 사용된 프로그래밍 언어 & 패키지
skills: [""] # 발행 전 AI 돌려서 quantitative/qualitative/technical/academic skillset 추출하기
tags: [""] # forecast (예상), verification (검정), post-mortem analysis (사후 분석; 복기)
draft: true
---