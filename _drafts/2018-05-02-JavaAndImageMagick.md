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
im4java는 ImageMagick을 사용하기 위한 순수 자바 인터페이스이다. 원래 커멘드로 사용하는 ImageMagick을 자바 함수로 사용하도록 만들었다.
  
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
Maven을 이용해서 쉽게 빌드가 가능했던 im4java와 달리 JMagick은 많은 시행 착오가 있었다.
처음에는 빌드를 어떻게 해야하지 몰랐고 구버전의 브랜치를 보니 알고 있는 configue파일이 있어서 구버전으로 시도했다. imagemagic의 위치를 찾지 못하는 현상이 발생했다. 
하지만 마찬가지로 jni.h의 위치를 찾지 못하는 현상이 발생했다.리눅스와 OS X와 빌드 방법이 다를지도 모른다고 생각해서 구글에서 찾아 봤다. eadme에서 docker를 이용해서 빌드하는 방법이 있어서 그대로 수행했지만, Make.def파일 찾지 못한다고 나왔다. 원래 알던 configue가 아니라 configue.ac가 있어서 찾아보니 [좋은 설명](http://kthan.tistory.com/entry/%EB%A6%AC%EB%88%85%EC%8A%A4-Linux-autotool%EB%A1%9C-%EC%86%90%EC%89%BD%EA%B2%8C-Makefile-%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0-autoscan-autoconf-automake-etc)이 있어서 어느 정도 이해하게 되었다. 내가 알고 있는  

결론적으로 이슈를 읽어보니 2017년 기준으로 master branchImageMagick 최신버전으로 바이너리 빌드가 되는 것을 알게 되었고
다음과 같은 방법으로 Make가 가능하게 되었다.(먼저 autoconf와 automake의 설치가 필요하다.)
```
autoreconf --force --install
automake --add-missing
./configure
`autoreconf`는 autotool들을 사용하는데 필요한 파일이 오래 되었거나  다시 만들어 준다.[참고](https://www.gnu.org/software/autoconf/manual/autoconf-2.68/html_node/autoreconf-Invocation.html)

```
 하지만 `./configure`단계에서내가 사용하고 있는 맥의 java path를 찾지 못해서 [과거에 찾아본 내용](https://www.snip2code.com/Snippet/1458563/ImageMagick-and-JMagick-install-on-Mac-O) 을 참고해서 옵션을 추가 해주었다.
```
./configure --with-java-home=/System/Library/Frameworks/JavaVM.framework/Versions/Current
```
실제 java의 경로를 알고 싶다면 `which java`명령어를 사용하면 되고 링크된 경로가 나올 수 있으므로 `readlink 자바의 링크 경로`명령어로 자바의 위치를 알 수 있다. Home의 경로가 필요하므로 맞는 상위 경로를 지정해 주면 된다.(보통은 include, lib같은 폴더가 있는 곳이 Home이다.)

이후에는 원래 알던 Make를 사용하면 빌드가 된다.
```
make clean
make
```

성공적으로 빌드가 되었지만 프로젝트에서는 빌드가 되지 않았다. 문제는 java.library.path에서 JMagick을 찾는DJava="경로"를 이용한 빌드도 해보았고 System.set.property()도 시도해 봤지만 되지 않았다.

링크가 필요했다.

```
sudo ln -s /Users/changmin/Projects/jmagick/lib/libJMagick.so /Library/Java/Extensions/libJMagick.jnilib
```

1. JMagick Repo의 master branch에서 autoreconf, automake
2. ./configure
3. make clean, make
4. 빌드된 .so파일을 .jnilib파일로 링크한다.
5. IDE에서 빌드된 .jar파일 추가 