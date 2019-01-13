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

## 범위로 commit 가르키기
### '..' double dot
* A..B : A에는 없지만 B에만 있는 commit들
```
$ git log A..b

## 현재 branch(HEAD)에는 있고 origin/master에 없는 commit
## push할 경우 여기에 나오는 commit들이 push된다. 
$ git log origin/master..HEAD

## 한쪽 refs를 생략하면 현재 branch에 HEAD라고 가정함
## 아래는 위와 동일한 경우
$ git log origin/master..

## upstream을 fetch하고 현재 branch와 비교할 경우...
$ git fetch upstream
...
$ git log FETCH_HEAD..(HEAD생략가능)

```

### 3개 이상의 refs 비교
* '..'의 경우 2개 refs만 비교가능
* 3개 이상의 refs를 비교하려면 '...', '^'. '--not' 을 사용하면 된다. 
```
## .., ^, --not 비교
$ git log refA..refB
$ git log ^refA refB
$ git log refB --not refA

## 3개 이상 비교
$ git log refA refB ^refC #refA, refB에는 있는데 refC에만 없는 것
$ git log refA refB --not refC 
```

### '...' triple dot(서로 다른 commit '만' 보여줌)
* A...B : A와 B refs에서 서로 다른 commit만 보여
```

```

