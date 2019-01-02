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
