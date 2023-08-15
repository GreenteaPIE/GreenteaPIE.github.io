---
layout: post
title: JAVA 용어 정리 - 17
date: 2023-08-15
excerpt: "자바 용어 정리 - Spring FrameWork와 Spring Boot "
tags: [java, study]
comments: true


---

#### Spring 과 Spring Boot의 차이점

##### Spring framework

자바 EE 어플리케이션을 빌드할 수 있는 오픈소스 경량 프레임워크이다. 스프링은 프레임워크의 프레임워크라 말할 수 있는데, 이는 다양한 프레임워크(Struts, JSP, Hibernate 등)에 대한 지원을 제공하기 때문이다.

##### Spring boot

Spring + boot(strap). 스프링 부트는 기존 스프링 프레임워크 위에 구축되어있다. 스프링 부트를 사용하면 독립 실행형(stand-alone) 프로덕션 등급(production-grade) 스프링 기반 어플리케이션을 쉽게 만들 수 있다. 스프링 프레임워크를 사용하기 위한 설정의 많은 부분을 자동화하여 사용자가 편하게 스프링을 활용할 수 있도록 돕는다.

##### 차이점

기본적으로 Spring은 경량 애플리케이션 프레임워크이며 Struts, JSP,Hibernate 등과 같은 다양한 프레임워크에 대한 지원을 제공하고 애플리케이션을 빌드하는데 사용되는 반면 Spring Boot는  스프링 - REST API 개발에 주로 사용되는 기반 프레임워크이다.

Spring의 주요 차이점 또는 주요 기능은 종속성 주입이며 Spring Boot의 경우 자동 구성이며 Spring Boot Framework 개발자의 도움으로 개발 시간, 개발자 노력을 줄이고 생산성을 높일 수 있다.

|             | Spring                                               | Spring Boot                                      |
| :---------: | ---------------------------------------------------- | ------------------------------------------------ |
|    사용     | 자바 기반 어플리케이션을 만드는 데 사용됨            | 주로 REST API 개발을 위해 사용됨                 |
|  주요기능   | 의존성 주입                                          | AutoConfiguration                                |
|  개발 유형  | 느슨하게 결합된 어플리케이션을 만드는데 도움이 된다. | 독립 실행형 어플리케이션을 만드는데 도움이 된다. |
| 서버 종속성 | 서버를 명시적으로 설정해야 함                        | Tomcat이나 Jetty와 같은 임베디드 서버를 제공     |
|    구성     | 수동으로 구성을 빌드해야 함                          | 부트스트랩 가능한 기본 구성이 있음               |

요약하자면 Spring Boot는 표준 Spring 프레임워크의 모든 기능을 포함하는 동시에 애플리케이션 개발을 훨씬 쉽게 만들어주는 프레임워크이고, Spring과 비교할 때 모든 Spring Boot 속성이 자동 구성되기 때문에 훨씬 더 짧은 시간에 애플리케이션을  시작하고 실행할 수 있다는 장점을 가진다.

##### Hibernate

Hibernate ORM(Object-relational mapping) : 자바 언어를 위한 객체 관계 매핑 프레임워크. 객체 지향 도메인 모델을 관계형 데이터베이스로 매핑하기 위한 프레임워크를 제공한다.

##### ORM(객체 관계 매핑)

데이터베이스와 객체 지향 프로그래밍 언어 간의 호환되지 않는 데이터를 변환하는 프로그래밍 기법. 객체 지향 언어에서 사용할 수 있는 "가상" 객체 데이터베이스를 구축하는 방법이다.

##### Bean scope

Bean이 존재할 수 있는 범위.<br>Bean으로 생성된 객체들은 스프링 컨테이너에서 함께 시작되어서 종료될 때까지 스프링이 관리해주는데 이 이유는 스프링 빈들은 singleton scope으로 관리되기 때문이다.

|     Scope      |                         Description                          |
| :------------: | :----------------------------------------------------------: |
|   singleton    | 하나의 Bean 정의에 대해서 Spring IoC Container 내에 단 하나의 객체만 존재한다. |
|   prototype    |   하나의 Bean 정의에 대해서 다수의 객체가 존재할 수 있다.    |
|    request     | 하나의 Bean 정의에 대해서 하나의 HTTP request의 생명주기 안에 단 하나의 객체만 존재한다. 즉 각각의 HTTP request는 자신만의 객체를 가진다. Web-aware Spring/ApplicationContext 안에서만 유효하다. |
|    session     | 하나의 Bean 정의에 대해서 하나의 HTTP request의 생명주기 안에 단 하나의 객체만 존재한다. Web-aware Spring ApplictionContext 안에서만 유효한다. |
| global session | 하나의 Bean 정의에 대해서 하나의 global HTTP request의 생명주기 안에 단 하나의 객체만 존재한다. 일반적으로 portlet context 안에서 유효하다. Web-aware Spring ApplicaionContext 안에서만 유효하다. |
|  application   | 하나의 Bean 정의에 대해서 ServletContext의 생명주기 안에 단 하나의 객체만 존재한다. Web-aware Spring ApplicationContext 안에서만 유효하다. |

##### Singleton

- 스프링 IoC 컨테이너에서 단 한 번 생성되어 캐시에 저장된다.
- 모든 후속 request와 bean 이름에 대한 참조는 캐시된 객체를 반환한다.
- 기본적으로 모든 bean은 scope이 명시되어 있지 않으면 singleton이다.

##### Prototype

- 모든 요청에서 새로운 객체를 생성하여 주입한다.
- stateful한 bean에는 prototype scope를 사용해야 하고 stateless한 bean에는 singleton scope를 사용한다.

만약 프로토타입 빈에 종속된 싱글톤 빈을 사용한다면 인스턴스화 시 종속성이 해결된다. 싱글톤 빈에 프로토타입 빈을 주입할 때, 먼저 프로토타입 빈이 인스턴스화 되고나서 싱글톤 빈에 주입된다. 그 프로토타입 빈은 싱글톤 빈에게 제공되는 유일한 인스턴스가 된다.

##### Request, session, and global session scoples

얘네는 오직 web-aware<br>Spring ApplicationContextimplementation(XmlWebApplicationContext)를 사용할 때만 이용할 수 있다. 얘네를 보통의 스프링 IoC 컨테이너에서 사용하려고 하면 알 수 없는 Bean scope에 대한 IllegalStateException 에러가 뜬다.<br>싱글톤과 프로토타입과 달리 얘네를 지원하기 위해서는 bean을 정의하기 전에 초기 구성이 몇개 필요하다.
