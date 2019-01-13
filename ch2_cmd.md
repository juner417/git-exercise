# snapshot status
* Untracked : Not in existing snapshot, New File
* Unmodified : already in existing snapshot, Not Modified
* Modified : changed file
* Staged : Add stage After Changed and Ready to commit

# git config
```
## listing configs
$ git config -l
$ git config --add user.name juner417
```

# add - change a file status to the staged 
git add <file> : Untracked -> Stage, Modified -> Stage

# status - check git status
git status 
git status -s/--short : short status

```
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	new file:   README
	modified:   README.md

 ✔ :~/dev/progit (master +)  kubernetes-dev-nucleo-admin@kubernetes-dev-nucleo
$ git status -s
A  README
M  README.md
?? .cmd.swp
?? cmd
```

# .gitignore
* 특정파일을 snapshot에 제외하고 싶을때 이 파일에 지정
* 아무것도 없는 라인이나, #로 시작하는 라인은 무시
* 표준 Glob 패턴을 사용한다. 
* (/)슬래시로 시작하면 하위 디렉터리에 적용되지 않는다(not recursive)
* 디렉터리는 슬래시(/)를 끝에 사용하는 것으로 표현한다. 
* 느낌표로 시작하는 패턴은 무시하지않고 포함한다. 
* 참고 : https://github.com/github/gitignore

# git diff - differ working directory(Unstaged) and Staging area(Staged)
* differ working directory and Staging area
```
$ git diff
diff --git a/README.md b/README.md
index f0a4d0c..f14f671 100644
--- a/README.md
+++ b/README.md
@@ -1,3 +1,4 @@
 # Test ProGit
 ##
 ### another modify
+## test
```

* differ in Staging area with last commited(--staged, --cached)
```
$ git diff --staged
diff --git a/README b/README
new file mode 100644
index 0000000..56266d3
--- /dev/null
+++ b/README
@@ -0,0 +1 @@
+My Project
diff --git a/README.md b/README.md
index 4cac55c..f0a4d0c 100644
--- a/README.md
+++ b/README.md
@@ -1 +1,3 @@
 # Test ProGit
+##
+### another modify
```

# git commit - create snapshot(Unmodified status)
```
$ git commit -m "exercise commit"
[master 48d864f] exercise commit
 2 files changed, 3 insertions(+)
 create mode 100644 README
```

# git commit option(skip to add)
```
$ git commit -a -m "exercise commit"
# only add in tracked file(Not Untracked file) 
```

# git rm
* file delete in snapshot(working directory -> Staged -(commit)> Untracked and delete) 
```
$ touch gitrm.test
$ git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)

	gitrm.test

nothing added to commit but untracked files present (use "git add" to track)

# commit for test
$ git add gitrm.test

$ git commit -m "test for git rm"
[master b8528d2] test for git rm
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 gitrm.test

$ git rm gitrm.test
rm 'gitrm.test'

$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	deleted:    gitrm.test

$ git commit -m "test for git rm2"
[master 5ef9b52] test for git rm2
 2 files changed, 1 deletion(-)
 delete mode 100644 README
 delete mode 100644 gitrm.test

$ ls -al
total 48
drwxr-xr-x   7 user  staff   224B 12 30 21:58 ./
drwxr-xr-x  49 user  staff   1.5K 12 27 21:50 ../
-rw-r--r--   1 user  staff    12K 12 30 21:56 .cmd.md.swp
drwxr-xr-x  12 user  staff   384B 12 30 21:58 .git/
-rw-r--r--   1 user  staff     6B 12 27 22:22 .gitignore
-rw-r--r--   1 user  staff    44B 12 27 22:21 README.md
-rw-r--r--   1 user  staff   2.1K 12 27 22:55 cmd.md

# --cached 옵션을 사용하면 working directory에서 지우지 않고 index에서만 삭제 가능
# git rm --cached gitrm.test
# cat <<EOF>> .gitignore 
# *.test
# EOF
# git commit -m "test for git rm3"
```

# git history - commit history
```
$ git log
commit 531586b01c5572ba701e62d25ee09704aaeb1802 (HEAD -> master)
Author: junho.son <junho.son@linecorp.com>
Date:   Sun Dec 30 22:04:01 2018 +0900

    test for git rm5

commit 2ac7f1dfbef09f818033b33ee89b4937eeac1f09
Author: junho.son <junho.son@linecorp.com>
Date:   Sun Dec 30 22:03:40 2018 +0900

    test for git rm4

commit b8528d2099cd6d6d4d9e3d57c40e57eb2e0129dd
Author: junho.son <junho.son@linecorp.com>
Date:   Sun Dec 30 22:02:35 2018 +0900

    test for git rmi3
...

## latest two commit
$ git log -p -2
commit 531586b01c5572ba701e62d25ee09704aaeb1802 (HEAD -> master)
Author: junho.son <junho.son@linecorp.com>
Date:   Sun Dec 30 22:04:01 2018 +0900

    test for git rm5

diff --git a/.gitignore b/.gitignore
index 1377554..36980ba 100644
--- a/.gitignore
+++ b/.gitignore
@@ -1 +1,2 @@
 *.swp
+*.test

commit 2ac7f1dfbef09f818033b33ee89b4937eeac1f09
Author: junho.son <junho.son@linecorp.com>
Date:   Sun Dec 30 22:03:40 2018 +0900

    test for git rm4

diff --git a/gitrm.test b/gitrm.test
deleted file mode 100644
index e69de29..0000000

## git log stat
$ git log --stat

## git pretty print
$ git log --pretty=onelien

## log formatting
$ git log --pretty=format:"%h - %an, %ar : %s"
https://git-scm.com/docs/pretty-formats
```

# git amend
* modify a commit message or add a file
```
## change the commit message after commiting
$ git commit --amend
## open edit and change message

## forgoted file to add after commiting
$ git commit -m "test"
$ git add forget_file
$ git commit --amend
## open edit and change message

```

# git reset
* change a file status from Staging to Unstaged
```
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   cmd.md

no changes added to commit (use "git add" and/or "git commit -a")

$ git add .

$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	modified:   cmd.md


$ git reset HEAD cmd.md
Unstaged changes after reset:
M	cmd.md

$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   cmd.md

no changes added to commit (use "git add" and/or "git commit -a")
```

# git checkout -- file 
* change a file to origin(not modified)
```
$ git checkout -- cmd.md
## return to original cmd.md file
```

# git remote
```
## show remote repository
$ git remote
$ git remote -v

## add remote repository
$ git remote add [repo_nickname] [repo_addr]

## show remote repository
$ git remote show [repo_nickname]

## rename remote repository
$ git remote rename [org_nick] [new_nick]

## delete remote repository
$ git remote rm [repo_nickname]
```

# git tag
* lightweight tag : 특정 commit에 대한 포인터
* annotated tag : 테그를 만든 사람 및 정보가 함께 포함된 tag를 만듬
```
$ git tag -a v0.1 -m 'version tag'

$ git tag
v0.1

$ git show v0.1
tag v0.1
Tagger: junho.son <junho.son@linecorp.com>
Date:   Mon Dec 31 13:19:59 2018 +0900

version tag

commit 32e0d51512d6a6a72e7c8fa1c747f85734f9b8c1 (tag: v0.1-lw, tag: v0.1)

## lightweight tag에는 -a -m 옵션을 사용하지 않는다. 
```

# git alias
* like shell alias
```
git config --global alias.co checkout
git config --global alias.br branch
```

```

## References
* Pro git chapter2
