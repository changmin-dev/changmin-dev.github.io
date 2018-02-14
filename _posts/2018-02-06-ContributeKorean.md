---
laytout: post
title: Flask 한글 문서에 Pull Request를 보내기 위해서 경험한 것들
---

[Flask 한글 문서](http://flask-docs-kr.readthedocs.io/ko/latest/)에서 오타를 찾아서 수정 후 Pull Request를 했다. 문서를 계속 보다가 `.rst`파일에 문법이 적용이 안된 부분이 있었다. 그래서 Pull Request를 닫고 변경사항을 반영하고 Pull Request를 다시 했다. 두번째 변경사항을 반영하기 위해서 모험을 해서 기록해 놓으려고 한다.   

## reStructuredText
`.rst` 파일의 정체 부터 알아야 했다. `reStructuredText` 문서의 포맷이었다. reStructuredText는 [파이썬](https://www.python.org/dev/peps/pep-0287/)에서 문서 포맷으로 사용하기로 한 포맷이다.
[2017년도 파이콘](https://www.pycon.kr/2017/program/149)에서 관련 발표가 있었다. `마크다운`은  많은 에디터에서 뷰어를 기본으로 제공하지만, `reStructuredText`는 플러그인을   

## Sphinx
[Sphinx](http://www.sphinx-doc.org/en/stable/domains.html)는 파이썬 문서 생성기다. html, LaTex등의 문서를 만들 수 있다. `.rst`파일을 Sphinx를 이용해서 html로 만들 수 있다. 한글화 문서도 이를 이용하고 있었다. 
내가 보던 문서에서 `:meth:~`부분이 노출 되었는데 이는 [Sphinx의 도메인](http://www.sphinx-doc.org/en/stable/domains.html)이었다. 역시 이런 포맷의 특성상 공백이 중요한데 공백이 빠져있어서 도메인이 노출되었다.

## Rebase하기 
변경사항은 반영 했지만 다시 Pull Request를 하기 전에 Rebase를 해야 할것 같아서 Rebase를 했다.    
다음 명령어는 최근 2개 커밋을 Rebase한다. 
```
git rebase -i HEAD~2
```

이후에 vi화면이 나오면서 다음과 같이 최근 부터의 커밋이 나온다. 
```
pick [hash] 커밋 메시지(이전)
pick [hash] 커밋 메시지(최근)
# 주석 설명들
```

최근의 커밋을 제외한 나머지 커밋의 `pick`을 `squash`로 바꾸어 준다.
```
pick [hash] 커밋 메시지(이전)
squash [hash] 커밋 메시지(최근)
# 주석 설명들
```
저장을 하면 다시 vi화면이 나오고 커밋 메시지를 작성하라고 한다. 메시지를 작성하고 저장하면 완료.


## 원격 저장소에 푸쉬한 내역 취소하기
Rebase하기 전에 이미 Fork된 브랜치에 푸쉬를 보내버려서 Rebase이후의 내역이 원격에 반영 되지 않았다. 그래서 푸쉬된 커밋을 취소해야 했다.   
```
 git reset HEAD^
 git push --force
```

## 원격 저장소에 있는 브랜치 삭제 
이전에 만든 소스트리에서 원격 브랜치 삭제가 안되서(내가 할 줄 모르는거 같다.) 명령어로 삭제 했다.    
```
git push <브랜치 이름> --delete
```

## 후기
짧은 내용을 고치기 위해서 많은 것을 공부했다. 조금 더 파이썬과 관련된 환경과 git에 익숙해 질 수 있는 기회 였다. 그리고 해당 레포의 가장 최근 Merge가 작년 8월이라서 Pull Request를 받아 주실지 조금은 걱정된다.

## 본문에 링크를 하지 않은 참고 자료들
- [Git-브랜치-Rebase하기](https://git-scm.com/book/ko/v1/Git-%EB%B8%8C%EB%9E%9C%EC%B9%98-Rebase%ED%95%98%EA%B8%B0)
- [원격 브랜치 삭제](https://www.lesstif.com/pages/viewpage.action?pageId=20776547)
- [파이썬 코드 문서화 하기](http://www.hanul93.com/python-sphinx/)
- [rebase로 커밋 합치기](http://ko.gitready.com/advanced/2009/02/10/squashing-commits-with-rebase.html)
- [Git 취소](Git add 취소, git commit 취소, git reset 취소, git push 취소)