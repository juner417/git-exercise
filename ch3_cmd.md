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

