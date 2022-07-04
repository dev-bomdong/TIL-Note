# Arrow Function (화살표 함수)

화살표 함수란 함수를 더욱 간단히 표현할 수 있는 ES6 문법이다.
(\* ES6 : JavaScript를 표준화/규격화하기 위해 만들어진 ECMA Script의 버전 중 하나)
기존의 일반함수 표현식에 비해 구문이 짧아 처음엔 이게 뭐지? 싶지만 적응하면 그 편리함에 감탄하게 될 것

## 화살표 함수의 선언

### 기본형태

함수 표현식 자체의 기본적인 형태는 아래와 같다.

```javascript
//function 함수 표현식
function (arg1, arg2...) {expression}

//화살표 함수 표현식
(arg1, arg2...) => expression
```

<br>

### 인자 개수에 따른 분류

함수의 인자 개수에 따라 인자를 감싸는 괄호 생략 가능 여부가 다르다.

#### 인자가 없을 때

```javascript
//첫 번째 방법 : 인자 괄호 생략 불가, 안은 비워둔다.
let sayGoodmorning = () => console.log("Good morning!");

//두 번째 방법 : 인자 괄호 생략 가능, 인자 자리에 _를 작성한다.
let sayGoodmorningS = (_) => console.log("Good morning!");
```

#### 인자가 하나일 때

```javascript
//인자 괄호 생략 가능
let double = (a) => a * 2;
```

#### 인자가 하나 이상일 때

```javascript
//인자 괄호 생략 불가
let plus = (a, b) => a + b;
```

<br>

### 표현식/구문 개수에 따른 분류

```
//화살표 함수 표현식
(arg1, arg2...) => expression
```

화살표 함수는 `=>` 왼쪽에 있는 인수를 이용해 `=>` 오른쪽의 표현식을 평가하는데,
이 때 표현식(구문)의 개수에 따라 구문에 차이가 있다.

#### 표현식/구문이 하나일 때

- 표현식 부분에 `return`이 생략되어 있고 `{ }` 을 사용할 필요가 없으므로, `return될 값` 그 자체만 작성하면 된다.

```javascript
const sum = (a, b) => a + b;
```

#### 표현식/구문이 하나 이상일 때

- 표현식을 시작할 때 `{ }` 를 사용해 본문이 여러 줄로 구성되어 있음을 알려준다.
- 중괄호 사용시엔 `return` 으로 결과값을 반환해주어야 한다.

```javascript
const sum = (a, b) => {
  // 중괄호 사용 = 본문 여러 줄이다!
  const result = a + b;
  return result; // return을 사용해 결과 값 반환
};
```

<br>

## 화살표 함수의 호출

화살표 함수는 익명으로만 사용이 가능하기 때문에, 호출하기 위해선 함수 표현식을 사용하거나 콜백 함수로서 사용된다.

### 함수 표현식 사용

```javascript
const double = (a) => a * 2;
console.log(double(7));
//14
```

### 콜백 함수로서 사용

```javascript
const arr = [2, 4, 6, 8, 10];
const doubleArr = arr.map((x) => x * 2);

console.log(doubleArr);
//[4,8,12,16,20]
```

## 일반 함수와의 차이

표현식의 길이 차이도 있겠지만 가장 큰 차이는 `this`이다.

### 일반 함수의 this

일반 함수는 함수를 선언할 때 this에 바인딩할 객체가 정적으로 결정되는 것이 아니고, <span style="background-color:lightgreen">함수를 호출할 때 </span>**함수가 어떻게 호출되었는지에 따라 동적으로 결정**된다.

<br>

### 화살표 함수의 this

화살표 함수는 <span style="background-color:lightgreen">함수를 선언할 때</span> this에 바인딩할 객체가 **정적으로 결정**된다. 동적으로 결정되는 일반 함수와는 달리 화살표 함수의 this는 항상 **상위 스코프의 this**를 가리킨다. 이를 Lexical this라 한다.

<br>

## 화살표 함수를 사용하면 안되는 경우

앞서 화살표 함수의 편리성을 강조해왔는데, 그럼 이제 일반함수는 필요가 없는걸까?
모든 걸 화살표 함수로 대체하면 되는걸까? Noooo... 🙅️
화살표 함수를 사용하면 안되는 네 가지 경우를 알아보자.

### 메소드

```javascript
const Person = {
  name: "Kim",
  sayHi: () => console.log(`Hi ${this.name}`),
};

Person.sayHi();
// Hi undefined
```

위 코드에서 화살표 함수 안의 this는 전역 객체 Window를 가리키므로, 화살표 함수로 메소드를 정의하는 것은 적절하지 않다.

### prototype

화살표 함수로 정의된 메소드를 prototype에 할당할 때도 동일한 문제가 발생한다.

```javascript
const Person = {
  name: "Kim",
};

Object.prototype.sayHi = () => console.log(`Hi ${this.name}`);

Person.sayHi();
// Hi undefined
```

### 생성자 함수

생성자 함수는 prototype 프로퍼티를 가지고, 이것이 가리키는 프로토타입 개체의 constructor를 사용한다. 하지만 화살표 함수는 prototype 프로퍼티를 가지고 있지 않기 때문에 생성자 함수로서 사용될 수 없다.

<span style="color:gray">_화살표 함수 new와 함께 호출해놓고 어디에서 잘못되었을까 고민하지않기..약속.._</span>

### addEventListener 함수의 콜백 함수

addEventListener 함수의 콜백 함수를 화살표 함수로 정의하면, 화살표 함수의 this가 상위의 전역 객체 window를 가리킨다. <span style="color:gray">_(이쯤되면 정말 화살표 함수의 this..뚝심있다..!)_</span> 일반 함수로 정의할 경우엔 이벤트 리스너에 바인딩된 요소(currentTarget)를 가리키므로, 일반 함수를 이용하자.

> 📌️ **참고 학습자료**

[poiemaweb.com - 화살표 함수](https://poiemaweb.com/es6-arrow-function)
[MDN - 화살표 함수](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Functions/Arrow_functions)
