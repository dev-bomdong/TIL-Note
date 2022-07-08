# CORS에러와 대처방안

## CORS에러란?

보안 상의 이유로 브라우저는 서로 다른 출처의 (다른 Protocol, Host, Port를 가진) HTTP 요청들을 제한하는데, <br />
이 때 **CORS policy에 의해 access가 차단되었다**는 CORS에러가 뜬다.

예를 들어 클라이언트 서버의 포트가 `3000`이고 서버의 포트가 `8000`일 때 클라이언트에서 서버로 리소스를 요청할 경우 CORS 에러 메세지가 뜬다.

CORS는 어떤 출처에서 실행 중인 웹 애플리케이션이 다른 출처의 자원에 접근하는 권한을 부여받도록 브라우저에 알려주는 체제로, 같은 출처에서만 리소스를 공유할 수 있도록 하는 보안 방식인 `SOP(Same Origin Policy)` 때문에 등장하게 되었다.

클라이언트와 서버 간 HTTP-header에 origin을 전달하고 응답받은 `Access-Control-Allow-Origin` 을 통해 진행된다.

## 대처방안

1. 우선 로컬 환경일 경우, http-proxy-middleware 라이브러리를 사용하거나 chrome의 allow CORS 확장 프로그램을 통해 해결할 수 있다.

2. CORS는 결국 브라우저 간에 이루어지기 때문에 프록시 서버를 구축해서 클라이언트와 서버 간에 징검다리 역할을 해줄 수 있다. 프론트에서는 `package.json`에 proxy를 수정하고 서버에서 `Access-Control-Allow-Origin` 를 설정한다.
