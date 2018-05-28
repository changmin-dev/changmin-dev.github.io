---
layout: post
title : Java 이미지 도구 비교
tags:
- JMagick
- im4java
- image
- Java
---
Java에서 이미지를 다룰 필요가 있었다.(gif 관련) 때문에 몇가지 도구를 비교해 보았다. 대상은 JMagick, im4java, 자바의 이미지 관련 클래스이다.

## 실행
[ImageMagick 문서](https://www.imagemagick.org/Usage/anim_basics/#Adjoin)에 있는 gif 분리 예제를 약간 변형해서 사용했다. 
```
convert ./images/masterbaby.gif +adjoin  ./images/splited/frame_%03d.gif
```
사용된 gif파일은 294프레임의 gif파일 이다.
![masterbaby](/images/masterbaby.gif)

### im4java
```java
ConvertCmd cmd = new ConvertCmd();

IMOperation operation = new IMOperation();
operation.p_adjoin();
operation.addImage("./images/masterbaby.gif");
operation.addImage("./images/splited/frame_%03d.gif");

cmd.run(operation);
```

### JMagick
```java
ImageInfo imageInfo = new ImageInfo("./images/masterbaby.gif");
MagickImage magickImage = new MagickImage(imageInfo);

MagickImage[] frames = magickImage.breakFrames();

for(int i = 0; i < frames.length; ++i){
     frames[i].setFileName(String.format("./images/splited/frame_%03d.gif", i));
     frames[i].writeImage(imageInfo);
}
```
## 비교 하기
성능 측정은 `System.currentTimeMillis()`을 이용했다.
### im4java
- [문서](http://im4java.sourceforge.net/docs/dev-guide.html)가 잘되어 있다.
- 빌드가 쉽다. (Maven이나 Gradle같은 도구로 의존성 추가만 하면 빌드 가능) 또한 배포도 쉽다.
- 명령어를 API로 사용하는 거라서 직관적이지 않다. 하지만 ImageMagick에 익숙하면 개발이 빨라질거 같다. 
(ImageMagick문서를 보고 테스트 수행 -> im4java를 사용. api문서를 안봐도 gif 다루는데는 10분 정도 밖에 안걸렸다.) 
- 상대적인 성능 차이(약 100~500밀리 세컨드가 느리다.)
- ImageMagick을 익히면 im4java도 복잡한게 아니라면 구현 가능할꺼 같다.

### JMagick
- [문서](http://downloads.jmagick.org/jmagick-doc/)는 API만 있음. 
- 빌드가 힘들다. (별도의 JMagick 빌드가 필요, 추가 설정 필요), 로컬에서는 IDE에서 설정하면 되지만, 배포까지 하려면 Gradle에도 의존성 설정이 필요하다.(./gradlew build 불가)
- 인터페이스는 im4java에 비해서 직관적인 편이지만 그렇게 직관적인 편인건 아니다. API 문서에서 필요한걸 못찾아서 레포에서 키워드 검색으로 찾았다.(GIF, Frame.. 등등 검색) 레퍼런스도 없었다 ㅠ 나머지시간을 모두 여기에 쓴거 같다. 
- 명령어에 종속되는 것이 아니라서 상대적으로 자유롭게 사용할 수 있다는것이 장점(프레임 별로 이미지를 다룰 수 있음, 픽셀 접근 가능)
- 상대적인 성능 차이(약 100~500밀리 세컨드가 빠르다.)


## Java의 이미지관련 클래스(주로 javax.imageio)

### 실행하기
위의 두 라이브러리와 같은 동작을한다.
```java
ImageReader reader = ImageIO.getImageReadersByFormatName("gif").next();
ImageInputStream in = ImageIO.createImageInputStream(new FileInputStream("./Images/masterbaby.gif"));
reader.setInput(in, false);

int count = reader.getNumImages(true);
for(int i = 0; i < count; ++i){
     BufferedImage image = reader.read(i);
      ImageIO.write(image, "GIF", new File(String.format("./Images/splited/frame_%03d.gif", i)));
}
```

### 비교
- 문서는 [공식 API문서](https://docs.oracle.com/javase/7/docs/api/javax/imageio/package-summary.html) 혹은 [레퍼런스](https://www.tutorialspoint.com/java_dip/index.htm) [사이트 들](https://www.programcreek.com/java-api-examples/?class=javax.imageio.ImageIO&method=createImageOutputStream)을 봐야함
- 기본 패키지를 사용하므로 빌드나 배포에 문제가 없다.
- 인터페이스는 공식 API이므로 자바스타일에 익숙하다면 큰문제는 없다.
- 프레임별로 gif를 다룰 수 있고 픽셀도 접근가능하다.
- 성능은 im4java보다 비슷하거나 약간 느리다.
- ImageMagick에서 제공하는 다양한 조작은 직접 구현해야 할 수도 있다.

## 결론
대상으로 정한 외부 라이브러리의 경우 설정 및 구현에 중점에 둔다면 im4java, 성능 및 조작에 중점을 둔다면 JMagick
약간이라도 Special한 조작이 필요한 경우 가능하면 별도 라이브러리를 사용하는게 좋을거 같다. im4java를 사용하는 경우 프레임, 픽셀 접근을 위해서 보조적으로 사용해도 될거 같다. 당시에 수행하는 과제가 배포는 상관없었기 때문에 픽셀 조작이 용이한 JMagick으로 선택했다.