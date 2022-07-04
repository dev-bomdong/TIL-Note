# 아이템 38 : any타입은 가능한 한 좁은 범위에서만 사용하기

#### any의 장점

- 마이그레이션 (JS 코드 => TS 코드로 전환) 시 코드 일부분의 타입 체크 비활성화
- 프로그램의 일부분에만 타입 시스템 적용 가능하도록 함

#### any 사용시 주의점

- 타입 안정성 훼손을 피하기위해 사용 범위를 최소한으로 좁혀야한다.

### 1. 함수와 관련된 any 사용법

#### 문제상황

```javascript
const processFunc = (a: Value) => {
  /*...*/
};

const returningFoo = (b: Foo) => {
  return b;
};

const fun = () => {
  const x = returningFoo();
  processFunc(x);
  // Error : Foo 형식의 인수는 Value 형식의 매개변수에 할당될 수 없다.
};
```

#### 방법 1 : 타입 오류 변수를 any타입으로 선언

- 사용되지 말아야 할 방법 : any타입이 다른 코드에 많은 영향을 끼침 (타입 체크 무력화)
- 최악의 경우 : 함수의 반환값에 any타입이 할당될 때

```javascript
const func1 = () => {
  const x: any = returningFoo();
  processFunc(x);
  return x;
  // x의 타입 : any
};

const _func1 = () => {
  const foo = func1(); // 반환값 : any타입
  foo.fooMethod(); // 타입 체크 되지 않음
};
// func1의 반환값 타입이 _func1에서의 foo 타입에 영향을 끼치게 됨 (타입체크 되지않음)
```

#### 방법 2 : 타입 오류 변수가 사용되는 곳에 as any 단언문 사용

- 가장 권장되는 방법 : any타입이 다른 코드에 영향을 미치지 않음

```javascript
const func2 = () => {
const x = returningFoo();
processFunc(x as any); // x의 타입 : any
// x의 타입 : Foo
};
```

#### 방법 3 : @ts-ignore 사용

- 다음 줄의 오류를 무시하는 방법
- 근본적인 해결책이 아님

```javascript
const func1 = () => {
  const x: any = returningFoo();
  @ts-ignore //다음 줄의 오류가 무시됨
  processFunc(x);
  return x;
};
```

### 2. 객체와 관련된 any 사용법

#### 문제상황

```javascript
const config: Config = {
  x: 1,
  y: 2,
  z: {
    key: value,
    //Error
  },
};
```

#### 해결 방법

```javascript

// 안좋은 예 : 객체 전체를 any로 단언
const config: Config = {
  x: 1,
  y: 2, // x,y 모두 타입 체크되지 않음
  z: {
    key: value,
  },
} as any;

//적절한 예 : 최소한의 범위에만 any 사용
const config: Config = {
  x: 1,
  y: 2, // x,y 모두 정상적으로 타입 체크됨
  z: {
    key: value as any
  },
} as any;
```

# 아이템 39 : any를 구체적으로 변형해서 사용하기

#### 구체적으로 모델링하기

1. 배열

- 배열 요소의 타입을 알 수 없을 때, `any`보다 `any[]` 사용하기

```javascript
// 나쁜 예
const getLength = (array: any) => {
  return array.length;
};
// 함수 반환값이 타입체크 되지않음
// 함수 호출시 매개변수 타입체크 되지않음

// 좋은 예
const _getLength = (array: any[]) => {
  return array.length;
};
// 함수 반환값이 타입 체크됨 (number 타입 체크)
// 함수 호출시 매개변수 타입체크 됨
```

2. 객체

- 함수의 매개변수가 객체이지만 값을 알 수 없을 때, `{[key: string]: any}`사용하기

3. 함수

```javascript
type Func1 = () => any;
// 매개변수 없이 호출 가능한 모든 함수

type Func2 = (arg: any) => any;
// 매개변수 1개

type Func3 = (...arg: any[]) => any;
//모든 개수의 매개변수
```

# 아이템 40 : 함수 안으로 타입 단언문 감추기

- 타입 단언문은 현실적인 해결책이 될 수도 있다.
- 타입 단언문을 사용할 땐 정확한 정의를 가지는 함수의 내부로 숨기는 것이 좋다.

```javascript
declare function swallowEqual(a: any, b: any): boolean;
// declare : 타입 스크립트 컴파일러에게 이런 타입이 있다고 말해주는 것

function swallowEqual<T extends Object>(a: T, b: T) {
  for (const [k, aVal] of Object.entries(a)) {
    if (!(k in b) || aVal !== b[k]) {
      // Error => b[k] : Element implicitly has an 'any' type because expression of type 'string' can't be used to index type 'Object'.
      // 해결방법 : b[k] => (b as any)[k] : 이미 k in b 체크를 했으므로 안전한 타입 단언문
      return false;
    }
  }
  return Object.keys(a).length === Object.keys(b).length;
}
```

# 아이템 41 : any의 진화를 이해하기

- 타입스크립트에서 변수의 타입에 새로운 값이 추가되도록 확장하는 것
  (일반적으로 타입스크립트에서 변수의 타입은 변수를 선언할 때 결정되고, 이후 추가나 확장할 수 없으나 any를 활용했을 때 예외적으로 허용된다.)

#### any의 진화 예시

```javascript
const result = []; //타입 : any[]

result.push("a");
result; //타입 : string[]

result.push(1);
result; //타입 : (string | number)[]
```

- 암시적 any타입에 어떤 값을 할당할 때만 발생(암시적 any 상태인 변수를 할당없이 사용하려고 하면 암시적 any 오류 발생)

#### 오류발생 예시

```javascript
const range = (start: number, limit: number) => {
  const out = [];
  // Variable 'out' implicitly has type 'any[]' in some locations where its type cannot be determined.

  if (start === limit) {
    return out;
    // Variable 'out' implicitly has an 'any[]' type.
  }
};
```

- 명시적으로 any를 선언하면 타입이 그대로 유지된다.

```javascript
let val: any;
if (Math.random() < 0.5) {
  val = "text";
  val; //타입 : any
} else {
  val = 11;
  val; //타입 : any
}
val; //타입 : any
```

- any를 진화시키는 방법보다 명시적 타입 구문을 사용하는 것이 타입을 더 안전하게 지키는 안전한 설계

<br />

# 아이템 42 : 모르는 타입의 값에는 any대신 unknown을 사용하기

### unknown이란?

any대신 쓸 수 있는 타입 시스템에 부합하는 타입

#### any의 특징

1. 어떠한 타입이든 할당 가능
2. 어떠한 타입으로도 할당 가능

#### unknown의 특징

1. 어떠한 타입이든 할당 가능
2. 어떠한 타입으로도 할당 **불가** (unknown과 any에만 할당 가능)

#### never의 특징

1. 어떠한 타입이든 할당 **불가** (어떤 타입도 never에 할당 불가)
2. 어떠한 타입으로도 할당 가능

#### 함수의 반환값과 관련된 unknown

unknown 타입인 채로 값을 사용하면 오류가 발생하므로 타입 단언 등으로 적절한 타입으로 변환 필요

```javascript

const parseYAML(yaml:string): any {
// 반환값의 타입이 any
}

interface Book {
  name: string;
  author: string;
}

function safeParseYAML(yaml:string): unknown {
  return parseYAML(yaml);
  // 반환값의 타입이 unknown
}

const book: Book = safeParseYAML(`
name: Harry Potter
author: J.K.Rowling
`); as Book;
// unknown 타입 그대로 값을 사용하면 에러 발생해 Book으로 타입 단ㄷ언

alert(book.title)
// Error
```

#### 변수 선언과 관련된 unknown

어떠한 값이 있지만 그 타입을 모르는 경우 unknown사용

```javascript
interface Person {
  name: string;
  favoriteThing: unknown;
}
```

#### 단언문과 관련된 unknown

- 이중 단언문에서 any대신 unknown 사용 가능
- 이중 단언문을 분리할 때 unknown 형태가 더 안전 (분리되는 즉시 오류 발생)

```javascript
const foo: Foo;
let A = foo as any as Bar;
let B = foo as unknown as Bar; //상대적으로 안전
```

### {}, object, unknown의 차이점 알기

#### {}

- **null과 undefined를 제외한** 모든 값 포함
- unknown 타입 도입 전 일반적으로 사용되었으나 최근엔 드물게 사용
- null과 undefined가 불가능하고 확신할 때만 사용

#### object

모든 비기본형(non-primitive) 타입으로 구성
(객체/배열은 포함 / string, number, boolean 미포함)

<br />

# 아이템 43 : 몽키 패치보다는 안전한 타입을 사용하기

### 몽키 패치란?

- 일반적으로는 런타임 중인 프로그램의 메모리 소스를 직접 변경하는 것을 뜻하며,
  javascript에선 prototype object를 직접 변경하는 것을 뜻한다.

### 몽키 패치의 문제점

- 전역변수가 된 데이터의 의존성을 고려해야 함
- 타입스크립트의 타입 체커는 임의로 추가한 속성의 타입을 알지 못함 (as any로 단언할 시 타입 안전성 상실)

### 해결법

#### 1. document/DOM으로부터 데이터 분리

가장 최선의 해결책이나 분리할 수 없는 경우 존재
(객체-데이터가 붙어있어야 하는 라이브러리 사용 등)

#### 2. interface의 특수 기능인 보강(augmentation) 사용

- 보강 (augmentation)의 예시 (아이템 13)

```javascript
interface Person {
  name: string;
  age: number;
}

interface Person {
  favoriteColor: string;
}

const Dong: Person = {
  name: "Dong",
  age: 20,
  favoriteColor: "white",
  //정상 : 선언 병합 (declaration merging)
};
```

- 모듈 관점에서 (타입스크립트 파일이 import/export 사용시) 제대로 동작하려면 global선언 추가 필요

```javascript
export {};
deglare global {
  interface Document {
  monkey: string;
  }
}
document.monkey = "Taitan";
```

- 전역적으로 적용되므로 코드의 다른 부분/라이브러리와 분리 불가
- HTML element 조작시 모든 element의 속성 여부를 고려해야함

#### 3. 더 구체적인 타입 단언문 사용

```javascript
interface MonkeyDocument extends Document {
  // 새로운 타입 도입으로 import하는 영역에만 해당됨
  monkey: string;
}

(document as MonkeyDocument).monkey = "Taitan";
// 몽키 패치된 속성을 참조하는 경우에만 단언문 사용
```
