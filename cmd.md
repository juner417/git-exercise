# snapshot status
* Untracked : Not in existing snapshot, New File
* Unmodified : already in existing snapshot, Not Modified
* Modified : changed file
* Staged : Add stage After Changed and Ready to commit

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

