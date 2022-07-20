# Vanilla JS로 구현한 debounce로 성능 최적화하기

## debounce란?

주어진 시간 내에 동일한 이벤트가 반복적으로 시행되는 경우, 마지막 이벤트 발생 후 새로운 이벤트가 발생하지않고 제한 시간이 모두 지나면 해당 이벤트가 실행되도록 하는 기술. 연이은 이벤트를 하나의 그룹으로 묶어준다는 개념이다.

<br />

## Vanilla JS로 debounce 구현하기

```javascript
const handleDebounce = (callback, limit) => {
  let timeout;
  return function (...args) {
    clearTimeout(timeout);
    timeout = setTimeout(() => {
      callback.apply(this, args);
    }, limit);
  };
};
```

- `callback`: 실행 대상이 되는 콜백 함수
- `limit`: 얼마 후에 함수를 실행할 지를 결정하는 제한 시간 (millisecond)

handleDebounce 함수가 실행되면 변수`timeout`은 `clearTimeout` 함수에 의해 초기화되고, 이후 `setTimeout`에 의해 limit ms이후 callback함수가 실행된다.
따라서 limit ms 이내에 callback함수가 호출되면 `clearTimeout(timeout)`에 의해 실행되지 않고, 마지막 이벤트 발생 후 limit ms가 지나면 setTimeout이 실행되어 callback함수가 실행된다.

<br />

## debounce 활용 예제

debounce가 가장 잘 활용되는 곳은 바로 검색 기능.
가령 input에 텍스트가 입력될 때 이벤트를 추가한다고 하면, 아래와 같이 input에 addEventListener로 keyup이벤트를 추가한다.

```javascript
// --- debounce를 적용하지않고 addEventListener
input.addEventListener("keyup", () => {
  console.log("search"); //연속적으로 텍스트를 입력하면 입력할 때마다 search 출력
});
```

위 코드를 실행하면 텍스트를 연속으로 입력할 때마다 callback함수가 실행되어 console에 search가 미친듯이 찍히게된다.
기능 구현은 되지만 의도한 것보다 이벤트가 불필요하게 많이 실행된다는 문제점이 있으니, 해결을 위해 아래와 같이 debounce를 적용해보자.

<br />

```javascript
// --- 검색 함수 호출 최적화를 위한 debounce
const handleDebounce = (callback, limit) => {
  let timeout;
  return function (...args) {
    clearTimeout(timeout);
    timeout = setTimeout(() => {
      callback.apply(this, args);
    }, limit);
  };
};

// --- debounce 적용 후 addEventListener
input.addEventListener(
  "keyup",
  handleDebounce(() => {
    console.log("search");
  }, 500) //연속적으로 텍스트를 입력하더라도 일정시간 이후에 한번만 search 출력
);
```

위 코드를 적용하면 텍스트를 연속해서 작성해도 마지막 작성 뒤 500ms(0.5초) 후 console에 search가 한 번만 찍힌다.
