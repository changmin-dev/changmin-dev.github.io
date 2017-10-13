---
layout: post
title: docker에서 nginx 사용하기
---
docker pull nginx
docker run -t -i --name 컨테이너이름 -p 80:80 -d 이미지이름 /bin/bash

-t : 터미널 사용 
-i : 상호작용
둘다 써야함 뒤에 /bin/bash로 쉘을 정함
--name : 컨테이너의 이름을 정해준다.
-P : 포트를 연다. docker ps로 포트를 확인 가능(안될수도있음)
-p 80:80 컨테이너의 포트와 바인딩
-d : 백그라운드에서 실행하며, 컨테이너 id를 출력
nginx : 실행할 이미지 이름

ffmpeg -f mp4 -i "/root/TT.mp4" -f flv rtmp://localhost/live

### 참고 
http://blog.nacyot.com/articles/2014-01-27-easy-deploy-with-docker/
http://pyrasis.com/book/DockerForTheReallyImpatient/Chapter03
https://blog.docker.com/2014/06/why-you-dont-need-to-run-sshd-in-docker/
https://docs.docker.com/engine/reference/commandline/run/#options

컨테이너로 파일 복사
http://shy-blg.tistory.com/entry/Docker%EB%8F%84%EC%BB%A4-%EC%BB%A8%ED%85%8C%EC%9D%B4%EB%84%88%EC%97%90-%ED%8C%8C%EC%9D%BC-%EC%A0%84%EC%86%A1%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95

rtmp-nginx 도커
https://www.atlantic.net/community/howto/install-rtmp-ubuntu-14-04/

변경 사항이 있는 도커 포트
https://stackoverflow.com/questions/19335444/how-do-i-assign-a-port-mapping-to-an-existing-docker-container