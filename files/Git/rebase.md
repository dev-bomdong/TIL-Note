# rebase

: re + base = base를 다시 재배치하는 개념

main branch에서 feature branch가 파생되었다고 할 때, main / feature branch가 공통으로 가지고 있는 커밋이 base가 된다.

이 때 feature branch가 rebase를 한다면?

base를 재배치 (=feature의 모branch인 main branch의 가장 최근 커밋을 feature의 base로 변경) 한다는 것

```javascript
// rebase 전
A - B - C - D <br/> //main
        ㄴ E   //feature

// feature 브랜치에서 main 브랜치를 rebase
$ git checkout feature
$ git rebase main

A - B - C - D - *E

// main 브랜치에서 feature 브랜치를 rebase
$ git checkout main
$ git rebase feature

A - B - C - *E - D
//주의 : main 브랜치의 D와 feature 브랜치의 E에서 발생하는 conflict를 해결할 때 D가 유실될 수 있어 유의해야한다.
```
