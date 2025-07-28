---
categories: ["framework"]
subcategories: ["numbers"]
title: "Ubuntu LTS 24.04에 texlive-full 설치 시 오류 해결 2"
description: "feat. luahbtex 호환성 버그" #필요하다면 추가
studies: ["STEM"] # 해당 분야 선택 (이공학)
tools: ["LaTeX", "Hugo"] # 사용된 프로그래밍 언어 & 패키지
skills: [""] # 발행 전 AI 돌려서 quantitative/qualitative/technical/academic skillset 추출하기
tags: ["Cognitive Framework", "Linux", "Ubuntu LTS 24.04"] # 세부 분야: book report, lecture, class, data science, data analytics, mathematics, statistics, Python, R, SQL, Linux, Ubuntu, DB, algorithm, ML, AI, LaTeX, Hugo
---

## 1. 우분투 LTS 24.04 TexLive 설치 오류 문제 {#1}
LTS 24.04.2 기준, `sudo apt install texlive-full`로 LaTeX 설치 시도시 한국 서버에서 다운로드가 안 된다는 오류가 발생함.

내 에러 로그는:

> E: Failed to fetch http://kr.archive.ubuntu.com/ubuntu/pool/universe/b/biber/biber_2.19-2_all.deb  403  Forbidden [IP: 210.117.237.2 80]
> E: Failed to fetch http://kr.archive.ubuntu.com/ubuntu/pool/universe/l/latexmk/latexmk_4.83-1_all.deb  403  Forbidden [IP: 210.117.237.2 80]
> E: Unable to fetch some archives, maybe run apt-get update or try with --fix-missing?

<br>

### 1-1. 미러 서버를 글로벌 서버로 변경 {#1-1}
```bash
sudo sed -i 's|kr.archive.ubuntu.com|archive.ubuntu.com|g' /etc/apt/sources.list
```
→ 안 됨

### 1-2. 로컬 캐시 초기화 후 APT 인덱스 재설정 {#1-2}
```bash
sudo rm -rf /var/lib/apt/lists/*
sudo apt clean
sudo apt update
```
→ 안 됨

### 1-3. 손상된 패키지 재설치 {#1-3}
```bash
sudo apt upgrade --fix-missing
```
→ 안 됨

### 1-4. texlive-full 로 전체 설치 대신 필요한 부분만큼만 재설치 {#1-4}
```bash
sudo apt install texlive-base texlive-latex-base texlive-latex-recommended texlive-fonts-recommended texlive-lang-korean texlive-latex-extra texlive-fonts-extra texlive-bibtex-extra latexmk biber
```
→ ~~**됨!**~~ **되는 줄 알았음!**

위 패키지만으로도 보고서와 학술 논문 쓰는데는 아무 지장 없으니 굳이 7G에 육박하는 texlive 전체를 다운받을 필요는 없다. 특히 나처럼 VSC로 레이텍을 작성하는 경우라면 더더욱.

*추신: 직접 tar.xz 파일 다운 받아서 오프라인 설치도 실행해봤었다. 총 1시간 40분 정도 걸렸는데 마지막에 또 크래시가 떴다. 왠지 미러서버의 문제가 아니라 호환성 문제 같아서 그냥 설치 켜놓고 잠들었는데 현명한 선택이었다. 설치 내내 기다리다가 오류 떴으면 화나서 잠을 못 잤을듯.*

*\*추신2: 웹사이트에서 다운받지 말고 `wget`으로 최신 버전 다운받는 걸 권장한다. [아래 내용](#3) 참조.*

<br>

## 2. LTS 24.04와 XeTeX, Luahbtex 호환성 버그 오류 {#2}

이제 편히 쓰면 되나 싶었는데 계속 크래시 리포트가 뜬다.

![LTS 24.04 luahbtex crash](https://i.imgur.com/nyat4xs.png)

2024년 3월에 보고된 texlive-binaries 에서 발생하는 luahbtex stack smashing crash 인데 ([Debian Bug #1067576](https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1067576)), 2025.7.27. 현 작성 시점 기준으로 아직도 고쳐지지 않은 상태다.

혹시 몰라서 `sudo apt --reinstall install texlive-binaries`로 패키지 바이너리를 복구하려고 했는데 이 과정에서 또 오류가 발생.

![LTX 24.04 xetex crash](https://i.imgur.com/OdmqUDy.png)

**SIGSEGV (Segmentation Fault)**: 잘못된(없는) 가상 메모리 영역(Virtual Memory Area)를 dereference 하는 오류.

luahbtex뿐 아니라, texlive 엔진 자체가 segmentation fault/stack smashing/illegal instruction 등의 이유로 계속 crash 나는 이슈가 다수 리포트 되었다고 함.

<br>

## <mark>3. sudo apt install 대신 공식 리포지토리에서 클린 설치</mark> {#3}

### 3-1. 시스템 라이브러리/패키지 전체 검사·복구 (*특히 <u>segmentation 오류</u> 발생할 경우*) {#3-1}
```bash
sudo apt update
sudo apt upgrade
sudo apt --fix-broken install
sudo apt install --reinstall libc6 perl
sudo apt autoremove
sudo apt clean
```

### 3-2. 캐시/잔여 인스톨러 파일 폴더 완전 삭제 {#3-2}
```bash
rm -rf ~/Downloads/install-tl-unx
```

### 3-3. 최신 버전 새로 다운로드 {#3-3}
```bash
wget http://mirror.ctan.org/systems/texlive/tlnet/install-tl-unx.tar.gz
tar -xzf install-tl-unx.tar.gz
cd install-tl-*
sudo ./install-tl -repository http://ctan.math.illinois.edu/systems/texlive/tlnet
```

### 3-4. 필요 사양만 최소 설치 {#3-4}

![LTS 24.04.2 TeX Live repository - official install](https://i.imgur.com/N68YbSL.png) [S]: 커스텀 설치 진입 <br><br>

![LTS 24.04.2 TeX Live repository - medium install](https://i.imgur.com/PfFy3Ck.png) [B]: 미디엄 모드 선택 [R]: 해당 옵션으로 설치 선택 <br><br>

![LTS 24.04.2 TeX Live repository - medium install](https://i.imgur.com/prgMtkB.png) [I]: 설치 시작 (Full: 8.9G → Medium: 2.3G) <br><br>

![LTS 24.04.2 TeX Live repository - medium install](https://i.imgur.com/m2Pi4dg.png) 37분 55초가 걸린 후 설치가 완료됐다. <mark> texlive-full 버전을 설치할 때와 다르게, medium 설치는 fmtutil.cnf 또는 기타 바이너리 패키지와 아무 충돌이 없다.</mark>


## 4. 환경변수(PATH)에 경로 등록 {#4}
환경변수 영구 적용:

```bash
sudo nano .bashrc
export PATH=/usr/local/texlive/2025/bin/x86_64-linux:$PATH
```
위 내용을 `.bashrc`에 입력 후 저장(`CTRL + X` → `Y` → `ENTER`)한 다음
<br>

```bash 
source ~/.bashrc
```
설정 적용
<br>

```bash
echo $PATH
xelatex --version
luatex --version
luahbtex --version
pdflatex --version
tlmgr --version
```
확인
<br>

![LTS 24.04.2 TeXLive xelatex luatex pdflatex tlmgr](https://i.imgur.com/fg2ZRap.png) 환경변수에 잘 등록됐고, 처음에 `apt install texlive-full`로 설치했을 때는 계속 호환성 문제가 생기던 xetex, luatex, luahbtex (luatex의 확장판; 스크린샷에는 없음), tlmgr 전부 잘 설치된 것을 확인할 수 있다.