# JSX

## JSX란?

자바스크립트의 확장 문법으로, 브라우저가 실행되기 전에 코드가 번들링되는 과정에서 바벨을 사용해 일반 자바스크립트 형태의 코드로 변환된다.
<br />
JSX 코드의 변환 과정은 아래와 같다.

```javascript
//JSX
function App() {
  return (
    <div>
      Hi, there! <b>react</b>
    </div>
  );
}

//변환후
function App() {
  return React.createElement("div", null, "Hi,there!", React.createElement("b", null, "react"));
}
```

컴포넌트를 렌더링할 때마다 위 코드처럼 React.createElement 함수를 사용하면 매우 번거롭기 때문에,
JSX를 사용하면편하게 UI를 렌더링할 수 있다.

<br />

## JSX의 장점

1. 보기 쉽고 익숙하다.
   HTML코드를 작성하는 것과 비슷하기 때문에 가독성도 높고, 작성하기도 쉽다.

2. 활용도가 높다.
   JSX에서는 HTML태그 (ex. `<div>`, `<span>`)뿐만 아니라 컴포넌트도 JSX 안에서 작성할 수 있다.

<br />

#### 덧, ReactDOM.render란?

컴포넌트를 페이지에 렌더링하는 코드로, react-dom 모듈을 불러와 사용할 수 있다. <br />
함수의 첫 번째 파라미터에는 페이지에 렌더링할 내용을 JSX형태로 작성하고,
두 번째 파라미터에는 해당 JSX를 렌더링할 document 내부 요소를 설정한다.

```javascript
ReactDOM.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
  document.getElementById("root")
);
```

위 예시에선 id가 root인 요소 안에 렌더링을 하도록 설정되었다.

#### 덧, React.StrictMode란?

리액트 프로젝트에서 리액트의 레거시 기능들을 사용하지 못하게 하는 기능.
문자열 ref, componentWillMount 등 나중에 완전히 사라지게 될 옛날 기능을 사용했을 때 경고를 출력한다.

<br />

## JSX 문법

### 1. 감싸인 요소

컴포넌트에 여러 요소가있다면 반드시 부모 요소 하나로 감싸야 한다.
Virtual DOM에서 컴포넌트 변화를 감지해 낼 때 효율적으로 비교할 수 있도록 컴포넌트 내부는 하나의 DOM트리 구조로 이루어져야 한다는 규칙이 있기 때문이다.
<br />
부모 요소에 `<div>`를 사용하고 싶지 않다면 리액트 v16이상부터 도입된 Fragment라는 기능을 사용해도 된다.
(부모 요소에 `<Fragment>`태그 사용도 가능하고, `<>`형태로도 사용 가능)

### 2. 자바스크립트 표현

JSX는 단순히 DOM요소를 렌더링하는 기능 외에도 자바스크립트 표현식을 쓸 수 있다.
자바스크립트 표현식을 작성하려면 JSX내부에서 코드를 { }로 감싸면 된다.

<br />

### 3. If문 대신 조건부 연산자

JSX 내부의 자바스크립트 표현식에서 if문을 사용할 수는 없지만, 조건부 렌더링이 필요할 때는
JSX 밖에서 if문을 사용하거나 { }안에 조건부 연산자 (=삼항 연산자)를 사용하면 된다.

<br />

### 4. AND 연산자(&&)를 사용한 조건부 렌더링

리액트에서 false를 렌더링할 때는 null과 마찬가지로 아무것도 나타나지않기 때문에, true 조건일때만 렌더링이 되도록 &&연산자를 사용할 수 있다. 다만 0은 예외적으로 화면에 나타난다는 것을 주의해야한다.

```javascript
const num = 0;
return num && <div>내용</div>;
```

위 코드에서 결과값은 0이 렌더링된다.

<br />

### 5. undefined를 렌더링하지 않기

리액트 컴포넌트에서는 함수에서 undefined만 반환해 렌더링하는 상황을 만들면 안된다.
어떤 값이 undefined이인지 알 수 없다면, OR(||) 연산자를 사용해 반환값이 undefined일 때 사용할 값을 지정할 수 있어
오류를 방지할 수 있다.

```javascript
function App() {
  const name = undefined;
  return name || "값이 undefined";
}

export default App;
```

위 코드에선 name의 값이 undefined일 경우, 값이 undefined라는 문자열이 출력된다.

<br />

### 6. 인라인 스타일링

리액트에서 DOM요소에 스타일을 적용할 땐 문자열이 아닌 객체 형태로 넣어주어야 한다.
스타일 이름 중 `background-color`와 같이 -가 포함되는 이름이 있을 경우, -문자를 없애고 camelCase로 작성해야 한다.

```javascript
function App() {
  const name = "인라인 스타일링 예제";
  return (
    <div
      style={{
        backgroundColor: "black",
        fontSize: "12px",
      }}
    >
      {name}
    </div>
  );
}

export default App;
```

<br />

### 7. class 대신 className

일반 HTML에서 CSS클래스를 사용할 땐 class속성을 설정하지만, JSX에서는 class가 아닌 className으로 설정해주어야 한다.
class로 설정해도 스타일이 적용되긴 하지만 브라우저 개발자 도구의 console탭에 경고가 나타난다.

<br />

### 8. 꼭 닫아야 하는 태그

HTML 코드에선 `<input>`등 몇 태그는 닫지않아도 문제없이 작동되었지만, JSX에선 코드를 닫지 않으면 오류가 발생한다.
태그 사이에 별도의 내용이 들어가지 않을 경우 `<input />`과 같이 self-closing을 통해 태그를 선언하면서 동시에 닫을 수 있다.

<br />

### 9. 주석

JSX 내부에서 주석을 작성할 땐 `{/* */}` 혹은 `//`의 형식으로 작성한다.
