---
title: 'Java stream API' 
excerpt: 'Java stream API 알아보기'
toc: true 
toc_sticky: true
header:
    teaser: /assets/image/java_stream.png
categories:
  - JAVA
tags:
  - JAVA
  - STREAM
last_modified_at: 2021-10-04
---
## Stream API란?
JDK 8에서 추가된 Stream API는 **데이터 소스를 추상화**하고, **데이터를 다루는데 자주 사용되는 메서드들을 정의**한 API다. 데이터 소스 추상화라던가, 다룬다던가 우선 막연하니 다짜고짜 코드부터 한 번
봐보자.

```java
// langs 리스트에 프로그래밍 언어들을 추가하고
// "J"를 포함한 언어를 별도 리스트에 분리하는 코드 

var langs = new ArrayList<String>();
langs.add("C");
langs.add("C++");
langs.add("C#");
langs.add("Java");
langs.add("JavaScript");
langs.add("TypeScript");

// stream 미적용
var filteredLangs = new ArrayList<String>();
for (var lang: langs) {

    if(lang.contains("J")){
        filteredLanguages.add(lang);
    }
}

// stream 적용
var filteredLangsWithStream = langs.stream()
                                    .filter(lang -> lang.contains("J"))
                                    .collect(Collectors.toList());
```

stream 미적용 코드엔 for, if를 사용해 "J"를 포함한 언어를 필터링해 새로운 리스트에 추가하고, stream 적용 코드엔 **List에 stream을 사용**해 filter 메서드로 **데이터를 처리**
했다. stream은 List뿐만 아니라 Array나 다른 Collection(Map, Set 등)에도 사용할 수 있는데 이는 **데이터 소스(Array, Collections)를 추상화**해놨기 때문에 어떤
자료구조에도 사용할 수 있는것이다. filter뿐만 아니라 다양한 데이터 처리 메서드를 지원한다. 조금 더 자세히 알아본다.

## Stream의 연산 구조
Stream API는 나름 체계가 있는 친구다. 아래 코드를 보자.

```java
langs.stream()                           // 1.stream 생성
     .filter(lang -> lang.contains("J")) // 2.중간 연산
     .collect(Collectors.toList());      // 3.최종 연산
```

### 1. Stream 생성
간단하다. stream API를 사용하려면 stream을 생성해야하는데 Array OR Collections.stream() 과 같이 생성할 수 있다.

```java
langs.stream() // 1.stream 생성
```

### 2. 중간 연산
**데이터를 처리**하기 위한 메서드들을 사용하는 단계다. return type이 stream이므로 계속해서 다른 메서드로 체이닝이 가능하다.

```java
langs.stream()
     .filter(lang -> lang.contains("J"))       // 2.중간 연산 A
     .filter(lang -> !lang.contains("script")) // 2.중간 연산 B
     .distinct()                               // 2.중간 연산 C
```

지원 메서드들은 아래와 같다.

|메서드명|설명|
|:---|:---|
|filter()|스트림 요소마다 조건문을 만족하는 요소로 구성된 스트림을 반환|
|distinct()|스트림 요소들의 중복을 제거한 스트림을 반환|
|map()|스트림의 각 요소마다 수행할 연산을 구현할때 사용|
|flatMap()|스트림 요소를 새로운 요소로 대체한 스트림을 생성|
|limit()|스트림의 시작 요소로 부터 인자로 전달된 인덱스 까지의 요소를 추출해 새로운 스트림을 생성|
|skip()|스트림의 시작 요소로 부터 인자로 전달된 인덱스 까지를 제외하고 새로운 스트림을 생성|
|sorted()|스트림 요소를 정렬하는 method로 기본적으로 오름차순으로 정렬|
|peek()|결과 스트림의 요소를 사용해 추가로 동작을 수행|


### 3. 최종 연산
stream 끝맺음 위한 매서드들을 사용하는 단계다. return type이 stream이 아니므로 더 이상 체이닝할 수 없다.

```java
langs.stream()
     .filter(lang -> lang.contains("J"))
     .collect(Collectors.toList());     // 3. 최종연산 
```

지원 메서드들은 아래와 같다.

|메서드명|설명|
|:---|:---|
|forEach()|스트림의 요소들을 순환 하면서 반복해서 처리해야 하는 경우 사용|
|reduce()|map 과 비슷하게 동작하지만 개별연산이 아니라 누적연산이 이루어진다는 차이 존재|
|findFirst()|스트림에서 지정한 첫번째 요소를 찾는 메서드|
|findAny()|스트림에서 지정한 첫번째 요소를 찾는 메서드 parallelStream()와 함께 사용|
|anyMatch()|스트림의 요소중 특정 조건을 만족하는 요소가 하나라도 있는지 검사|
|allMatch()|스트림의 요소중 특정 조건을 모두 만족하는지 검사|
|noneMatch()|스트림의 요소중 특정 조건을 만족하는 요소가 하나도 없는지 검사|
|count()|스트림의 원소들로 부터 전체 개수 구하기 위한 메서드|
|min()|스트림의 원소들로 부터 최소값 구하기 위한 메서드|
|max()|스트림의 원소들로 부터 최대값을 구하기 위한 메서드|
|sum()|스트림 원소들의 합계를 구하는 메서드|
|average()|스트림 원소들의 평균을 구하는 메서드|
|collect()|스트림의 결과를 모으기 위한 메서드로 Collectors 객체에 구현된 방법에 따라 처리하는 메서드|

## Stream 특징
지금까지 설명한 Stream API는 아래와 같은 특징을 가진다.

### 1. 내부 반복(internal iteration)을 통해 작업을 수행.
for문을 내장하고 있어 따로 쓸 필요가 없다.
덕분에 간결하고 가독성 높은 코드를 작성할 수 있다.

### 2. 단 한 번만 사용가능.
Stream API는 일회용이기 때문에 한번 사용이 끝나면 재사용할 수 없다.
재사용이 필요한 경우에는 Stream을 다시 생성해야 한다.

### 3. 원본 데이터를 변경하지 않는다.
원본 데이터가 아닌 별도의 요소들로 Stream을 생성한다.
원본 데이터는 읽기만 하며 별도의 Stream 요소들에서 데이터를 처리한다.

### 4. 병렬 처리 지원.
parallelStream() 메소드를 사용해 병렬처리를 쉽게 구현할 수 있다.