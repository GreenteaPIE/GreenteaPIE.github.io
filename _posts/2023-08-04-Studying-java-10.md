---
layout: post
title: JAVA 용어 정리 - 10
date: 2023-08-04
excerpt: "자바 용어 정리 - JPA와 MyBatis"
tags: [java, study]
comments: true


---

#### JPA, MyBatis 의 등장 배경

기존에 JDBC를 사용했을 때는 sql문이 코드에 섞여 있었고 만들어 요청하는 과정에서 sql문 생성시 String을 붙이고 자르는등의 작업이 필요해서 sql문이 조금만 길어져도 번거롭고 관리도 힘들었다. 따라서 코드와 sql문을 분리해서 관리하기 위해서 JPA, MyBatis 등을 사용한다.

#### MyBatis란?

1. SQL 실행 결과를 자바 빈즈 또는 Map 객체에 매핑해주는 Persistence 솔루션으로 관리한다. SQL을 소스 코드가 아닌 XML로 분리한다.
2. SQL 문과 프로그래밍 코드를 분리해서 구현한다.
3. 데이터소스 기능과 트랜잭션 처리 기능을 제공한다.

#### MyBatis의 장점, 단점

##### 장점

1. 접근이 쉽고 코드가 간결하다. (배우기가 쉽다.)
2. SQL문과 프로그래밍 코드가 분리되어 있어서 SQL문에 변경이 있을 때 마다 자바 코드를 수정하거나 컴파일 하지 않아도 된다.
3. 다양한 프로그래밍 언어로 구현이 가능하다.(이식성이 뛰어나다.)

##### 단점

1. 스키마 변경시 SQL 쿼리를 직접 수정해주어야 한다.
2. 반복된 쿼리가 발생하여 반복 작업이 있다.
3. 쿼리를 직접 작성하기 때문에 데이터베이스에 종속된 쿼리문이 발생할 수 있다.
4. 데이터베이스 변경시 로직도 함께 수정해주어야 한다.

#### JPA 란? (Java Presistence API)

1. Java ORM 기술에 대한 API 표준 명세이다. 구현된 클래스와 매핑을 해주기 위해 사용되는 프레임워크이다.

   ORM (Object-Relational Mapping) 

   Class와 RDB(Relational DataBase)의 테이블을 매핑한다는 뜻.

   객체를 RDB 테이블에 자동으로 영속화 해주는 것

2. 구현체로는 Hibernate, EclipseLink, Data Nucleus가 있으며 Hibernate가 가장 대중적이다.

#### JPA의 장점, 단점

##### 장점

1. 쿼리를 하나하나 작성할 필요가 없어 코드량이 줄어든다.
2. 가독성이 좋다.
3. 간편하게 수정이 가능하다. (유지보수, 리팩토링 용이)
4. 동일한 쿼리에 대한 캐시 기능을 사용하기 때문에 더욱 높은 성능을 낼 수 있다.

##### 단점

1. 매핑 설계를 잘못했을 때 성능 저하가 발생할 수 있다.
2. JPA를 제대로 사용하려면 알아야할 것이 많아서 학습하는데 시간이 오래 걸린다.
3. 다수의 테이블 조인시 신경써야 할게 많다.

#### JPA vs MyBatis

결론적으로, JPA는 객체 중심의 개발 방식을 지원하며, 개발자가 직접 SQL을 작성하지 않고도 객체를 관리할 수 있다는 장점이 있고, MyBatis는 SQL을 직접 작성할 수 있어 개발자가 더욱 자유롭게 데이터베이스를 다룰 수 있다는 장점이 있다. 따라서, 프로젝트의 목적과 상황에 따라 적절한 ORM 기술을 선택해야 한다.
