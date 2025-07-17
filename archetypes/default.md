---
date: {{ .Date }}
lastmod: {{ .Date }}  # 이 줄을 추가합니다
title: "{{ replace .Name "-" " " | title }}"
# description: "" # 필요하다면 추가
draft: true
# author: "ECK" # 필요하다면 추가
categories:
---