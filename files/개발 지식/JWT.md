# JWT (JSON Web Token)

인증에 필요한 정보들을 암호화시킨 토큰

## JWT기반 인증

JWT토큰을 HTTP header에 실어 서버가 client를 식별하는 토큰 기반 인증 시스템

### Process

1. `Client`가 id, pw를 입력해 Server에 로그인 인증 요청
2. `Server`는 Header, Payload, Signature를 정의하고 각각 암호화해 JWT를 생성하고 이를 cookie에 담아 Client에게 발급
3. `Client`는 JWT를 저장하고, 인증이 필요한 API요청시 Server에 Authorization Header에 Access token을 담아 보냄
4. `Server`는 Client로부터 받은 token과 이전에 Server에서 발행한 token 비교하고, 일치하면 payload의 정보를 Client에게 돌려줌
5. `Client`는 Access token이 만료되면 refresh token을 Server에 보냄
6. `Server`는 새로운 Access token을 발급해 Client에 전달

## 구조

`.`을 구분자로 나뉘는 세 문자열의 조합
`Header.Payload.Signature`의 형태

### Header

```javascript
//디코딩된 형태
{
    "alg" : ... , //서명 암호화 알고리즘
    "typ" : "JWT", //토큰 유형
}

```

- signature 암호화 알고리즘
- 토큰 유형

### Payload

서버와 클라이언트가 주고받는 시스템 속에서 실제로 사용될 정보 (claim : key-value 형식의 정보 한 쌍)

- Registered claims : 미리 정의된 클레임 `ex. iss(발행자), exp(만료시간), iat(발생시간) 등`
- Public claims : 공개용 정보 전달을 위한 클레임 (사용자가 정의가능))
- Private claims : 당사자들간 정보 공유용 사용자 지정 클레임

### Signature

`header의 인코딩 값 + payload의 인코딩 값 + 서버의 key값`을 header에서 정의한 알고리즘으로 암호화
서버의 key값이 유출되지 않는 이상 복호화할 수 없다.

## 장점

- Header와 Payload로 signature를 생성하기 때문에 데이터가 위,변조되었을 때 더 쉽게 판별할 수 있다.

#### Session과 달리

- 별도 저장소가 불필요하다.
- 서버를 stateless로 유지할 수 있다.
- 정보를 자체적으로 보유할 수 있어 요청할 때마다 DB조회를 하지않아도 된다.
- 모바일 환경에서도 잘 동작한다.

## 단점

- token의 길이가 길어 인증 요청이 많아질수록 네트워크 부하 위험이 있다. (DB 성능면에선 이득이지만)
- token은 탈취당할 경우 대처하기가 어렵다.
