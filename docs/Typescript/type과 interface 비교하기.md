# type과 interface 비교하기

<br />

```javascript
interface IPerson {
  name: string;
  age: number;
}

type TPerson = {
  name: string,
  age: number,
};
```

타입스크립트에서 타입을 정의하는 방법은 `type`과 `interface`가 있다. <br />
대부분의 경우에는 비슷하게 사용되지만, 분명한 차이점이 존재하는 두 방법에 대해 알아보자.

## 차이점

### 1. 타입 확장

- interface는 `extends` 키워드를 사용해 타입을 확장한다.
- type은 `&` 연산자를 통해 타입을 확장한다.

```javascript
interface IPerson2 extends IPerson {
  gender: string;
}

type TPerson2 = TPerson & {
  gender: string,
};
```

<br />

- type은 동일한 이름으로 다시 타입을 선언하면 에러가 난다.
- interface는 동일한 이름으로 다시 interface를 정의해 병합하며 확장할 수 있다. (declaraion merging)

```javascript
interface IPerson {
  name: string;
  age: number;
}

interface IPerson {
  id: number;
}

const person: IPerson = {
  name: "Gildong",
  age: 20,
  id: 12121,
};
```

<br />

### 2. computed property name (계산된속성명)

computed property name (계산된속성명)은 아래처럼 표현식으로 객체의 key값을 정의하는 문법이다.

```javascript
const handleObj = (key, value) => {
  ({ [key]: value });
};
```

computed property name을 활용한 타입 선언이 type은 가능한 반면, interface는 불가하다.

```javascript
type fullName = 'firstName' | 'lastName';

type NameType = {
  [key in fullName]: string;
};

const friend: NameType = {
  firstName: 'Harry',
  lastName: 'Potter',
};

interface NameInterface {
  [key in fullName]: string;
} //error

```

<br />

### 3. 유니온 개념의 유무

```javascript
type ID = number | string;
```

type은 유니온 타입 선언이 가능하므로 병렬적인 확장에선 type을 사용하는 것이 좋다.
