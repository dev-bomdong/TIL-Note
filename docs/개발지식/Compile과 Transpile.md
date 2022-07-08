# Compile과 Transpile

## Compile

어떠한 언어로 작성된 소스 코드를 다른 언어로 변환하는 것

### 예시

Transpile 개념을 포함하므로, Transpile의 예시는 Compile의 예시이기도 하다.

## Transpile

어떤 언어로 작성된 소스 코드를 `비슷한 수준의 추상화를 가진 다른 언어`로 변환하는 것

### 유념해야 하는 사항

`Compile > Transpile` <br />
Transpile은 결국 어떠한 언어를 다른 언어로 변환하는 것이므로, Compile의 범주 안에 속한 개념이다.
(Babel이 javascript compiler라고 설명되는 이유. transpiler이자 compiler인 것.)

### 예시

#### babel

javascript ES6 -> ES5 로 변환

#### Typescript

Typescript -> Javascript로 변환

#### Sass

전처리기로 작성 -> CSS로 변환

- Javascript 개발환경에선 주로 `node-sass`를 활용해 컴파일을 하므로 `node-sass` 패키지를 함께 설치한다.
