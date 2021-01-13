---
title: 'Crawl the web with Puppeteer Part.1'
excerpt: 'Puppeteer 살펴보기'
toc: true
toc_sticky: true
header:
  teaser: /assets/images/bio-photo-keyboard-teaser.jpg

categories:
  - Puppeteer
tags:
  - Typescript
  - Puppeteer
  - Nodejs
  - Crawler
last_modified_at: 2021-01-12
---

# 웹 크롤링

웹 크롤링이란 웹상에 존재하는 데이터를 수집하기 위한 하나의 방법입니다. 크롤러는 여러가지 경우와 이유로 사용하겠지만 저는 아래의 문제들을 해결하기 위해 크롤러를 만들었습니다.

1. 포털,커뮤니티,SNS 데이터 수집용(업무)
2. 나 또는 지인이 사고 싶은 키보드가 생겼는데 재고가 없어 재입고를 확인해야하는데 몹시 귀찮음.
3. 사내 업무등록 시스템에 업무를 일일이 등록하고 수행버튼 누르기가 상당히 귀찮음.

# Headless Browser ?

GUI 환경이 아니여도 사용할 수 있는 브라우저. 즉, Ubuntu등의 CUI환경에서 사용하기 위한 웹 브라우저.

# Puppeteer

[Puppeteer](https://github.com/puppeteer/puppeteer)는 Chrome또는 Chromium을 제어하기 위한 API들을 제공하는 Nodejs 라이브러리입니다.
Python에서는 Selenium이라는 라이브러리를 사용해서 크롤링 할 수 있듯 Nodejs에서는 Puppeteer를 사용한 웹 크롤링이 가능합니다. 크롤링을 할 수 있는 방법은 여러가지가 존재하지만, 이 포스팅에서는 Puppeteer에 대해 살펴봅니다.

## 1. 브라우저, 페이지 생성

아래 코드는 브라우저와 페이지를 각각 생성하고, 이벤트 리스너를 등록하는 예제입니다.

```ts
import Puppeteer, { Page, Browser, Dialog } from 'puppeteer';

async function create(): Promise<void> {
  // 브라우저 런치
  const browser: Browser = await Puppeteer.launch({
    // 이 외에도 다양한 옵션이 존재
    // userAgent, viewPort, args 등등..
    headless: true,
  });

  // 새 페이지 생성
  const page: Page = await browser.newPage();

  // 종종 다이얼로그로 인해 다음 단계를 진행하지 못하는 경우가 발생하는데
  // 다이얼로그 이벤트 리스너를 등록하여 해결할 수 있다.
  page.on('dialog', (dialog: Dialog) => {
    console.log(`MESSAGE: ${dialog.message()} / TYPE: ${dialog.type()}`);
    dialog.dissmiss();
  });

  return;
}
```

## 2. 페이징

페이징을 너무 빈번하게 처리할 경우 디도스공격이나 봇으로 인식하여 IP를 막아버리는 경우가 발생할 수 있다.
웹 크롤링을 할 땐 적당한 인터벌을 주는것이 안정성면에서 좋다(일종의 상도덕이라는 포스팅도 있다). 각 포털, 커뮤니티마다 기준이 다르니, 실험을 통해서 최소 인터벌을 알아내야한다.

### 1.goto()

가장 기본적인 페이징 방법은 goto를 사용하는 방법이 있다.
페이징 쿼리를 사용하는 대형포털등은 url를 잘 살펴보면 페이징 토큰이 있는데(예: &page=1) 이걸 잘 사용하는게 제일 베스트.

```ts
// URL로 이동후 페이지가 완전히 로딩되면 3000ms 동안 대기.
await page.goto('이동할 대상의 URL', { waitUntil: 'networkidle2' });
await page.waitFor(3000);
```

### 2. click()

추천하지는 않지만 불가피할 경우 직접 클릭하여 페이징처리할 수 있다.
왜때문인지, 3.0.0 버전에서는 button tag가 아닌 경우엔 정상적으로 click되지 않았는데 이후 버전에서는 해결이 됐는지 아직 모르겠다.

```ts
// 페이징 button tag selector를 직접 클릭 후 3000ms 동안 대기.
// 클릭 후 로딩되는 페이지에서 수집할 데이터의 셀렉터가 나타날 때 까지 대기해야 정상적으로 페이징됐는지 확인할 수 있다.
await page.click(`BUTTON_SELECTOR`);
await page.waitForSelector(`클릭후_로딩되는_페이지에서_수집할_데이터_SELECTOR`);
await page.waitFor(3000);
```

### 3. Scroll

퍼펫티어는 공식적으로 스크롤을 지원해주지 않는다. 그래서 브라우저의 window 객체를 끌어와 직접 구현해야한다.
스크롤로직을 구현하면서 scrollTo와 scrollBy 두 개를 확인했는데, scrollTo는 절대적 위치값을 사용하고 scrollBy는 상대적 위치값을 사용한다는 차이가 있다. 스크롤은 맨 바닥까지 내려가야하므로 고정값보단 상대값으로 해결하는게 편하다.

```ts
// 스크롤 후 3000ms 동안 대기
await page.evalute(
  'window.scrollBy({ top: window.innerHeight * 10, left: 0 })'
);
await page.waitFor(3000);
```

<br/>
<br/>
<br/>
<br/>
다음 포스팅에선 본격적으로 데이터를 크롤링하는 로직을 살펴보겠습니다.
