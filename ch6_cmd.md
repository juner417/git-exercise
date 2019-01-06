# github 사용하기 
## 참조 
* issue와 pr을 참조하기 위한 방법
* 모든 Pr와 issue는 프로젝트 내에서 유일한 번호를 하나 할당 받는다. 
* '#<number>'를 사용하여 comment를 담
* fork 저장소의 경우 'username#<number>' 'username/repo#<number>'

## markdown
* github flavored markdown을 사용한다. 
### task list
```
## task list 
- [X] write the code
- [ ] write all the tests

```
### code highlight
```
## write code
...
```

### 인용
* '>' 사용

### emoji
* :<name>:


## pull request의 Ref
* github의 pr merge 버튼 말고 로컬에서 merge하고 테스트를 해보고 싶은 경우 
* remote를 등록하지 않고 쉽게 pr을 fetch 할 수 있다. 
* 해당 브랜치에 pr이 열려 있으면 아래의 두 branch가 보인다. 
** refs/pull/num/head(pull request가 가르키는 마지막 커밋)
** refs/pull/num/merge(github merge 버튼으로 머지 할때 적용되는 결과 branch)
```
$ git ls-remote [repo_url/master/upstream]
...
f1c9a15b8fd88792d450c8892560a3ec75ce0c0b	refs/pull/1451/head
b1ce938c0d3d157dcaa6755c2d936ae3b0c60c67	refs/pull/1451/merge

$ git fetch origin refs/pull/1451/head
From https://.../progit-exercise
 * branch           refs/pull/1451/head -> FETCH_HEAD
```
> 하지만 이렇게 FETCH_HEAD를 가지고 merge할 경우 merge commit이 이상해짐
> .git/config를 수정하여 항상 pr을 전부 브랜치로 가져올수 있음

```
$ vi .git/config
## 새로운 refspec 추가
[remote "origin"]
	url = https://github.com/juner417/git-exercise.git
	fetch = +refs/heads/*:refs/remotes/origin/*
	fetch = +refs/pull/*/head:refs/remotes/origin/pr/*
...
$ git fetch 
...
 * [new ref]        refs/pull/1/head -> origin/pr/1
...

$ git checkout pr/1
```

