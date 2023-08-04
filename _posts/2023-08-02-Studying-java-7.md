---
layout: post
title: JAVA 용어 정리 - 7
date: 2023-08-02
excerpt: "자바 용어 정리 - API의 정의와 종류, 장 단점"
tags: [java, study]
comments: true


---

#### API 의 정의

API 는 "Application Programming Interface" 의 준말. 풀이 하자면, 여러 프로그램들과 데이터베이스, 그리고 기능들의 상호 통신 방법을 규정하고 도와주는 매개체이다. API는 데이터베이스가 아니지만, 엑세스 권한이 있는 앱의 규정과 "서비스 요청"에 따라 데이터나 서비스 기능을 제공하는 메신저 역할을 한다. 그럼 왜 그냥 데이터베이스를 연결하지 굳이 이런 매개체가 필요한가?

##### Web의 진화와 API

API는 필요성은 Web의 진화와 밀접한 연관이 있으니 잠깐 살펴보면, 모놀리틱 아키텍처(Monolithic Architecture)가 주도적이었던 Web 1.0 시대에서는 (현재에도 많이 쓰이고 있다.)서버와 클라이언트가 분리되지 않고 모두 서버에서 동시에 처리하기 때문에 API 필요성이 그다지 절실하지 않았다.

그러나 2000년경부터 시작된 Web 2.0의 "개방, 참여, 공유" 의 정신을 바탕으로 정보가 쌍방향으로 소통하고 "사용자가 생성한 데이터" 를 위주로 웹 앱의 붐, 그리고 2010년대 들어서 클라우드(Cloud) 기반 인프라와 MSA(Microservice Architecture)의 사용이 확산되면서 API 확산이 가속화되었고 이제 API 에서 가장 흔한 구조인 REST 또는 RESTful API가 점차 새로운 웹 생태계의 기반으로 주목된 것이다.

API를 개인적으로는 이렇게 이해한다. : "request-to-service", 한마디로 "각자 권한 분야에서 각자 필요한 것만 연계하기를  가능하게 해주는 서비스"

예를 하나 들어보자면 하나의 꽃집(데이터베이스)이 있다. 꽃들과 다양한 꽃에 대한 정보(꽃 공급 농장주, 꽃 이름, 색깔, 가격, 이번 달 매출 물량과 내역서... 등)를 데이터베이스에 들어있는 데이터라고 보자.

이곳에 일하는 사람(꽃집 관리자) = API

꽃집 방문 손님이나 꽃집 회계사, 주인, 파트너 등(요청자/애플리케이션)

보통 손님과 회계사는 액세스 권한이 다르다. 같은 데이터베이스(꽃집)로 접속하나, 각자 영역에 따라 조회/사용 영역이 다른 것이다. 이런 권한과 영역, 엑세스 등이 API 명세서와 인증 Authentication에서 관리된다. 그 조건하에서 각자 CRUD - Create(생성), Read(조회), Update(갱신), Delete(삭제) 기능을 활용하여 각자의 앱을 구축하고 운영하는 것이 가능하다. 전체 데이터베이스는 이러한 조각조각 다양한 데이터 변화들과 기능들을 총괄적으로 저장하고 연결하는 레포지토리라고 볼 수 있다.

#### API 종류

> A. 접근 방식에 따른 유형

##### Private API

Private API는 내부 API로, 기업이나 연구 단체 등에서 자체 제품과 운영 개선을 위해 단체 내부에서만 사용. 따라서 제 3자에게 노출되지 않는다.

##### Public API

Public API는 말 그대로 public, 즉 개방형 API로, 모두에게 공개된다. Public API 중에서도 접속하는 대상에 대한 제약이 없는 경우를 OpenAPI라 한다.

###### 오픈 API 사이트 예)

- 구글 :[ https://cloud.google.com/apis?hl=ko](https://cloud.google.com/apis?hl=ko)
- 공공데이터포털 :[https://www.data.go.kr/](https://www.data.go.kr/)
- 문화데이터 광장 :[https://www.culture.go.kr/data/main/main.do](https://www.culture.go.kr/data/main/main.do)
- 카카오 :[https://developers.kakao.com/](https://developers.kakao.com/)

##### Parter API

Partner API는 특정 비즈니스 파트너 간의 데이터 공유. 그러므로 동의하는 특정인들만 사용 할 수 있다.

> B. 아키텍처 스타일에 따른 API 종류

SOAP, RPC, REST API, 그리고 GraphQL

각각 유형과 기능, 보완 지원방식 등에 차이점이 있다. 현재 시점에서는 이중 REST/RESTful API와 GraphQL를 더욱 살펴보기를 권장하고, 이 글에서는 현재 사실상 표준화가 되었다 해도 과언이 아닐 정도로 가장 많이 쓰이고 있는 REST* API(REST 를 기반으로 만들어진 API)를 중점으로 설명한다.

"REST"는 REpresentational State Transfer의 약자로 자원을 이름으로 구분하여 해당 자원의 상태를 주고 받는 모든 것을 의미한다.

즉 REST 란

1. HTTP URI(Uniform Resource Identifier)를 통해 자원(Resource)을 명시하고, 
2. HTTP Method(POST, GET, PUT ,DELETE, PATCH 등) 을 통해
3. 해당 자원(URI)에 대한 CRUD Operation을 적용하는 것을 의미합니다.

`CRUD Operation`이란 CRUD는 대부분의 컴퓨터 소프트웨어가 가지는 기본적인 데이터 처리 기능인 Create(생성), Read(읽기), Update(갱신), Delete(삭제)를 묶어서 일컫는 말로 REST에서의 CRUD Operation 동작 예시는 다음과 같다.

- Create : 데이터 생성(POST)
- Read : 데이터 조회(GET)
- Update : 데이터 수정(PUT, PATCH)
- Delete : 데이터 삭제(DELETE)

#### REST 구성 요소

REST 는 다음과 같은 3가지로 구성이 되어있다.

1. 자원(Resource) : HTTP URI
2. 자원에 대한 행위(Verb) : HTTP  Method
3. 자원에 대한 행위의 내용 (Representations) : HTTP Message Pay Load

REST의 특징

1. Server-Client(서버-클라이언트 구조)
2. Stateless(무상태)
3. Cacheable(캐시 처리 가능)
4. Layered System(계층화)
5. Uniform Interface(인터페이스 일관성)

#### REST의 장단점

##### 장점

- HTTP 프로토콜의 인프라를 그대로 사용하므로 REST API 사용을 위한 별도의 인프라를 구출 할 필요가 없다.
- HTTP 프로토콜의 표준을 최대한 활용하여 여러 추가적인 장점을 함께 가져갈 수 있게 해준다.
- HTTP 표준 프로토콜에 따르는 모든 플랫폼에서 사용이 가능하다.
- Hypermedia API의 기본을  충실히 지키면서 범용성을 보장한다.
- REST API 메시지가 의도하는 바를 명확하게 나타내므로 의도하는 바를 쉽게 파악 할 수 있다.
- 여러 가지 서비스 디자인에서 생길 수 있는 문제를 최소화한다.
- 서버와 클라이언트의 역할을 명확하게 분리한다.

##### 단점

- 표준이 자체가 존재하지 않아 정의가 필요하다.
- HTTP Method 형태가 제한적이다.
- 브라우저를 통해 테스트할 일이 많은 서비스라면 쉽게 고칠 수 있는 URL 보다 Header 정보의 값을 처리해야 하므로 전문성이 요구된다.
- 구형 브라우저에서 호환이 되지 않아 지원해주지 못하는 동작이 많다.(익스플로어)

#### REST API란?

REST API란 REST의 원리를 따르는 API를 의미한다. 하지만 REST API를 올바르게 설계하기 위해서는 지켜야 하는 몇가지 규칙이 있으며 해당 규칙을 알아보도록 하자.

##### REST API 설계 예시

1. URI는 동사보다는 명사를, 대문자보다는 소문자를 사용하여야 한다.
   - Bad Example http://test.com/Running/
   - Good Example http://test.com/run/
2. 마지막에 슬래시 (/)를 포함 하지 않는다.
   - Bad Example http://test.com/test
   - Good Example http://test.com/test
3. 언더바 대신 하이폰을 사용한다.
   - Bad Example http://test.com/test_test
   - Good Example http://test.com/test-test
4. 파일 확장자는 URI에 포함하지 않는다.
   - Bad Example http://test.com/photo.jpg
   - Good Example http://test.com/photo
5. 행위를 포함하지 않는다.
   - Bad Example http://test.com/delete-post/1
   - Good Example http://test.com/post/1

#### RESTful 이란?

RESTful 이란 REST의 원리를 따르는 시스템을 의미한다. 하지만 REST를 사용했다 하여 모두가 RESTful 한 것은 아니다. REST API의 설계 규칙을 정확하게 지킨 시스템만이 RESTful 하다고 할 수 있다. 예를 들어, 모든 CRUD 기능을 POST로 처리하는 API나  URI 규칙을 제대로 지키지 않은 API는 REST API를 사용하기는 했지만, RESTful하지 못한 시스템이라고 볼 수 있다.

#### API의 장단점

##### 데이터 접속의 표준화와 편의성

위의 접근 방식에 따른 public API 와 partner API 등의 종류들에서 알아보았듯 API는 모든 접속을 표준화 하기 때문에 디바이스/운영체제 등과 상관없이 조건만 맞다면, 말하자면 범용 플러그 처럼 누구나 동일한 액세스를 약속한다. 또 조직에서 애플리케이션을 개발할 때 기능적 API를 사용하면, 필요한 기본 기능들 - 인증, 통신, 지불 처리 및 위치 확인 등 을 매번 자가 개발/업데이트할 필요 없이 손쉽게 이용할 수 있는 장점이 있다. 즉, 더 이상 수레바퀴를 만들 때마다 매번 재발명할 필요가 없다.

##### 자동화와 확장성

API를 통한 CRUD 처리에 따라 관련 데이터와 콘텐츠가 자동으로 생성되고 사용자의 환경에 맞춰서 정보가 전달되어 개발 워크플로우가 간소화 되고 애플리케이션 확장이 다소 용이하다. 예를 들자면 API를 활용한 웹 사이트 분석, 프로젝트 및 팀 관리 도구, 온라인 결제 시스템, 기타 여러 운영 솔루션에 필요한 애플리케이션을 다소 쉽게 작성 가능하다.

##### 적용력

API는 변화 예측에도 큰 도움이 되기 때문에 API를 통해 데이터를 수집하고 전달하는 데 있어 유연한 서비스 환경을 구축하고 소프트 웨어를 통합하고자 할 때, 그리고 개발자들 간의 협업이 필요할 때 더욱 용이하다.

#### API의 단점

##### 보안성과 HTTP 방식의 제한

가장 주목할 것은 API의 단일 진입점인 API 게이트웨이 해커의 타겟 대상이 될 수 있다는 점이다. 한마디로 API의 장점 - 평범한 HTTP 메서드를 사용하여 액세스 할 수 있다는 점이 보안성에 관해서는 반면 크나큰 단점이 된다. 

이 때문에 다른 일반적 인터넷 기반 리소스와 마찬가지로 메시지 가로채기 공격(man-in-the-middle), CSRF(Cross-Site Request Forgery) 공격, 크로스 사이트 스크립팅(Cross Site Scripting, XSS), SQL 삽입 공격(SQL injection), DDoS(Denial-of-service attack) 공격 (서비스 거부 공격) 등에 취약한 것이 사실이다. 또한 이러한 HTTP method는 메서드 형태가 다소 제한적이라는 문제점이 있다.

##### 표준의 부재와 개발 비용

REST API의 설계에 있어 가장 큰 단점이라고 할 수 있는 점이 공식화된 표준이 존재하지 않는다는 점이다. 그러므로 관리가 어렵고 실제로 API 기능을 구현하고 제공하려면 개발 시간, 지속적인 유지 관리 요구 사항 및 지원 제공 측면에서 비용이 많이 들 수 있따. 또한 기존 API의  기능을 확장하려고 할 때 광범위한 프로그래밍 지식이 필요한다.

