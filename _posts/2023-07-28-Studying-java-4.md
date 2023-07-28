---
layout: post
title: JAVA 용어 정리 - 4
date: 2023-07-28
excerpt: "자바 용어 정리 - EL문과 JSTL"
tags: [java, study]
comments: true


---

### EL문과 JSTL문을 쓰는 이유

- 페이지의 가독성을 높이기 위해 사용한다.
- Mybatis와 유사하게 라이브러리로 자바 구문을 만들어 놓고 필요할 때마다 꺼내쓰면 되기 때문에 유지보수에 편리
- JSP 페이지 내에서 자바코드와 HTML코드(태그)가 섞여 있으면 가독성이 떨어진다.
- EL문과 JSTL문을 이용하면 HTML과 태그로만 구성된 일관적인 소스코드로 볼 수 있다는 장점이 있다.

#### EL(Expression Language)

- 값을 간편하고 간결하게 출력할 수 있도록 해주는 표현 언어이다.
- 자바 <%=name%> / EL ${ }
- 값을 찾을 때 작은 Scope에서 큰 Scope 순으로 찾는다. <u>Scope이란? 변수를 사용할 수 있는 범위</u>
- pageContext > request > session > application
- EL ${ } 의 중괄호 사이에 들어가는 것은 변수명이 아니라 해당 객체에 attribute로 세팅해줄 때의 key 값을 써준다.

#### EL문 연산자

```jsp
/ : div
% : mod
&& : and
|| : or
! : not
> : gt - ${10>3} / ${10 gt 3} 둘다 true
< : lt
>= : ge
<= : le
== : eq
!= : ne
empty : 값이 비어있다면 true, 아니면 false - ${empty  data}
```

#### EL문 사용법

- ${requestScope.키값}
- ${paraml.네임값}
- ${paramValues.네임값[인덱스]}
- ${cookie.키값.value}

### JSTL(JSP Standard Tab Library)란?

- 연산이나 조건문, 반복문을 편하게 처리할 수 있으며 JSP페이지 내에서 자바코드를 사용하지 않고도 로직을 구현 할 수 있도록 해준다.
- 자바에서는 for(int i=0; ...){ } 이렇게 쓰지만 JSTL 에서는`<c:forEach></c:forEach>` 

#### JSTL core 태그

```jsp
<c:set></c:set>
//변수 만들 때 사용

<c:out></c:out>
//값을 출력할 때 사용

<c:if></c:if>
//조건 제어(if문)

<c:choose></c:choose>
//조건 제어(switch문)
//내부에는 c:when과 c:otherwise만 있어야한다.

<c:when></c:when>
//조건 제어(case문)

<c:otherwise></c:otherwise>
//조건 제어(default)

<c:forEach></c:forEach>
//반복 제어(for)
```

