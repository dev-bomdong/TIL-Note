주로 데이터를 받아올 때마다 기계적으로 사용하고 있는 promise에 대해서 다시 한 번 짚고 넘어가보기. 아는 것도 다시 보자!

# Promise

자바스크립트에서 비동기 처리를 돕는 자바스크립트 내장 오브젝트.
정해진 기능 수행 후 성공/실패 결과를 반환한다.

## 1. state

`pending` => `fulfilled` or `rejected`

#### pending

promise가 만들어져서 작업이 수행중일 때

#### fulfilled

작업이 성공했을 때

#### rejected

작업이 실패했을 때

## 2. Producer(정보제공)

원하는 작업을 수행해 해당하는 정보(데이터)를 제공하는 Promise Object.
새로운 Promise가 생길 때, 함께 전달된 콜백 함수는 바로 실행되는 점을 유의해야 한다.

```javascript
const promise = new Promise((resolve, reject) => {
  console.log("processing");
  setTimeout(() => {
    //성공했을 때
    resolve("success");
    //실패했을 때 : reject(new Error("error"));
  }, 1000);
});
```

## 3. Consumer

Producer가 제공한 데이터를 소비하는 `then`, `catch`, `finally`

```javascript
promise
  //promise가 성공적으로 수행되었을 때 실행
  .then((value) => {
    console.log("value : ", value);
  })
  //promise의 수행이 실패했을 때 실행
  .catch((error) => {
    console.log("error : ", error);
  })
  //성공/실패 상관없이 마지막에 무조건 실행
  .finally(() => {
    console.log("anyway, finally!");
  });
```

# async/await

promise를 동기적으로 실행되는 것처럼 관리할 수 있는 방법.

```javascript
const fetchData = async () => {
  const firstPromise = getFirstData();
  const secondPromise = getSecondData();
  const first = await firstPromise;
  const second = await secondPromise;
  return `${first}+ ${second}`;
};
```

# Promise API : Promise.all

```javascript
const fetchData = () => {
  return Promise.all([getFirstData(), getSecondData()]).then((result) =>
    result.join(",")
  );
};
fetchData().then(console.log);
```
