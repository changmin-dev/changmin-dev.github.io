---
layout: post
title: 소켓 통신으로 만드는 HTTP Hello World 페이지 서버 
---

예전에 Java의 소켓을 이용해서 HTTP 리퀘스트 처리를 하는 연습을 해봤지만, 기억이 잘 안 나는 거 같아서   
다시 해보려고 한다. 저번에는 참고자료를 많이 봤지만 이번에는 정말로 내가 하나씩 찾아가면서 하는 것이 목표이다.

## 소켓 통신

### 서버
1. 서버에서는 소켓을 만들어 주소 및 포트를 bind 한다.
2. listen을 하면서 연결을 기다린다.
3. 클라이언트의 요청이 들어오면 클라이언트와 통신을 하기 위한 소켓을 만든다.
4. 클라이언트와 통신(read\write)을 한다.
5. 통신이 완료되면 종료한다.

### 클라이언트
1. 접속하려는 서버의 주소와 포트를 이용해 소켓을 만든다.
2. 소켓을 connect 한다.
3. connect에 성공하면 서버와 통신(read\write)을 한다.
4. 통신이 완료되면 종료한다.

## HTTP
HTTP는 Hyper Text Transfer protocol로 원래는 문서를 전달하기 위한 프로토콜이지만 확장되어 웹에서 다양한 정보를 주고받을 수 있는 규약이다.   
[최초의 HTTP 문서](https://www.w3.org/Protocols/HTTP/AsImplemented.html)에서 연결은 TCP 같은 연결 지향 서비스를 이용한다고 되어있다.
> 하이퍼 텍스트는 참조를 통해 다른 문서로 이동할 수 있는 문서이다. 참조는 하이퍼링크를 말한다. 

> 하이퍼 링크는 하이퍼 텍스트 문서에서 모든 형식의 자료를 가리킬 수 있는 참조 고리이다.

### HTTP Request
요청 메시지는 [Method URL Protocol]의 형태이다.
```
GET / HTTP/1.1
```

### HTTP Response
응답 메시지가 나오고 2번의 줄 바꿈 후에 본문이 나온다.
```
HTTP/1.1 200 OK
\n
\n
Hello World
```

## 구현하기
클라이언트는 웹 브라우저 같은 HTTP Request를 보낼 수 있는 프로그램이 된다.

### JAVA
Java의 경우는 `java.net` 패키지에 있는 `ServerSocket`과 `Socket`을 사용했다. `ServerSocket`은 서버에서 사용하는 소켓으로 생성시 bind와 listen과정을 거친다. 구현을 추적해보면(IntelliJ의 Go to 메뉴를 이용) native메서드를 사용하고 있다. 이후 accept()를 이용해서 클라이언트의 connect요청이 들어오면 입력을 받기 위한 소켓(connectSocket)을 가져온다. 

```java
//생성자를 bind 찾아 보면 내부에서 bind하고 다시 찾아가면 listen(synchronized)을 하고 있다.
ServerSocket listenSocket = new ServerSocket(5000, 50);

Socket connectSocket = listenSocket.accept();

Scanner requestScanner = new Scanner(connectSocket.getInputStream());

//Http Request Message는 "Method URL Protocol"의 형태이다. 예를 들면 "GET / HTTP/1.1"
String[] requests = requestScanner.nextLine()
                                .split(" ");
String protocol = requests[2].trim();// HTTP/1.1

//서버에서 나오는 출력은 소켓에서 OutputStream을 가지고 와서 출력한다.
OutputStream responseStream = connectSocket.getOutputStream();

//Http Response Message가 나오고 2번의 줄바꿈 이후에 본문이 나온다.(Hello)
StringBuilder responseMessage = new StringBuilder();
responseMessage.append(protocol + " 200 OK");
responseMessage.append("\n");
responseMessage.append("\n");
responseMessage.append("Hello World");

responseStream.write(responseMessage.toString()
                    .getBytes());

connectSocket.close();
listenSocket.close();
```
