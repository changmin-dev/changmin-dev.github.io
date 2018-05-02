---
layout: post
title: Java에서 ImageMagick사용하기
---

## ImageMagick
ImageMagick은 이미지를 새로 만들거나 고치는데 사용되는 오픈 소스 소프트웨어다. 커맨드 라인 형태로 사용이 가능하고 x
OSX에서는 brew를 이용해서 설치가 가능하다.
```
brew install imagemagick
```

## im4java
im4java는 ImageMagick을 사용하기 위한 순수 자바 인터페이스이다.  
Maven을 통해서 다음과 같이 설정했다.
```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>im4javaTest</groupId>
    <artifactId>im4javaTest</artifactId>
    <version>1.0-SNAPSHOT</version>

    <dependencies>
        <!-- https://mvnrepository.com/artifact/org.im4java/im4java -->
        <dependency>
            <groupId>org.im4java</groupId>
            <artifactId>im4java</artifactId>
            <version>1.4.0</version>
        </dependency>
    </dependencies>
</project>
```



## JMagick
바이너리가 필요한거 같다.