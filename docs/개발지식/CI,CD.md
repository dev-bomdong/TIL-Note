# CI/CD

어플리케이션 개발부터 배포까지의 단계 (code => build => test => release => deploy)를 자동화해서 더 쉽고 빠르게 배포하는 것.

## CI (Continuous Integration) : 지속적인 통합

버그수정, 새로 만든 기능들이 main repo에 주기적으로 build,test, merge되는 것

### 원칙

1. 최소 단위의 코드 변경사항을 주기적으로, 자주 merge해야한다.
2. build, test, merge의 자동화

### 장점

mege 충돌을 피해 개발 생산성 향상
자동화 과정에서 문제점이 빠르게 발견되어 버그 수정이 용이하다.

## CD (Continuous Delivery(Deployment)) : 지속적인 제공(배포)

CI를 통해 자동으로 build, test된 코드가
(1) 배포 준비를 제대로 했는지 검증 후 수동으로 배포 : Continuous Delivery
(2) 배포 준비가 되자마자 자동으로 배포 :Continuous Deployment
