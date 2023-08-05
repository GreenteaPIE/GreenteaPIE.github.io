---
layout: post
title: SQL 용어 정리 - 2
date: 2023-08-04
excerpt: SQL 용어 정리 - JOIN문, JOIN 종류"
tags: [SQL, study]
comments: true


---

#### JOIN 이란?

두 개 이상의 테이블을 서로 연결하여 데이터를 검색할 때 사용하는 방법

두 개의 테이블을 마치 하나의 테이블인 것처럼 보여준다.

#### 기본 구조

```sql
1) 일반
SELECT 테이블.컬럼, 테이블.컬럼
FROM 테이블1, 테이블2
WHERE 조건
```

#### JOIN 의 종류

- Inner Join
- Natural Join
- Outer Join
- Left Join
- Right Join
- Cross Join

##### 내부 조인 (Inner Join)

```sql
1)
SELECT 조회할 컬럼
FROM 테이블1, 테이블2
[WHERE 조건문]

2)
SELECT 조회할 컬럼
FROM 테이블1
(INNER) JOIN 테이블2
ON 테이블1.컬럼 = 테이블2.컬럼
[WHERE 추가조건]
```

##### 자연 조인 (Natural Join)

```SQL
SELECT 조회할 컬럼
FROM 테이블1
NATURAL JOIN 테이블2
[WHERE 조건문]
```

내부 조인에 속함.

두 테이블에서 동일한 컬럼명을 갖는 컬럼은 모두 조인이 된다.<br>두 테이블이 동시에 가지고 있는 컬럼의 값이 전부 같은 것만 골라냄 -> 반드시 두 테이블 간의 동일한 이름, 타입을 가진 컬럼이 필요함. 이름이 같지만 타입이  다른 컬럼이 있으면 에러<br>기준 테이블과 조인 테이블 모두에 데이터가 존재해야 조회됨<br>Inner Join 에서 조건문 추가하여 같은 결과값을 얻을 수 있다.

##### 전체 외부 조인(Full Outer Join)

```SQL
SELECT 조회할 컬럼
FROM 테이블1
FULL OUTER JOIN 테이블2
ON 조건문
[WHERE 추가조건문]
```

공통된 부분만 골라 결합하는 Inner Join 과 다르게 공통되지 않은 행도 유지한다.<br>이때 두 테이블 모두의 값을 유지하면 Full Outer Join<br>왼쪽 테이블 값만 유지하면 Left Outer Join<br>오른쪽 테이블 값만 유지하면 Right Outer Join<br>MYSQL에서는 FULL OUTER JOIN을 지원하지 않으므로 LEFT OUTER JOIN 결과와 RIGHT OUTER JOIN 결과를 UNION 하여 사용해야 한다.

##### Left Outer Join

```sql
SELECT 조회할 컬럼
FROM 기준테이블1
LEFT OUTER JOIN 테이블2
ON 조건문
[WHERE 추가조건문]
```

Left Join = Left Outer Join<br>왼쪽 테이블을 기준으로 일치하는 행만 결합되고, 일치하지 않는 부분은 null 값으로 채워진다.

##### Right Join

```sql
SELECT 조회할 컬럼
FROM 테이블1
RIGHT OUTER JOIN 기준테이블2
ON 조건문
[WHERE 추가조건문]
```

Right Join = Right Outer Join<br>오른쪽 테이블을 기준으로 일치하는 행만 결합되고, 일치하지 않는 부분은 null 값으로 채워진다.

##### Cross Join

```sql
1)
SELECT 조회할컬럼
FROM 테이블1, 테이블2

2)
SELECT 조회할컬럼
FROM 테이블1
JOIN 테이블2

3)
SELECT 조회할컬럼
FROM 테이블1
CROSS JOIN 테이블2
```

곱집합<br>두 테이블 데이터의 모든 조합<br>테이블1의 row * 테이블2의 row 개수만큼의 row를 가진 테이블 생성
