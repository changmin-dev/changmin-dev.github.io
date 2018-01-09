---
layout: post
title: Spring AOP
---
# AOP(Aspect Oriented Programming)
Aop 다양한 작업에서 중복적으로 필요한 작업을 분리 해준다.

## Core Concerns(핵심 관심, 주 업무)
요청에 따라서 수행되는 작업

## Crosscutting Concern(횡단 관심, 보조 업무)
로깅, 트랜젝션과 같은 작업, Core Concerns 앞,뒤 중간에 수행되는 작업들

## JoinPoint 
삽입가능한 위치, 포인트 컷의 후보가 된다.   
1. 함수 - Spring에서 지원 나머지는 x
2. 변수변경
3. 객체로딩
4. 예외발생

## PointCut
어떤 패키지 어떤클래스의 어떤 메소드에 적용할 것인가를 결정
아래와 같은 방법으로 정의한다.
```
execution(리턴타입 패키지..클래스..메서드(매개변수))
```
### 리턴타입
|   *   | 모든 리턴타입을 허용한다.        |
___________________________________
| void  | 리턴타입이 void               |
___________________________________
| !void| 리턴타입이 void 인것을 제외한다.   |

### 패키지
|package.name| package.name패키지 선택
|packege.name..|package.name으로 시작하는 패키지 선택
|package.name..last |package.name으로 시작하면서 마지막 패키지 이름이 impl으로 끝나는 패키지 

### 클래스
|ClassName|ClassName클래스 선택|
_____________________________________________
|*Name|클래스 이름이 Name으로 끝나는 클래스만 선택|
_____________________________________________
|Class+|해당클래스로 부터 파생된 모든 자식클래스|
_____________________________________________
|Interface+|해당인터페이스를 구현한 모든 클래스 |

### 메서드
*(..)|모든 메서드 선택
메서드*(..)||메서드 이름이 method로 시작하는 모든 메서드 선택

### 매개변수 
(..)|
(*)|
(package.name.Class)| 해당클래스를 매개변수로 가지는 메소드 ,패키지 이름이 반드시 필요
(!package.name.Class| 해당클래스를 매개변수로 가지지 않는 메소드 
(Class, ..) 한개이상의 메소드를 가지되, 첫번째 매개변수가 Class
(Class, *) 두개의 메소드를 가지고 첫번째 매개변수 타입이 Class

## advice
joinPoint에 삽입되어 동작할수 있는 코드, 시점을 정해 주어야 한다.
### Before
포인트 컷에 해당하는 메소드 실행전 동작
### After
1. After Returning - 성공적으로 리턴되면 동작
2. After Throwing - 
3. After - 포인트컷에 해당하는 메소드 실행후 성공 여부에 관계없이 동작 
### Around advise 
함수 앞뒤에 사용가능한 advice정의
advise = advice + pointcut1
포인트 컷에 해당하는 메소드의 호출을 가로 챈다.
ProceedingJoinPoint를 매개변수로 받아서 proceed()를 호출해 주면 원래 메소드로 간다. 

## Weaving
적용 되는 시점, 컴파일 타임(컴파일 오래걸림 실행시 빠름, 낭비) vs 로딩(클래스 로드 될때, 불필요하게 될 수 있음)vs런타임(함수 호출될때 필요할때만 사용하지만 느려짐) 스프링의 경우는 런타임이다.





