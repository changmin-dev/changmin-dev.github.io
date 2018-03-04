---
laytout: post
title: JAVA의 콘솔 출력 테스트 하기  
---

보통 `System.out.print()` 함수를 이용해서 콘솔 출력을 하는데 여기에 있는 것을 테스트 하는 방법이 필요했다.
`System.setOut()`함수를 사용해서 출력을 설정 할 수 있다. `PrintStream`형을 입력으로 받고 `PrintStream`형은 `ByteArrayOutputStream`을 입력으로 받는다. 참고로 `System.out`은 `PrintStream` 형이다. 
```java
ByteArrayOutputStream consoleOutput = new ByteArrayOutputStream();
System.setOut(consoleOutput)
```

이제 `System.out.print()`를 사용하면 `consoleOutput`으로 출력이 들어 온다. `consoleOutput`은 `toString()` 함수를 지원 하므로 다음과 같은 방식이 가능하다.
```java
@Test
public TestConsole_defaultIO(){
    assertEquals(System.out.print('hi'), consoleOutput.toString());
}
```

[참고-junit이 아니라 testing을 사용](http://openwritings.net/content/public/excerpt/unit-test-standard-output-systemoutprintln)