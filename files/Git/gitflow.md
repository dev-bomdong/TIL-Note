# Gitflow

## git branch 공동 관리
> 회사의 main branch는 모든 작업 및 테스트가 완료된 파일만 리뷰 후 merge가 가능하다. 
> feature-mypage 하위의 branch를 팀원들간에 공동으로 작업하고자 한다.
> local과 remote에 feature-mypage와 하위 branch feature-mypage-nav를 생성하는 방법을 알아본다.

```
remote = 회사측 repo
local = local 컴퓨터
```

## local과 remote에 branch 만들기

1. local에 `feature-mypage`를 만들고 add, commit, push한다.

2. remote에 `feature-mypage`가 생긴다.

3. local에서 `feature-mypage`로 checkout한 뒤 하위 branch를 만들고 add, commit, push한다.

4. remote에 각각의 하위 branch가 생성된다.
(아직 `feature-mypage`와 하위 branch가 연결되지 않았고, 각각 존재한다.)

5. local에서 `feature-mypage`에 checkout하고 `pull origin 하위 branch`를 진행한다. 그후 add, commit, push한다. 

6. remote의 `feature-mypage`에 각각의 하위 branch가 연결된 상태로 update된다. 

7. local에서 `pull origin feature-mypage`를 한다. 

## remote의 저장소 local로 가져오기

1. `$ git branch -a` 옵션을 주면 local과 remote의 모든 branch를 볼 수 있다.

2. remote의 branch를 local에서 checkout하고 관리하고 싶다면, `$ git checkout -t branch명(주로 origin/~으로 시작)`을 입력한다. -t 옵션과 remote의 branch 이름을 입력하면 local에 동일한 이름의 branch를 생성하면서 checkout한다.

앞선 과정을 이어가자면, `$ git checkout -t origin/feature-mypage-nav` 을 입력하면 된다.

## 작업 후 main branch로 push하기

feature-mypage-nav에서의 작업이 끝나 feature-mypage에 모두 합쳐졌다면, feature-mypage에서 main으로 push 후 PR을 올린다. 
