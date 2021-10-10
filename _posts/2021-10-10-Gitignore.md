---
title: 'gitignore.io'
excerpt: 'gitignore.io로 쉽게 .gitignore 생성하기'
toc: true
toc_sticky: true
header:
    teaser: /assets/image/gitignore.png
categories:
    - Git
tags:
    - gitignore
    - gitignore.io
last_modified_at: 2021-10-10
---

## .gitignore ?
github, gitlab등의 형상관리를 사용하는 사람이라면 .gitignore에 대해 잘 알것이다.
프로젝트마다 캐시라던가, 빌드등은 딱히 버전관리를 하지 않아도 되는데, 이 때 .gitignore 파일에 등록해 사용하면 해당 디렉토리,파일은 관리하지 않게 된다.

## 불-편
그런데, 나 같이 운영체제는 mac과 window를 자주 오간다거나, 코드 편집기로 vscode,intellij를 사용하다보면 가끔씩 뭐 하나를 .gitignore에 등록하는 걸 까먹을 때가 있다.
이럴 때 마다 .gitignore를 다시 수정해 푸시하고.. 귀찮고 불편한 작업을 해야한다. (불-편 😂)
이럴 때 실수를 최대한 줄일 수 있게끔 내 프로젝트 환경에 맞춰 .gitignore에 어떤 내용들이 들어가야하는지 아주 쉽게 만들어주는 웹 서비스가 있다. 이걸 한 번 소개해보겠다.

## gitignore.io
[gitignore.io](https://gitignore.io) 바로 이 서비스이다. 
링크를 통해 접속하면 https://www.toptal.com/developers/gitignore 라는 주소로 리다이렉트 되는데 똑같은 서비스이다.(몰랐는데 이제 정식 한국어 지원도 해준다👍)

![gitignore.io 진입 화면](/assets/image/gitignoreio_intro.png)
<div style="text-align: center; font: caption; color: #3d4144">gitignore.io 진입 화면</div>
<br>

이제, 내 프로젝트 환경에 맞는 키워드들만 차곡 차곡 빼먹지 않고 정성을 담아 입력해주면 된다.
지금 나같은 경우엔 주로 mac과 window 운영체제 환경에서 intellij를 사용해 spring boot(with gradle) 개발을 하고 있으니 아래와 같이 키워드를 설정했다.

![gitignore.io keyword 입력화면](/assets/image/gitignore_keyword_input.png)
<div style="text-align: center; font: caption; color: #3d4144">키워드 입력 화면</div>
<br>

그리고 우측의 연두색 '생성'버튼을 클릭하면!! 아래와 같은 결과를 확인할 수 있다.
![gitignore.io 결과화면](/assets/image/gitignore_result.png)
<div style="text-align: center; font: caption; color: #3d4144">결과 화면(너무 길어 일부만 캡쳐)</div>
<br>

이 내용들을 그대로 복사해 .gitignore에 붙여 넣기만 하면 끝난다! 편하지 않은가 😂👍


그럼 모두 행복한 git생활 하시길 👋





