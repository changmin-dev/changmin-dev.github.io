---
laytout: post
title: 고성능 HTTP Server 검토
tags: [Server, HTTP]
---

성능이 매우 빠른 HTTP Server가 필요해서 찾아보았다.
[Benchmark 자료](https://www.techempower.com/benchmarks/#section=data-r16&hw=ph&test=plaintext)에서 계속 상위 랭크를 유지한 몇까지 에 대해서 검토해 봤다. DB는 쓰지 않을 예정이라 Plaintext항목만 확인했다.
주로 본인이 짧은 시간에 공부해서 사용할 수 있고 검색을 통한 트러블 슈팅이 가능 여부에 대해서 이다.

## Tokio-minihttp
Tokio라는 경량의 비동기 런타임 환경을 사용한 HTTP 라이브러리이다. 
- Rust 언어의 러닝 커브가 있는 편이다. Rust 경험이 있는 지인에게 시간이 많으면 시도해보라는 조언을 들었다.
- stackoverflow에는 Tokio관련 질문 250개 정도 Tokio-minihttp 관련 질문 60개 정도가 있고 
- Tokio의 경우는 문서가 잘되어 있다. Tokio-minihttp 자체는 문서는 따로 존재하지 않는다. 


## FastHTTP
Go의 `net/http`패키지와 유사하게 개발할 수 있는 고속의 웹 프레임 워크이다.
- Go 언어 자체가 접근성이 좋아서 빨리 배울 수 있다.
- `net/http`패키지에서 넘어오는 가이드도 제공하고 있다.
- GoDoc에 문서가 잘되어 있는 편이다.
- 구글 검색에 나오는 페이지가 좀 있다. 
- 스택 오버 플로우에 60개 정도 질문이 있다.

## ULib
C++로 된 어플리케이션 프레임워크, HTTP를 지원한다. 
- 짤막한 4페이지의 위키가 문서의 전부이다. 나의 Cpp능력으로는 Example만 보고 원하는 걸 만들 수 없을 거 같다. 
- 구글 검색에도 별로 나오지 않는다. 
- 스택 오버 플로우에 질문조차 없다.

## Rapidoid
Java로 된 고속 비동기 웹 프레임워크. 구글에서 만들었다.
- 자바는 어느 정도 다룰 수 있는 편이라 언어는 크게 문제가 아니다. 인터페이스가 간단하지만, 공부가 필요해 보인다.
- 문서는 잘되어 있는 편이다.
- 구글에 검색하면 관련 내용이 잘 나오지만, 스택오버플로우에는 10개 정도밖에 질문이 없다.

