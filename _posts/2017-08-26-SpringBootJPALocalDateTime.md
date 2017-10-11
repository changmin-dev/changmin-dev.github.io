---
layout: post
title: Spring Boot JPA에서의 LocalDateTime과 그 친구들 처리하기
---

자바8에서 LocalDateTime이 추가 되었다. 이를 Spring Boot JPA에서 사용하려면 Jackson-datatype-jsr310이 필요하다.

아래는 메이븐에서 추가되는 의존성이다. 
```
<dependency>
    <groupId>com.fasterxml.jackson.datatype</groupId>
    <artifactId>jackson-datatype-jsr310</artifactId>
</dependency>
```


## 참고
[http://homoefficio.github.io](http://homoefficio.github.io/2016/11/19/Spring-Data-JPA-%EC%97%90%EC%84%9C-Java8-Date-Time-JSR-310-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0/)
[http://aoruqjfu.fun25.co.kr/index.php/post/1008](http://aoruqjfu.fun25.co.kr/index.php/post/1008)


