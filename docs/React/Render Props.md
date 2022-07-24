# Render Props (=Function as a child)

컴포넌트의 children이 있어야 할 자리에 일반 JSX 혹은 문자열이 아닌 함수를 전달하는 것.

<br />

## 예제

아래와 같은 컴포넌트 `RenderPropsSample`이 있다고 가정해보자.

```javascript
import React from "react";

const RenderPropsSample = ({ children }) => {
  return <div> 결과 : {children(5)}</div>;
};

export default RenderPropsSample;
```

<br />

`RenderPropsSample`은 추후 사용할 때 아래와 같이 사용할 수 있다.

```javascript
<RenderPropsSample>{(value) => 2 * value}</RenderPropsSample>
```

`RenderPropsSample`에게 children props로 파라미터에 2를 곱해서 반환하는 함수를 전달하면,<br />
이 컴포넌트에서는 함수에 5를 인자로 넣어 최종적으로 `결과 : 10`을 렌더링한다.
