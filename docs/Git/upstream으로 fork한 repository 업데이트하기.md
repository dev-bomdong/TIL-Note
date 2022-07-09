# upstream으로 fork한 repository 업데이트하기

내가 fork한 repository의 원본에 변경 사항이 있을경우, 자동으로 업데이트되지는 않는다.

그렇다고 해서 repository를 삭제하고 다시 fork를 하는 건 너무 번거로우니, upstream을 이용해 업데이트 하는 방법을 알아보자.

**참고사항**

```bash
- 원본 repo = 원본 repository
- fork repo = fork한 repository
```

### 1. 내 로컬 PC에 fork repo를 clone한다.

`$ git clone (fork repo 주소 : https://~ 형태)`

<br />

### 2. clone 한 프로젝트 디렉토리로 이동해 remote 상태를 확인한다.

`$ git remote -v`

```javascript
origin (fork repo 주소) (fetch)
origin (fork repo 주소) (push)
```

<br />

### 3. 리모트 저장소에 원본 repo 추가

`$ git remote add upstream (원본 repo 주소)`

`$ git remote -v` 명령어를 입력해보면 upstream으로 원본 저장소가 추가된 것을 확인할 수 있다.

```javascript
origin (fork repo 주소) (fetch)
origin (fork repo 주소) (push)
upstream (원본 repo 주소) (fetch)
upstream (원본 repo 주소) (fetch) (push)
```

<br />

### 4. 원본 repo에서 fetch

`$ git fetch upstream`

<br />

### 5. 원본 repo에서 merge

`$ git merge upstream/main`

<br />

### 6. fork repo로 push한다.

`$ git push`

<br />

### 7. GitHub 홈페이지의 fork repo에서 refresh한다.
