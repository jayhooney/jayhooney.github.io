---
title: 'RVM과 RBenv'
excerpt: '뭐가 다를까..'
toc: true
toc_sticky: true
header:
    teaser: /assets/image/rvm&rbenv.png
categories:
    - Ruby
tags:
    - Ruby 
    - Ruby Version Manager
    - RVM
    - RBenv
last_modified_at: 2021-11-27
---

## 생각좀 해라 ㅠㅠ 
이직한 회사에서 Ruby on Rails 온보딩 가이드 문서를 보며 생각 없이(이때 정신 차리고 의심했어야 했다.) RVM이랑 RBenv를 섞어 썼더니 원활하게 서버를 띄우지 못했다.
그래서 이번 기회에 두 개의 차이를 좀 알아보고, RVM 기준으로 작성된 현재 온보딩 가이드 문서에 RBenv도 추가해 봐야겠다.
이 둘 외에도 chruby와 기타 등등 더 있지만 해당 포스트에선 RVM과 RBenv를 비교한다. 설치 관련 포스트는 다음에 작성할 예정!


## RVM / RBenv 저울질 해보기 

|⚖️|RVM|RBenv|
|:---:|:---|:---|
|장점|다양한 기능들을 제공한다.<br />gemset 관리 기능을 포함한다.|현재 가장 많이 쓰이며 레퍼런스가 많다.<br />루비 버전 변경이 쉽다.|
|단점|제공 기능이 많은 만큼 무겁다.<br />shell environment를 꼬이게 하는 경우가 종종 발생한다.|shim 방식을 사용하기 때문에 ruby를 직접 사용하는 것보단 다소 느리다.<br />새로운 버전의 Ruby를 설치하면 rbenv rehash 명령어를 통해 제대로 shim 되도록 처리해 줘야 한다.|

여러 포스팅과 공식 문서를 보면서 각각 장단점을 정리해 보니 위와 같다.
그리고, RVM이냐 RBenv냐! 갑론을박하는 포스팅들을 살펴보면 크게 프로젝트별로 루비 버전을 분리해 써야 하는 경우가 대부분이었다.
RVM은 gemset을 지원해 줘서 프로젝트별로 루비 버전을 분리해 사용할 수 있지만 RBenv는 그때 그때 루비 버전을 변경해 줘야 하지만 이 과정이 크게 복잡하거나 어렵지 않다.
개인적인 생각으론 루비 버전 변경이 불가능한 것만 아니라면 훨씬 가벼운 RBenv가 나아 보인다.(성능 차이는 도긴개긴이다.) 

> 부록
> 
> [RBenv에서 설명하는 RVM대신 RBenv를 사용해야하는 이유](https://github.com/rbenv/rbenv/wiki/Why-rbenv%3F)



## 참고문서
- [어떤 루리 버전 관리자가 좋을까?](https://www.slideshare.net/hosangjeon10/ruby-version-managers)
- [RVM github](https://github.com/rvm/rvm)
- [RBenv github](https://github.com/rbenv/rbenv)
- [RBenv 작동원리](https://withrails.com/2015/11/25/rbenv-%EC%9E%91%EB%8F%99%EC%9B%90%EB%A6%AC/)
- [RBenv 설치](https://kbs4674.tistory.com/187)