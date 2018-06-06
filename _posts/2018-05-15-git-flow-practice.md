---
layout: post
title : Git Flow
tag : [git]
---

Git-flow가 어떻게 진행되는지 대략적으로 알고 있었으나 제대로 써본 적은 없어서 연습해 보았다. 
연습은 masOS의 콘솔에서 수행했고 [Cheatsheet](https://danielkummer.github.io/git-flow-cheatsheet/index.ko_KR.html)을 참고했다.  

## 시작하기  
macOS에서는 brew를 이용해서 설치한다.
```
brew install git-flow-avh
```

로컬 저장소를 만들고 원격 저장소를 추가한다. 그리고 git flow를 초기화한다.
```
git init
git remote add origin [repo 주소]
git flow init  
```
이후 네이밍등을 묻는데 네이밍은 기본값이 권장 사항이다. 따로 정한 규칙은 없으므로 기본값을 사용한다.
버전 tag는 0.1로 하였다.  
hook과 filter의 디렉토리를 확인하는데 복잡하지 않도록 기본값을 사용하였다. 
[hook](https://git-scm.com/book/ko/v1/Git%EB%A7%9E%EC%B6%A4-Git-%ED%9B%85)은 git에서 이벤트가 일어 났을 때 실행하는 스크립트이다. filter는 아마 [filter-branch](https://git-scm.com/book/ko/v1/Git-%EB%8F%84%EA%B5%AC-%ED%9E%88%EC%8A%A4%ED%86%A0%EB%A6%AC-%EB%8B%A8%EC%9E%A5%ED%95%98%EA%B8%B0#filter-branch%EB%8A%94-%ED%8F%AC%ED%81%AC%EB%A0%88%EC%9D%B8)를 의미하는 거 같다. filter-branch는 커밋을 대량 수정할 때 사용한다.
```
How to name your supporting branch prefixes?
Feature branches? [feature/] 
Bugfix branches? [bugfix/] 
Release branches? [release/] 
Hotfix branches? [hotfix/] 
Support branches? [support/] 
Version tag prefix? [] 0.1
Hooks and filters directory? [/Users/changmin/Projects/hackday/git-flow/.git/hooks] 
```
마치면 자동으로 develop 브랜치가 추가되어 있고 checkout된 것을 확인 가능하다.

## 기능(feature) 추가 하기
다음 명령을 입력하면 feature/f1이라는 브랜치가 생성되고 checkout된다. f1이라는 이름은 임으로 정했다. 
```
git flow feature start f1
```
이제 기능을 추가한다. 간단하게 파일을 만드는 변경 사항을 적용했다. cheatsheet에는 없지만 add와 commit은 알아서 해야한다. 
```
touch f1.txt
git add *
git commit -m "f1 구현"
```
추가한 기능 f1을 원격 저장소에 배포한다. finish를 하면 branch는 사라지니 주의 해야한다.
```
git flow feature publish f1
```
원격 저장소는 github을 이용했다. 다음과 같이 push가 되어 있는것을 확인할 수 있다.
<img width="464" alt="git-flow-practice1" src="https://user-images.githubusercontent.com/7522327/40048762-fcdabc16-586d-11e8-93ee-a2635e4fc944.png">

다른 계정으로 마크다운을 추가하고 원격 저장소의 내용을 바꾸고 다시 pull하였다.
```
git flow feature pull origin f1
```
origin은 저장소 이름이다. 일단 위의 명령으로 가져왔지만, depreacted이고 `git flow feature track MYFEATURE`을 사용하는 것을 권장한다고 한다. 

## 기능 추가 완료
이제 기능 추가를 완료 했다고 생각하고 종료했다. 종료하면 feature 브랜치는 사라진다.
```
git flow feature finsh f1
Switched to branch 'develop'
Merge made by the 'recursive' strategy.
 f1-added.md | 1 +
 f1.txt      | 0
 2 files changed, 1 insertion(+)
 create mode 100644 f1-added.md
 create mode 100644 f1.txt
```

## 릴리즈
기능 추가가 완료 되었다면 릴리즈한다. publish를 통해서 저장소에도 보낼수 있다. release1은 임의로 정한 이름이다.
```
git flow release start release1
git flow release publish release1
```

## 릴리즈 끝내기
다음과 같이하면 vi를 통해서 코멘트 내용을 입력받는다. 이후에는 태그 내용을 입력받는다. 
```
git flow release finish release1
```

## Hotfix
출시된 버전에 문제가 발생하면 핫픽스를 이용한다. master 브랜치에서 hotfix 브랜치가 생긴다. folder라는 이름으로 만들면 hotfix/folder로 브랜치가 생긴다.
```
git flow hotfix start folder
```

변경 사항을 반영하고 핫픽스를 끝내면 comment와 tag를 vi를 통해 물어본다.
```
mkdir f1
git mv f1-added.md f1.txt ./f1 
git commit -m "f1 폴더 추가, 파일을 폴더로 이동"
git flow hotfix finish folder
```
# 후기
설명으로만 들었을때는 복잡했는데 실제로 하니까 도구의 지원인지는 몰라도 간단하게 처리 되었다. 혼자 할때는 번거로울 수도 있지만 같이할때는 유용할것 같다. 특히 규모가 있는 팀에서 유용할거 같다. 소규모 팀에서 단순하게 진행하려면[github-flow](https://guides.github.com/introduction/flow/)도 나쁘지 않은것 같다.
