# moment로 날짜 나타내기

길고 복잡한 형태의 날짜+시간 데이터를 `slice`로 한땀한땀 자르고 `join`으로 결합시키는 건 이제 그만. moment 라이브러리를 통해 손쉽게 원하는 형식으로 변환할 수 있다.

## 1 : moment 라이브러리 설치

`import moment from 'moment';`

<br />

## 2 : 날짜 형식으로 변환

`const newDate = moment("2020-08-23T16:00").format('YYYY-MM-DD');`

### 결과

`2020-08-23T16:00 => 2020-08-23`
