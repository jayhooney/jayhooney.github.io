---
title: "Crawl the web with Puppeteer Part.3"
excerpt: "Puppeteer 살펴보기"
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
last_modified_at: 2021-01-14
---

## 1. CUI 환경에서 겪는 흔한 이슈들

### Failed to launch chrome

해당 이슈는 Ubuntu OS에 웹 크롤러를 배포하면서 겪었던 이슈입니다. Nodejs와 Puppeteer를 모두 정상적으로 설치했어도, 운영체제에 헤드리스 브라우저를 구동하기 위한 필수 라이브러리가 없기 때문에 발생한 문제이다. 브라우저 런칭 옵션에 args를 몇 개 추가하고, 간단한 명령어로 필요 라이브러리들을 설치하여 해결할 수 있다.

```ts
const browser: Browser = await Puppeteer.launch({
  headless: true,
  // 아래 args 옵션을 추가한다.
  args: ["--no-sandbox", "--disable-setuid-sandbox"],
});
```

```shell
sudo apt-get install -y gconf-service libasound2 libatk1.0-0 libc6 libcairo2 libcups2 libdbus-1-3 libexpat1 libfontconfig1 libgbm-dev libgcc1 libgconf-2-4 libgdk-pixbuf2.0-0 libglib2.0-0 libgtk-3-0 libnspr4 libpango-1.0-0 libpangocairo-1.0-0 libstdc++6 libx11-6 libx11-xcb1 libxcb1 libxcomposite1 libxcursor1 libxdamage1 libxext6 libxfixes3 libxi6 libxrandr2 libxrender1 libxss1 libxtst6 ca-certificates fonts-liberation libappindicator1 libnss3 lsb-release xdg-utils wget
```

## 2. 불필요 요소들 걸러내기

Puppeteer는 goto등의 request가 발생할 때 불필요 요소들을 이벤트 리스너를 등록하여 걸러낼 수 있다.

```ts
// 요청 인터셉트 옵션 활성화
await page.setRequestInterception(puppeteerConf.IS_INTERCEPT);

page.on("request", (intercept) => {
  // 이미지 파일 걸러내기
  if (intercept.resourceType() === "image") {
    intercept.abort();
  } else {
    intercept.continue();
  }
});
```

## 3. User-Agent

User-Agent를 설정하지 않고 크롤링하다보면, 봇으로 판단하여 접속을 막아버리는 경우가 종종 있다.
이 경우에도 간단하게 코드로 User-Agent를 설정하여 해결할 수 있다. User-Agent는 다양하게 설정할 수 있다.

```ts
await page.setUserAgent(
  "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/52.0.2743.116 Safari/537.36"
);
```
