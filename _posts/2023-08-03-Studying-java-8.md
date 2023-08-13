---
layout: post
title: JAVA 용어 정리 - 8
date: 2023-08-03
excerpt: "자바 용어 정리 - Http와 Https의 개념과 차이점"
tags: [java, study]
comments: true


---

#### HTTP

##### HTTP(Hypertext Transfer Protocol)

- HTTP(Hypertext Transfer Protocol)은 서로 다른 시스템들 사이에 통신을 주고 받게 해주는 가장 기본적인 프로토콜이다.
- 웹 서핑을 할 때 서버에서 자신의 브라우저로 데이터를 전송해주는 용도로 많이 사용되며, 서버-클라이언트 모델에 맞춰 데이터를 주고 받기 위한 프로토콜이다.
- 인터넷 초기에 모든 웹 사이트에서 기본적으로 사용되었던 프로토콜이었다.
- <u>80</u>번 포트를 기본적으로 사용하고 있다.

> `프로토콜` 이란?
>
> 컴퓨터 내부에서 또는 컴퓨터 사이에 데이터 교환 방식을 정의하는 규칙 세계이다. 기기 간 통신은 교환되는 데이터의 형식에 대해 상호 합의를 요구하며 이런 형식을 정의하는 규칙의 집합이다.

##### HTTP의 구조

- HTTP는 애플리케이션 레벨의 프로토콜이기 때문에 TCP/IP 위에서 동작한다. HTTP는 상태를 가지지 않은 Stateless 프로토콜이며 Method, Path, Version, Headers, Body 등으로 구성된다.

##### HTTP의 단점

- HTTP는 암호화가 되지 않은 평문 데이터를 전송하는 프로토콜이기 때문에 HTTP로 비밀번호나 주민등록번호처럼 중요한 정보를 주고 받으면 제 3자가 정보를 조회할 수 있다.
- 이런 보안의 문제를 해결하기 위해 HTTPS가 등장하게 되었다.

#### HTTPS

##### HTTPS(Hypertext Transfer Protocol Secure)

- HTTPS는 HTTP에 데이터 암호화가 추가된 프로토콜이다.
- HTTPS는 HTTP와 다르게 443포트를 기본으로 사용하며 네트워크 상에서 제 3자가 볼 수 없도록 암호화 하여 전송한다.
- HTTPS 프로토콜은 SSL(보안 소켓 계층)을 이용해서 HTTP의 보안상 문제를 해결했으며 SSL은 서버와 브라우저 사이에 안전하게 암호화된 연결을 만들 수 있게 도와주고 서버 브라우저가 민감한 정보를 주고 받을 때도 도난당하는 것을 막아준다.
- <u>443</u>번 포트를 기본적으로 사용하고 있다.

##### HTTPS의 보안 방식

- SSL 인증서를 HTTP 프로토콜에 추가하여 만들어진 것이 HTTPS이며, SSL 인증서는 사용자가 사이트에 제공하는 정보를 암호화하는데 이용한다.
- 따라서 제 3자가 데이터를 훔쳐낸다고 하더라도 데이터가 암호화처리 되어 있기 때문에 해동을 할 수 없어 데이터를 읽을 수 없다.
- 그리고 HTTPS는 TLS(전송 계층 보안) 프로토콜을 사용해서 보안을 유지한다. TLS는 데이터 무결성을 보장해서 데이터가 전송 중에 수정되거나 손상되는 것을 방지하며 사용자가 접속하려는 웹사이트와 통신하고 있음을 증명할 수 있는 인증 기능도 가지고 있다.

##### HTTPS의 대칭키 암호화와 비대칭키 암호화

1. 대칭키 암호화
   - 클라이언트와 서버가 동일한 키를 사용해서 암호화와 복호화를 진행한다.
   - 키가 노출되면 매우 위험할 수 있지만 연산속도가 빠른 장점이 있다.
2. 비대칭키 암호화
   - 1개의 쌍으로 구성된 공개키와 개인키를 이용해 암호화와 복호화를 진행한다.
   - 키가 노출되어도 비교적 안전할 수 있지만 연산속도가 느린 단점이 있다.
3. HTTPS는 대칭키 암호화와 비대칭키 암호화 방식 둘 다를 사용한다.

##### HTTPS의 비대칭키 암호화에서 공개키와 개인키

1. 비대칭키 암호화에서 공개키와 개인키 암호화 방식을 이용해 데이터 암호화를 진행하며 공개키와 개인키는 서로를 위한 1쌍의 키이다.
   - 공개키 : 모두에게 공개 가능한 키이다.
   - 개인키 : 나만 가지고 있어야 하는 키이다.
2. 공개키와 개인키를 암호화하면 다음과 같은 이점을 얻을 수 있다.
   - 공개키 암호화를 하면 개인키로만 복호화 할 수 있다. 따라서 개인키는 나만 갖고 있기 때문에 나만 데이터 정보를 볼 수 있다.
   - 개인키를 암호화하면 공개키로만 복호화 할 수 있다. 공개키는 모두에게 공개되어 있어 내가 인증한 정보임을 알려 신뢰성을 보장 받을 수 있다.
