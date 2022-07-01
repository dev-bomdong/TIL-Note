# HTML에 Javascript를 연결하는 방법

HTML에 Javascript를 연결하기 위해선 기본적으로 아래 코드를 해당 HTML에 입력한다.

```html
<script src="파일명.js"></script>
```

다만 이 때 **코드의 위치**와 **옵션**에 따라 각기 다른 특징을 가진다.
그렇다면 각 방법의 특징과, 어느 방법이 가장 최선일지도 함께 알아보자.
각 방법은 위치, 옵션에 따라 총 4가지로 나뉘어진다.

<br>

## 용어 정리

우선 각 방법의 실행 순서 이해를 위해 알아둘 용어들이 있어 정리해본다. ( \* Parsing(파싱)과 fetching(패칭)은 영문과 국문을 번갈아 쓸 예정 😅 )

### Parsing (파싱)

처음 들어본 개념인 Parsing(파싱)은 Parser(파서)에 대한 이해에서부터 시작된다.

> **Parser(파서)**는 컴파일의 일부로서 [원시 프로그램의 명령문이나 온라인 명령문, HTML 문서 등에서 Markup Tag등] 을 '입력'으로 받아들인 뒤, [구문을 해석할 수 있는 단위와 여러 부분]으로 '분할'해주는 역할을 한다.
> 정리하자면, **원시프로그램 Markup Tag >> **(입력)** >> <span style="color:teal">Parser</span> >> **(분할)** >> 구문을 해석할 수 있는 단위**

_Parsing(파싱)은 이러한 파서(parser) 역할을 하는 컴퓨터가 구문 트리(parse tree)로 재구성하는 구문 분석 과정을 뜻한다_ 고 하는데.. 너무 사전적인 정의라고 느껴져 현 상황에선 정확히 와닿지 않았다.

쉬운 말로 다시 말하자면, **Parsing(파싱)은 페이지(ex. html)에서 내가 원하는 데이터를 특정 패턴/순서로 추출해 정보로 가공하는 것! ** 이 과정에서 부호에 불과했던 일련의 문자열이 기계어로 번역되고, 결국 의미있는 단위가 된다.
광산에서 돌을 캐낸 뒤 그 중 원석<span style="color:olive">(원하는 데이터)</span>을 골라내어 보석<span style="color:olive">(정보)</span>으로 가공해내는 과정과 비슷하다고 생각하면 된다.

아래의 실행순서 란에서 parsing html이란 단어가 여러 번 나오는데, html 페이지가 완성된다는 의미로 참고하면 좋을 듯 하다.

### fetching (패칭)

가져오는 것

### executing

실행시키는 것

👉위의 두 개념은 상대적으로 익숙한 개념이였다.
데이터를 메모리에서 가져(fetch)와서, 실행(execute)시킨다.

<br>

## 1. `<head>`

> `<head>`안에 코드 입력

```html
<head>
  <script src="파일명.js"></script>
</head>
```

### 실행 순서

parsing HTML ▶ fetching JS ▶ executing JS ▶ parsing HTML <span style="color:seagreen">(page ready)</span>

### 단점

JS 패칭/실행까지 모두 완료되어야 HTML 파싱이 완료되므로, JS 파일이 무겁고 인터넷이 느리다면 웹사이트 로딩이 느려진다.

<br>

## 2. `<body>`

> `<body>`의 끝 부분에 코드 입력

```html
<body>
  …
  <script src="파일명.js"></script>
</body>
```

### 실행 순서

parsing HTML <span style="color:seagreen">(page ready)</span> ▶ fetching JS ▶ executing JS

### 단점

HTML파싱이 먼저 되므로 사용자가 기본적인 컨텐츠는 빨리볼 수 있지만, JS에 의존적인 페이지라면 정상적인 웹사이트를 보기 전까지 대기시간이 길어진다.

<br>

## 3. `<head>` + async

> `<head>`안에 `asyn` 옵션(비동기 처리) 추가

```html
<head>
  <script asyn src="파일명.js"></script>
</head>
```

### 실행 순서

parsing HTML & fetching JS ▶ executing JS ▶ parsing HTML <span style="color:seagreen">(page ready)</span>

### 장점

JS가 병렬적으로 패치/실행되므로 다운로드 시간이 절약된다.

### 단점

비동기 방식인지라 정의된 스크립트 순서와 상관없이 다운로드 되는 순서대로 실행된다.
`ex. a.js ▶ b.js ▶ c.js 순서로 실행되어야하는 JS여도 다운로드 순서대로 실행되기 때문에 문제가 생길 수 있다.`

<br>

## 4. `<head>` + defer

> `<head>`안에 `defer`옵션 (페이지가 모두 로드된 후에 외부 스크립트 실행) 추가

```html
<head>
  <script defer src="파일명.js"></script>
</head>
```

### 실행 순서

parsing HTML & fetching JS <span style="color:seagreen">(page ready)</span> ▶ executing JS
(HTML 파싱 동안 JS는 패칭만 해두고, HTML 다운완료시 JS 실행)

### 장점

HTML파싱과 JS패칭이 함께 이루어지고, 그 이후에야 JS가 실행되기 때문에 (async과 달리) JS가 원하는 순서대로 실행될 것임을 예상할 수 있다.

<br>

> 그렇게 최선의 방법을 알게되었다.
> HTML과 JS를 연결할 땐 **`<head>` + defer** 옵션을 사용하자!

<br>

# use strict 선언

이건 보너스 개념✨
(특히) 바닐라JS를 이용할 때, 가장 위에 `‘use strict’;`를 선언하고 시작하자.

JS의 Flexible함은 때로는 독이 될 수 있다.
(ex. 변수선언을 하지 않더라도 a=10;b=10;을 입력하면 a+b=20;이 나온다던가 그런..다른 언어 개발자들이 본다면 뭐지?싶을듯한..! )

때문에 그런 것들을 차단하는! **strict 모드로 개발**하도록 선언하는 것이 필요하다.
좀더 상식적인 범위에서 JS를 이용하고, JS엔진도 더 효율적으로 돌아가도록 할 수 있다.
