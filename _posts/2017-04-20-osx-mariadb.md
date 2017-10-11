---
layout: post
title: mac에서 mariadb 설치 및 실행
---
검색하면 많이 나오는 주제 이지만, 자주 잊기 때문에 작성한다.
brew를 통해 설치
```
brew install mariadb
```

서버 실행하기 및 접속
```
mysql.server start
mysql -u root
```

부팅시 자동으로 시작하기 위해서는 다음과 같이 한다.
```
brew services start mariadb
```



## 참고 
https://mariadb.com/kb/en/mariadb/installing-mariadb-on-macos-using-homebrew/