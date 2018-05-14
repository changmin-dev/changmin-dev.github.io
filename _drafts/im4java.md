## im4java
im4java는 커멘드로 사용하는 ImageMagick을 자바로 사용하도록 만들었다. 프로세스를 실행하는 방법으로 사용되는거 같다. JNI를 이용한 JMagick과는 다른 방법이다. [문서](http://im4java.sourceforge.net/index.html)가 JMagick보다 잘되어있다.
  
Maven을 통해서 의존성을 추가해주면 정상 동작 가능하다. MacOS sierra에서 IntelliJ를 사용하였고 Maven 메뉴를 이용해서 프로젝트를 생성했다. brew를 이용해서 사전에 ImageMagick을 설치한 것을 제외하고는 별도의 설정은 하지 않았다.
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

이미지 사이즈 크기를 바꾸는 명령을 테스트하였다.(convert -resize 200x300 Input.jpg Output.jpg에 해당한다.)이미지 파일의 위치는 IntelliJ에서 생성한 프로젝트 폴더 최상위에 위치한다. 
```java
import org.im4java.core.*;

public class TestIm4java {

    public static void main(String[] args) throws Exception{
        // 커멘드를 만든다. ImageMagick의 명령중하나인 convert에 해당한다.
        ConvertCmd cmd = new ConvertCmd();

        // Operation은 ImageMagick의 인자들에 해당한다.
        IMOperation op = new IMOperation();
        // resize함수는 순서가 뒤에 와도 상관이 없다. 하지만 입출력 파일의 순서는 지켜야한다.
        op.resize(200, 300);
        op.addImage("Input.jpg");
        op.addImage("Output.jpg");

        // 실행한다.
        cmd.run(op);

    }
}
```