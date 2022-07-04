# var, let, const의 차이

## 1. 중복 선언 가능 여부

`var`는 중복 선언이 가능한 반면, `let`과 `const`는 중복 선언이 불가하다

```javascript
var a = 1;
var a = 3;
console.log(a);
// 3

let b = 1;
let b = 3;
console.log(b);
// Error : Identifier 'b' has already been declared.

const c = 1;
const c = 3;
console.log(c);
// Error : Identifier 'c' has already been declared.
```

<br />

## 2. 재할당 가능 여부

`var`와 `let`은 변수를 선언하는 키워드로, 변수 선언 및 초기화 이후 재할당이 가능하다.
`const`는 상수를 선언하는 키워드로, 선언/초기화 이후 재할당이 불가하다.
또한 초기 선언 때 반드시 값을 할당해주어야되기도 한다.

```javascript
var a = 1;
var a = 3;
console.log(a);
// 3

let b = 1;
b = 3;
console.log(b);
// 3

const c = 1;
c = 3;
console.log(c);
// Error : Assignment to constant variable
```

<br />

## 3. 스코프

`var`는 function-level scope로 함수 내부에 선언된 변수만 지역 변수로 간주하며, 나머지는 모두 전역변수로 간주한다. 따라서 if문, for문 등의 코드 블럭 내부에서 선언을 하더라도 전역 변수로 간주된다.

<br />

`let`과 `const`는 block-level scope로 함수 내부 뿐만 아니라 코드 블럭 내부에서 선언을 하더라도 모두 지역 변수로 간주되어 외부에서 참조할 수 없다.

<br />

```javascript
if ("test".length > 3) {
  var a = 1;
}
console.log(a);
// 1

if ("test".length > 3) {
  let b = 1;
}
console.log(b);
// Error: b is not defined

if ("test".length > 3) {
  const c = 1;
}
console.log(c);
// Error: c is not defined
```

## 4. 호이스팅

`var`는 변수를 선언함과 동시에 `undefined`로 초기화되므로, 뒤에서 선언된 변수가 앞에서 참조되어도 에러가 나지않는다.

<br />

반면에 `let`과 const는 변수 선언과 초기화가 별개로 이루어져야하므로, 변수 선언만 호이스팅되어 그 값을 참조할 수는 없다. 뒤에서 선언된 변수가 앞에서 참조되면 값을 참조할 수 없어 에러가 난다.

```javascript
console.log(a);
var a = 1;
// undefined

console.log(b);
let b = 2;
// Error: : b is not defined

console.log(c);
const c = 3;
// Error: c is not defined
```
