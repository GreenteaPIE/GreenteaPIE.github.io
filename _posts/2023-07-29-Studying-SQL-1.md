---
layout: post
title: SQL 용어 정리 - 1
date: 2023-07-29
excerpt: "SQL 용어 정리 - UNION 과 UNION ALL의 차이, MINUS, 계층형 조회, 서브쿼리"
tags: [SQL, study]
comments: true


---

#### UNION 과 UNION ALL의 차이

UNION은 두 개의 테이블을 하나로 만드는 연산이다. 두 개 테이블의 컬럼 수, 컬럼 데이터 형식이 모두 일치해야 한다.

UNION 연산은 두 개의 테이블을 하나로 합치면서 중복된 데이터를 제거한다. 그래서 UNION은 정렬을 발생시킨다.

UNION ALL 은 중복을 제거하거나 정렬을 유발하지 않는다. ALL 이니까 중복제거 없이 전부 보여 준다고 생각하면 된다.

```sql
SELECT * FROM DEPT;
UNION
SELECT * FROM DEPT;
```

#### MINUS(EXCEPT)

MINUS는 두 개의 테이블의 차집합을 구하는 연산이다. 선행 SELECT문에는 있고, 후행 SELECT 문에는 없는 집합을 조회한다.

MYSQL에서는 EXCEPT로 쓴다.

```SQL
SELECT DEPTNO FROM DEPT
MINUS
SELECT DEPTNO FROM EMP;
```

#### 계층형 조회(Connect by)

Connect by문은 계층형 데이터가 있을 때, 데이터를 트리 구조 형태로 조회할 수 있는 구문이다. Oracle DBMS에서 지원한다. 만약 MSSQL에서 쓰고 싶다면 CONVERT와 subquery등을 조합하여 사용해야한다.

예제를 통해 이해하면 이해하기가 쉽다.

```sql
CREATE TABLE EMP(
  EMPNO NUMBER(10) primary key,
  ENAME VARCHAR2(20),
  DEPTNO NUMBER(10),
  MGR NUMBER(10),
  JOB VARCHAR2(20),
  SAL NUMBER(10)
);

INSERT INTO EMP VALUES(1000, 'TEST1', 20, NULL, 'CLERK', 800);
INSERT INTO EMP VALUES(1001, 'TEST2', 30, 1000, 'SALESMAN', 1600);
INSERT INTO EMP VALUES(1002, 'TEST3', 30, 1000, 'SALESMAN', 1250);
INSERT INTO EMP VALUES(1003, 'TEST4', 20, 1000, 'MANAGER', 3000);
INSERT INTO EMP VALUES(1004, 'TEST5', 30, 1000, 'SALESMAN', 1250);
INSERT INTO EMP VALUES(1005, 'TEST6', 30, 1001, 'MANAGER', 2850);
INSERT INTO EMP VALUES(1006, 'TEST7', 10, 1001, 'MANAGER', 2650);
INSERT INTO EMP VALUES(1007, 'TEST8', 20, 1006, 'ANALYST', 3000);
INSERT INTO EMP VALUES(1008, 'TEST9', 30, 1006, 'PRESIDENT', 5000);
INSERT INTO EMP VALUES(1009, 'TEST10', 30, 1002, 'SALESMAN', 1500);
INSERT INTO EMP VALUES(1010, 'TEST11', 20, 1002, 'CLERK', 1100);
INSERT INTO EMP VALUES(1011, 'TEST12', 30, 1001, 'CLERK', 950);
INSERT INTO EMP VALUES(1012, 'TEST13', 20, 1000, 'ANALYST', 3000);
INSERT INTO EMP VALUES(1013, 'TEST14', 10, 1000, 'CLERK', 1300);
```

다음으로 connect by 구문을 실행한다. START WITH은 시작 조건을, CONNECT BY PRIOR은 조인 조건을 의미한다.

```SQL
SELECT LEVEL, EMPNO, MGR, ENAME
FROM EMP
START WITH MGR IS NULL
CONNECT BY PRIOR EMPNO = MGR;
```

| LEVEL | EMPNO | MGR  | ENAME  |
| :---: | :---: | :--: | :----: |
|   1   | 1000  |      | test1  |
|   2   | 1001  | 1000 | test2  |
|   3   | 1005  | 1001 | test6  |
|   3   | 1006  | 1001 | test7  |
|   4   | 1007  | 1006 | test8  |
|   4   | 1008  | 1006 | test9  |
|   3   | 1011  | 1001 | test12 |
|   2   | 1002  | 1000 | test3  |
|   3   | 1009  | 1002 | test10 |
|   3   | 1010  | 1002 | test11 |

LEVEL 이라는 컬럼을 제공한다. 인스턴스의 계층값을 표시 해주는 것이다. MGR 값이 null인 부분부터 ROOT가 된다. 

EMPNO = 1000 인 항목이 ROOT 이자 LEVEL 1 값이 되었다. 이후 EMPNO = MGR 이라는 규칙에 따라서 MGR = 1000번인 EMPNO = {1001, 1002}인 두 항목이 자식으로 LEVEL 2 로 표시된다. 다음으로 MGR이 1001 또는 1002인(부모가 1001 또는 1002인){1005, 1006, 1011, 1009, 1010} 은 LEVEL 3으로 지정되었다. 

이런 식으로 START WITH 구문을 통해 ROOT 를 지정하고, CONNECT BY PRIOR 구문으로 부모 - 자식간 계층을 지정하는 기준을 정하여 표기하는 것이다.

##### CONNECT BY의 구문들

- LEVEL : 검색 항목의 깊이를 의미, 가장 상위 레벨이 1이다.
- CONNECT_BY_ROOT : 가장 최상위 값을 표시한다.
- CONNECT_BY_ISLEAF : 가장 최하의 값을 표시한다.
- SYS_CONNECT_BY_PATH : 계층 구조의 전개 경로를 표시한다.
- NOCYCLE : 순환 구조 발생 지점까지만 표시된다.
- CONNECT_BY_ISCYCLE : 순환 구조 발생 지점을 표시한다.

#### 서브 쿼리(Sub query)

서브쿼리는 쿼리문 안에 또 다른 쿼리를 집어넣어 조회하는 것으로 Main query, Sub query로 구분하기도 한다. 종류는 다음과 같다.

##### 서브 쿼리 종류

- Inline view : FROM 구에 SELECT문 삽입
- scala subquery : SELECT 구에 SELECT 문 삽입
- subquery : WHERE 구에 SELECT 문 삽입

##### 단일 행, 다중 행 서브 쿼리

조회 결과가 단일 행 일 때와 다중 행 일 때 사용 해야 하는 연산자가 다르다.

##### 단일 행 서브 쿼리

서브 쿼리 실행 시 결과가 반드시 한 행만 조회 되는 것. scala subquery 는 단일 행 서브 쿼리여야만 한다. 비교 연산자 =, <, <=, >, >=, <>를 사용한다. (<> : 같지 않음)

##### 다중 행 서브 쿼리

서브 쿼리 결과로 여러 행을 반환하는 것. 다중행 연산자 IN, ALL, ANY, EXISTS를 사용한다.

- IN : 메인 쿼리의 비교 조건이 서브쿼리의 결과 중 하나만 동일하면 참이 된다.
- ALL : 메인 쿼리와 서브쿼리의 결과가 모두 동일하면 참이 된다.
- < ALL : 최소값 반환
- \> ALL : 최대값 반환
- ANY : 메인 쿼리의 비교 조건이 서브쿼리의 결과 중 하나 이상 동일하면 참이 된다.
- < ANY : 하나라도 크면 참
- \> ANY : 하나라도 작으면 참
- EXISTS : 메인 쿼리와 서브쿼리의 결과가 하나라도 존재하면 참이 된다.
