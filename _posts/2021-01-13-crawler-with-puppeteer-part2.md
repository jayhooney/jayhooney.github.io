---
title: 'Crawl the web with Puppeteer Part.2'
excerpt: 'Puppeteer 살펴보기'
toc: true
toc_sticky: true
header:
  teaser: /assets/image/web_crawling_with_puppeteer.png

categories:
  - Puppeteer
tags:
  - Typescript
  - Puppeteer
  - Nodejs
  - Crawler
last_modified_at: 2021-01-13
---

## 1. 크롤링

puppeteer에서는 &#36;() / &#36;eval() / &#36;&#36;() / &#36;&#36;eval() 함수들을 사용해 크롤링할 수 있습니다. 각각의 차이는 아래에서 설명합니다.

### $(), $$() 의 차이

&#36;()는 단 하나의 ElementHandle을, &#36;&#36;()는 하나 이상의 ElementHandle들을 배열로 반환한다.
ElementHandle은 $또는 $$로 반환받은 객체의 핸들러이며, 이 핸들러에서 바로 데이터를 추출할 수 있다면 좋겠지만 불가능하다.
그래서 &#36;()나 &#36;&#36;()는 부모 Node를 캐싱해둬야할 때 적합하다. 예를들어 수집대상 웹 사이트에서 iframe을 사용하는 경우가 해당된다.

```ts
// $를 사용한 iframe Node 캐싱
const iframeHandle: ElementHandle<Element> = await page
  .$('iframe_selector')
  .contentFrame();
```

### $eval(), $$eval()의 차이

위와 마찬가지지만, &#36;, &#36;&#36;와 차이점은 evaluate()가 내장됐는가 이다.
evaluate()가 내장된 &#36;eval(), &#36;&#36;eval()는 핸들러를 반환하지 않고 Element를 Callback으로 전달해주며, 이 안에서 원하는 데이터를 추출,반환해야한다.

```ts
// 위에서 캐싱한 iframeHandle에서 $eval을 사용한 데이터 추출
const content: string = await iframeHandle.$eval('selector', (e: Element) => {
  return e.innerText;
});

// 위에서 캐싱한 iframeHandle에서 $$eval을 사용한 데이터 추출
const contentArr: string[] = await iframeHandle.$$eval(
  'selector',
  (eArr: Element[]) => {
    const content: string[] = [];
    for (let e of eArr) {
      content.push(e.innerText);
    }

    return content;
  }
);
```

## 2. 종료

원하는 데이터들을 다 추출하였다면, 브라우저를 종료해줘야한다. 웹브라우저는 생각보다 메모리를 많이 사용하므로 제대로 종료해주는것이 시스템 리소스를 잘 활용하는데 있어 중요하다.

```ts
await browser.close();
```

<br/>
<br/>
<br/>
<br/>
다음 포스팅에선 Puppeteer를 사용하면서 겪었던 이슈들에 대해 정리해보겠습니다.
