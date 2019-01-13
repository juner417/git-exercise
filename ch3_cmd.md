# branch
## git branch 
```
$ git branch testing
$ git branch 
* master
  testing
## HEAD 포인터 이동은 안됨

$ git log --online --decorate
5b32670 (HEAD -> master, tag: v-ch2, testing) done ch2
9da5987 (tag: v0.2-lw, origin/master) change cmd
32e0d51 (tag: v0.1-lw, tag: v0.1) add ch2 contents
531586b test for git rm5
2ac7f1d test for git rm4
...

$ git checkout testing
$ git log --oneline --decorate
5b32670 (HEAD -> testing, tag: v-ch2, master) done ch2
## HEAD 포인터 이동


$ git commit -a -m "made a change"
[testing 06daf8b] made a change
 1 file changed, 4 insertions(+)
 create mode 100644 test.py



$ git log --oneline --decorate
06daf8b (HEAD -> testing) made a change
5b32670 (tag: v-ch2, master) done ch2
9da5987 (tag: v0.2-lw, origin/master) change cmd

## change branch to master
$ git checkout master
Switched to branch 'master'

## change HEAD commit
$ git log --oneline --decorate
5b32670 (HEAD -> master, tag: v-ch2) done ch2
9da5987 (tag: v0.2-lw, origin/master) change cmd
32e0d51 (tag: v0.1-lw, tag: v0.1) add ch2 contents
```

## make other change
```
$ git checkout master

$ vi branch_master.py
$ git add .
$ git commit -m "made other change"

$ git log --oneline --decorate
1ab42d8 (HEAD -> master) made other change
5b32670 (tag: v-ch2) done ch2
9da5987 (tag: v0.2-lw, origin/master) change cmd
32e0d51 (tag: v0.1-lw, tag: v0.1) add ch2 contents
531586b test for git rm5

## commit history 확인 
$ git log --oneline --decorate --graph --all
* 1ab42d8 (HEAD -> master) made other change
| * 06daf8b (testing) made a change
|/
* 5b32670 (tag: v-ch2) done ch2
* 9da5987 (tag: v0.2-lw, origin/master) change cmd
* 32e0d51 (tag: v0.1-lw, tag: v0.1) add ch2 contents
* 531586b test for git rm5
```

## merge branch(fast-forward)
```
$ git checkout -b iss53
Switched to a new branch 'iss53'

$ vi index.html
$ git commit -m "added a new footer [issue 53]"
[iss53 0122215] added a new footer [issue 53]
 1 file changed, 3 insertions(+)
 create mode 100644 index.html

$ git log --oneline --decorate --graph --all
* 0122215 (HEAD -> iss53) added a new footer [issue 53]
* df75ddb (master) commit for current staged
*   3fd3a42 (origin/master) Merge branch 'testing'

$ git checkout master
Switched to branch 'master'

## create hotfix
$ git checkout -b hotfix
Switched to a new branch 'hotfix'

$ git log --oneline --decorate --graph --all
* 0122215 (iss53) added a new footer [issue 53]
* df75ddb (HEAD -> hotfix, master) commit for current staged
*   3fd3a42 (origin/master) Merge branch 'testing'

$ vi index.html
$ git commit -a -m "fixed ther broken email address"
[hotfix 0f3265e] fixed ther broken email address
 1 file changed, 5 insertions(+)
 create mode 100644 index.html

$ git log --oneline --decorate --graph --all
* 0f3265e (HEAD -> hotfix) fixed ther broken email address
| * 0122215 (iss53) added a new footer [issue 53]
|/
* df75ddb (master) commit for current staged
*   3fd3a42 (origin/master) Merge branch 'testing'

## merge hotfix to the master
$ git checkout master
Switched to branch 'master'

$ git merge hotfix
Updating df75ddb..0f3265e
Fast-forward
 index.html | 5 +++++
 1 file changed, 5 insertions(+)
 create mode 100644 index.html

$ git log --oneline --decorate --graph --all
* 0f3265e (HEAD -> master, hotfix) fixed ther broken email address
| * 0122215 (iss53) added a new footer [issue 53]
|/
* df75ddb commit for current staged
*   3fd3a42 (origin/master) Merge branch 'testing'

```

## merge conflict(unmerged)
```
$ git checkout master

$ git merge iss53
Auto-merging index.html
CONFLICT (add/add): Merge conflict in index.html
Automatic merge failed; fix conflicts and then commit the result

$ git status
On branch master
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)

	both added:      index.html

no changes added to commit (use "git add" and/or "git commit -a")

## change a conflict file
$ vi index.html

$ git add index.html

$ git status
On branch master
All conflicts fixed but you are still merging.
  (use "git commit" to conclude merge)

Changes to be committed:

	modified:   index.html

$ git commit
[master 40c8a6b] Merge branch 'iss53'

$ git log --oneline --decorate --graph --all
*   40c8a6b (HEAD -> master) Merge branch 'iss53'
|\
| * 0122215 added a new footer [issue 53]
* | 0f3265e fixed ther broken email address
|/
* df75ddb commit for current staged

## git branch options
$ git branch -v # show last commit
* master 40c8a6b Merge branch 'iss53'

$ git branch --merged # show merged branch
$ git branch --no-merged # show no merged branch
$ git branch -d testing # delete branch on local repo

```

## rebase
* 3way-merge가 아닌, 현재 브랜치에서 rebase를 위해 가져온 branch를 patch로 만들어서 적용한뒤 
* 현재 브랜치의 변경내용을 차례대로 적용한다.
* commit history 관리가 편함
```
## master branch 
$ git commit -am "prepare rebase"
[master c5ab272] prepare rebase
 1 file changed, 1 insertion(+)

## git graph 확인
$ git log --oneline --decorate --graph --all
* c5ab272 (HEAD -> master) prepare rebase
| * 4313efb (trackingbranch) test tracking branch
|/
* 1dd7f13 (origin/master) summary for ch3
*   40c8a6b Merge branch 'iss53'
|\
| * 0122215 added a new footer [issue 53]
* | 0f3265e fixed ther broken email address
|/
* df75ddb commit for current staged

## 다른 브랜치 변경
$ git checkout trackingbranch
Switched to branch 'trackingbranch'

$ vi index.html

$ git commit -m "other branch for rebase"
[trackingbranch da22cf6] other branch for rebase
 1 file changed, 1 insertion(+)

$ git log --oneline --decorate --graph --all
* da22cf6 (HEAD -> trackingbranch) other branch for rebase
* 4313efb test tracking branch        # trackingbranch
| * c5ab272 (master) prepare rebase   # master
|/
* 1dd7f13 (origin/master) summary for ch3
*   40c8a6b Merge branch 'iss53'
|\
| * 0122215 added a new footer [issue 53]
* | 0f3265e fixed ther broken email address
|/
* df75ddb commit for current staged

## rebase
$ git rebase master
First, rewinding head to replay your work on top of it...
Applying: test tracking branch
Applying: other branch for rebase

$ git log --oneline --decorate --graph --all
* dadfa31 (HEAD -> trackingbranch) other branch for rebase
* def4456 test tracking branch
* c5ab272 (master) prepare rebase   # master branch가 앞에 rebase됨
* 1dd7f13 (origin/master) summary for ch3
*   40c8a6b Merge branch 'iss53'
|\
| * 0122215 added a new footer [issue 53]
* | 0f3265e fixed ther broken email address
|/
* df75ddb commit for current staged

## master로 이동
$ git checkout master
Switched to branch 'master'

$ git status
On branch master
nothing to commit, working tree clean

$ git log --oneline --decorate --graph --all
* dadfa31 (trackingbranch) other branch for rebase
* def4456 test tracking branch
* c5ab272 (HEAD -> master) prepare rebase   # HEAD가 master로 이동
* 1dd7f13 (origin/master) summary for ch3
*   40c8a6b Merge branch 'iss53'
|\
| * 0122215 added a new footer [issue 53]
* | 0f3265e fixed ther broken email address
|/
* df75ddb commit for current staged

## rebase한 branch merge
$ git merge trackingbranch
Updating c5ab272..dadfa31
Fast-forward
 index.html | 3 ++-
 1 file changed, 2 insertions(+), 1 deletion(-)

$ git log --oneline --decorate --graph --all
* dadfa31 (HEAD -> master, trackingbranch) other branch for rebase  # HEAD 이동
* def4456 test tracking branch
* c5ab272 prepare rebase
* 1dd7f13 (origin/master) summary for ch3
*   40c8a6b Merge branch 'iss53'
|\
| * 0122215 added a new footer [issue 53]
* | 0f3265e fixed ther broken email address
|/
* df75ddb commit for current staged
```

## References
* Pro git chapter3
