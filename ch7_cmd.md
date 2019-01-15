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
$ git log A...B # A,B에 서로 다른 commit만 보여줌

### --left-right 옵션을 이용하여 확인
$ git log HEAD...trackingbranch --oneline --left-right
> 7068794 (trackingbranch) commit for tracking only
< adda9b4 (HEAD -> master) summary ch7
< 8bf7d5a start ch7 -- changed
< 5b8e4e4 ch6 start
< 46fe785 test
< 39d23ee (tag: v0.5-a) summary for ch5
< 9a94b5b (tag: v0.3) done ch3
```

## 대화형 명령
* add -i(--interactive) 옵션을 사용하여 상태 변경 가능

### update(add staging area)
```
## interactive 사용
$ git add -i
           staged     unstaged path
  1:    unchanged       +11/-1 ch7_cmd.md

*** Commands ***
  1: status	  2: update	  3: revert	  4: add untracked
  5: patch	  6: diff	  7: quit	  8: help
What now> 2   # update
           staged     unstaged path
  1:    unchanged       +11/-1 ch7_cmd.md
Update>> 1    # choose a file
           staged     unstaged path
* 1:    unchanged       +11/-1 ch7_cmd.md
Update>>
updated 1 path  # update done(unstaged -> stage)

*** Commands ***
  1: status	  2: update	  3: revert	  4: add untracked
  5: patch	  6: diff	  7: quit	  8: help

```

### revert(delete a file on staging area)
```
What now> 3
           staged     unstaged path
  1:       +11/-1      nothing ch7_cmd.md
Revert>> 1
           staged     unstaged path
* 1:       +11/-1      nothing ch7_cmd.md
Revert>>
reverted 1 path
What now> 1
           staged     unstaged path
  1:    unchanged       +11/-1 ch7_cmd.md

*** Commands ***
  1: status	  2: update	  3: revert	  4: add untracked
  5: patch	  6: diff	  7: quit	  8: help

```

### diff
* interactive 모드는 'git diff --cached'와 동일
```
## add file (to the staging area)
What now> 2
           staged     unstaged path
  1:    unchanged       +63/-0 ch7_cmd.md
Update>> 1
           staged     unstaged path
* 1:    unchanged       +63/-0 ch7_cmd.md
Update>>
updated 1 path

## git diff --cached...
What now> 6
           staged     unstaged path
  1:       +63/-0      nothing ch7_cmd.md
Review diff>> 1
diff --git a/ch7_cmd.md b/ch7_cmd.md
index 19bc888..ff72c26 100644
--- a/ch7_cmd.md
+++ b/ch7_cmd.md
@@ -87,6 +87,69 @@ $ git log refA refB --not refC
 ### '...' triple dot(서로 다른 commit '만' 보여줌)
 * A...B : A와 B refs에서 서로 다른 commit만 보여
 ```
+$ git log A...B # A,B에 서로 다른 commit만 보여줌
...

```

### add --patch
* 일부분만 add(to the staging area)
```
What now> 5
           staged     unstaged path
  1:    unchanged       +11/-1 ch7_cmd.md
Patch update>> 1
           staged     unstaged path
* 1:    unchanged       +11/-1 ch7_cmd.md
diff --git a/ch7_cmd.md b/ch7_cmd.md
index 19bc888..cb06e14 100644
--- a/ch7_cmd.md
+++ b/ch7_cmd.md
@@ -87,6 +87,16 @@ $ git log refA refB --not refC
 ### '...' triple dot(서로 다른 commit '만' 보여줌)
 * A...B : A와 B refs에서 서로 다른 commit만 보여
 ```
-
+$ git log A...B # A,B에 서로 다른 commit만 보여줌
+
+### --left-right 옵션을 이용하여 확인
+$ git log HEAD...trackingbranch --oneline --left-right
+> 7068794 (trackingbranch) commit for tracking only
+< adda9b4 (HEAD -> master) summary ch7
+< 8bf7d5a start ch7 -- changed
+< 5b8e4e4 ch6 start
+< 46fe785 test
+< 39d23ee (tag: v0.5-a) summary for ch5
+< 9a94b5b (tag: v0.3) done ch3
 ```

Stage this hunk [y,n,q,a,d,e,?]? ?
y - stage this hunk
n - do not stage this hunk
q - quit; do not stage this hunk or any of the remaining ones
a - stage this hunk and all later hunks in the file
d - do not stage this hunk or any of the later hunks in the file
e - manually edit the current hunk
? - print help

## y선택시 해당 구간 add
## 위 내용과 동일한 cmd, 실행시 동일한 interactive out 나옴
$ git add -p
$ git add --patch
```

### stash and cleansing
```
$ git status
On branch master
Your branch is ahead of 'lineghe/master' by 6 commits.
  (use "git push" to publish your local commits)

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   ch7_cmd.md

no changes added to commit (use "git add" and/or "git commit -a")

## stash
$ git stash # OR
$ git stash save
Saved working directory and index state WIP on master: f218618 typo

## stash list보기
$ git stash list
stash@{0}: WIP on master: f218618 typo

## stash 한 내용 다시 적용
$ git stash apply
On branch master
Your branch is ahead of 'lineghe/master' by 6 commits.
  (use "git push" to publish your local commits)

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   ch7_cmd.md

no changes added to commit (use "git add" and/or "git commit -a")

## stash list는 남아있음 drop으로 지워야 함
$ git stash list
stash@{0}: WIP on master: f218618 typo

$ git stash drop
Dropped refs/stash@{0} (c09420af4145b12f23c2ba7cc4e9b9c1886b05ba)

## git stash pop(apply + drop)
$ git stash pop

## staging area에 있는 내용은 stash하지 않음 
$ git stash --keep-index

## git clean (working dir clean)
$ git clean #stage에 들어가지 않은 내용들 삭제
$ git clean -n #dry-run
$ git clean -n -d -x #.gitignore에 명시한 내용까지 삭제 


```

### search
#### grep
```
$ git grep -n test
...
$ git grep --count test # 지정한 문자 개수 count
$ git grep -p r.discover # 지정한 문자가 포함된 함수 찾기 
```

#### search a log
```
## describe 단어가 처음 들어간 commit 확인 
$ git log -Sdescribe --oneline
5b8e4e4 ch6 start

$ git show 5b8e4e4
...
```

### history 단장하기
#### commit 수정
```
$ git commit --amend
```

#### commit message 여러개 수정
```
$ git rebase -i HEAD~3

## 현재 HEAD 2~3 이전의(서버에 push하지 않은) commit message를 바꾸려고 할때 
$ git rebase -i HEAD~3
# 대화형 화면이 나오면 pick을 edit으로 수정하고 닫음
$ git commit --amend
$ git rebase --continue
# git rebase --edit-todo 를 실행하면 그 다음 수정할 commit으로 이동
## 대화형 화면에서 commit 순서를 바꿀수도 있음 

```
## References
* Pro git chapter7
