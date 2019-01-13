# commit guideline
* [git project submittingpatch](https://github.com/git/git/blob/master/Documentation/SubmittingPatches) 참고 

## 공백문제 제거 
```
$ git diff --check
branch_master.py:5: trailing whitespace.
+print('This is a master branch')  
branch_master.py:6: trailing whitespace.
+print('say hello')  
```

## commit은 논리적으로 구분되는 changeset
* 최대한 수정사항을 하나의 주제로 요약해야 함
* 여러가지 이슈에 대한 수정사항을 하나에 커밋에 담지 않아야 한다. 
* 커밋 메세지를 잘 작성해야 함
** 메세지의 첫 라인에 50자가 넘지 않도록 정리한다. 
** 다음 한 라인은 비우고 그 다음 라인 부터 커밋을 자세히 설명한다. 
** 현재형 표현을 사용한다. 
** 명령문으로 시작하는것도 좋다. 
```
# commit message 확인
$ git log --no-merges
```

## 이메일로 받은 patch 적용하기
```
$ git apply /tmp/patch-client.patch
$ git apply --check /tmp/patch-client.patch
```

## format-patch 명령어로 만든 파일 patch
```
$ git am /tmp/test.patch
```

## remote 브랜치 등록 후 통합
```
$ git remote add test [remote repo url]
$ git fetch test
$ git merge/rebase ...
```

## 브랜치에서 master에 포함되지 않은 commit만 보기
```
$ git log [branchname] --not master 
$ git log master..[branchname]
## -p옵션을 주면 commit의 diff까지 확인가능
```

## 변경된 내용 확인
```
## master와 현재 branch의 공통 commit이후의 커밋을 비교함
$ git diff master...[currentbranchname]
```

## 빌드넘버 만들기
* git describe 기능 이용, [annotated 태그](./ch2_cmd.md)가 있어야 그거를 기준으로 만듬
```
## 사전에 annotated 태그 존재 해야 함
$ git describe master
v0.1-lw-17-g46fe785
```

## release 준비하기 
```
$ git archive master --prefix='project' | gzip > `git describe master`.tar.gz
```

## References
* Pro git chapter5
