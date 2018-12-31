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
