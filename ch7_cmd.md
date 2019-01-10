# git tools

## revision 보기    
```
git log 
...
$ git log --oneline --decorate --graph --all
* 5b8e4e4 (HEAD -> master, origin/master, lineghe/master) ch6 start
* 46fe785 test
* 39d23ee (tag: v0.5-a) summary for ch5
* 9a94b5b (tag: v0.3) done ch3
* dadfa31 (trackingbranch) other branch for rebase
* def4456 test tracking branch

$ git show 5b8e4e4
commit 5b8e4e44d65b40d2c0af9af2cd895444cafaf889 (HEAD -> master, origin/master, lineghe/master)
Author: juner417 <juner84s@gmail.com>
Date:   Sun Jan 6 22:28:14 2019 +0900

    ch6 start

diff --git a/ch5_cmd.md b/ch5_cmd.md
index 6add23b..4e4fe5b 100644

...

### hash값 줄여서 보기 
$ git log --abbrev-commit --pretty=oneline
5b8e4e4 (HEAD -> master, origin/master, lineghe/master) ch6 start
46fe785 test
39d23ee (tag: v0.5-a) summary for ch5
9a94b5b (tag: v0.3) done ch3
dadfa31 (trackingbranch) other branch for rebase

### branch 이름으로 확인(아래 결과 동일)
$ git show trackingbranch
$ git show dadfa31

### branch가 가르키는 commit sha1값 확인 
$ git rev-parse trackingbranch
dadfa31284708872cdf543259ea1768b894105a2

### 계통관계 가르키기
commit~, commit^ : 해당 커밋의 부모를 가르킴
$ git show HEAD~1
$ git show HEAD^1

$ git show HEAD~2
$ git show HEAD^^
```
