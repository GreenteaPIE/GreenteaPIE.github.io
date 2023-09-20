---
layout: post
title: SQL 용어 정리 - 4
date: 2023-09-20
excerpt: "SQL 용어 정리 - 프로시저란"
tags: [SQL, study]
comments: true


---

#### 데이터베이스 객체 정의

#### 프로시저(Procedure)란

데이터베이스에 대한 일련의 작업을 정리한 절차를 관계형 데이터베이스 관리 시스템에 저장한 것으로 영구저장모를(Persistent Storage Module)이라고도 불린다.

보통 저장 프로시저를 프로시저라고 부르며, 일련의 쿼리를 마치 하나의 함수처럼 실행하기 위한 쿼리의 집합이다.

즉. 특정 작업을 위한 쿼리들의 블록이다.

##### 장점

하나의 요청으로 여러 SQL문들을 실행시킬 수 있다.(네트워크 부하를 줄일 수 있다.)

네트워크 소요 시간을 줄여 성능을 개선할 수 있다.

여러 어플리케이션과 공유가 가능하다.(API처럼 제공가능)

기능 변경이 편하다.(특정 기능을 변경할 때 프로시저만 변경하면 됨)

##### 단점

문자나 숫자열 연산에 사용하면 오히려 C, Java 보다 느린 성능을 보일 수 있다.

유지보수가 어렵다.(프로시저가 앱의 어디에 사용되는지 확인이 어렵다.)

##### 생성

```SQL
CREATE OR REPLACE PROCEDURE 프로시저 이름(파라미터1, 파라미터2...)

IS 
변수

BEGIN

쿼리문

END 프로시저 이름;
```

예)

```SQL
CREATE OR REPLACE PROCEDURE GET_TIER(in_name IN VARCHAR2,out_tier OUT VARCHAR2)

IS

BEGIN

SELECT TIER INTO out_tier FROM SUMMONER_TB WHERE NAME = in_name;

EXCEPTION
   --찾을 수 없을 때
   WHEN NO_DATE_FOUNT THEN
   
   out_tier:='NO_FOUND';
   
   END GET_TIER;
```

파라미터 값은 in, out, inout 으로 총 세가지 종류로 작성할 수 있다.

먼저 in은 전달될 데이터 이고, out은 결과로 나갈 데이터. in out은 in과 out 모두 가능한 데이터를 뜻한다.

##### 조회

```SQL
DECLARE
출력될 변수 선언
실행할 프로시저
출력문(Optional)
END
```

##### 수정

수정은 create or replace 구문을 사용하면 해당 프로시저명이 있다면 수정, 없다면 생성되게 됩니다.

```SQL
CREATE OR REPLACE PROCEDURE GET_TIER(in_name IN VARCHAR2, out_tier OUT VARCHAR2)
IS
BEGIN
SELECT NAME INTO out_tier FROM SUMMONER_TB WHERE NAME = in_name;
EXCEPTION
WHEN NO_DATE_FOUND THEN
out_tier:='NO_DATA_FOUND'
END get_tier;
```

##### 삭제

```SQL
DROP PROCEDURE 프로시저명;
```

