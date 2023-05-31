---
layout: post
title: Team Project Spring (DiamondBlack)
date: 2023-05-15
excerpt: "팀 프로젝트 DB Spring Version"
tags: [project, java, jsp, Oracle, css, HTML, BootStrap, API, JQuery, JavaScript, Spring, FrameWork]
feature: /img/SpringDB/logo.png
project: true
comments: true



---


> **사용한 플랫폼 : Spring, Oracle**

<details>
<summary><span style="font-size:25pt; font-weight:bold; color:gray;">>목차<</span></summary>
<div markdown="1">
- [**시작하며**](#시작하며)
  * [프로젝트 명세서](#프로젝트-명세서)
    + [1. 프로젝트 진행 순서](#1-프로젝트-진행-순서)
    + [2. 개요](#2-개요)
    + [3. 기능 별 요구 사항](#3-기능-별-요구-사항)
    + [4. DB 설계](#4-db-설계)
    + [5. API 설계](#5-api-설계)
    + [6.  화면 설계서](#6-화면-설계서)
      - [로그인을 하지 않았을 경우<br>](#로그인을-하지-않았을-경우)
      - [회원 가입 & 로그인<br>](#회원-가입--로그인)
      - [어드민<br>](#어드민)
      - [상품 검색<br>](#상품-검색)
      - [상품 구매<br>](#상품-구매)
      - [게시판 이용<br>](#게시판-이용)
      - [마이 페이지<br>](#마이-페이지)
    + [7. 개발 내용](#7-개발-내용)
    + [8. 개선 사항과 느낀 점](#8-개선-사항과-느낀-점)
  * [프로젝트 주소](#프로젝트-주소)
</div>
</details>

# **시작하며**

SpringBoot 로 넘어가기전 Spring FrameWork를 배웠지만 본격적인 다음 프로젝트는 <br>Boot로 진행 한다고 하여, 개인적으로 기존에 만든 프로젝트를 Spring 형식에 맞춰서 구현하고 싶었지만 <br>못 만들었던 기능들을 추가하고, 아쉬웠던 기능들의 Detail을 살려 만들어 보기로 했다.



## 프로젝트 명세서

1. 프로젝트 진행 순서
2. 개요
3. 기능 별 요구 사항
4. DB 설계
5. API 설계
6. 화면 설계서
7. 개발 내용
8. 개선 사항 과 느낀 점

### 1. 프로젝트 진행 순서

기존에 있던 DB에서 구현할 기능들에 필요한 컬럼들을 정리 후 추가하고 스프링에 <br>설정할 dependency들 pom.xml에 넣고 기본적인 설정을 마친 후 설계한 API를 토대로 기존의 form들을 <br>가져와 수정하고 추가한다. 

기능들을 하나씩 구현할때마다 JUnit 테스트를 통해 모듈들이 정상적으로 동작하는지 확인한다.

### 2. 개요

- 프로젝트 명 : DiamondBlack Spring Version

- 인원 : 1명

- 기간 : 2023.05.11 ~ ing

- 기능 :

  - 유저 
    1. 회원 가입
    2. 로그인
    3. 자유, 질문 게시판 게시글 등록, 수정, 삭제
    4. 자유, 질문 게시판의 게시글에 댓글 작성, 수정, 삭제
    5. 마이페이지
       - 주문 내역
       - 나의 작성 글 확인
       - 내 정보 수정 및 탈퇴
       - 보유쿠폰 확인
    6. 상품 구매
       - 경매 및 세일상품 등 상품 구매
       - 장바구니 기능 및 결제
       - 쿠폰 적용
       - 상품 검색
  - 어드민
    1. 회원 관리
       - 회원 정보 수정
       - 회원 탈퇴
    2. 게시판 관리(자유, 질문, 공지사항 게시판)
       - 게시글 등록, 수정, 삭제
    3. 상품 관리
       - 상품 등록, 삭제
    4. 브랜드 관리
       - 브랜드 추가, 삭제
    5. 옥션
       - 옥션 상품 관리
       - 시간 설정 및 시작가격 설정 등
  - 게시판
    1. 자유 게시판
       - CRUD기능, 조회수, 페이징 처리, 댓글, 멀티 이미지 첨부, 게시글 검색
    2. QnA 게시판
       - CRUD기능, 조회수, 페이징 처리, 멀티 이미지 첨부, 게시글 검색
  - 상품
    1. 상품 등록
       - 가격, 이미지, 상품 설명, 사이즈, 브랜드
    2. 장바구니 담기
  - 장바구니
    1. 장바구니 담긴 상품 수량 조정, 삭제
    2. 총 상품 가격 확인
  - 결제
    1. 구매자 정보 확인
    2. 배송받을 배송지 변경
    3. 총 결제 금액 확인
    4. 결제 API를 이용하여 결제
  - Event
    1. 등급에 따른 쿠폰 수령
    2. 반복수령 불가

  - Contact
    1. 지도 API를 이용하여 회사 위치 확인
    2. 회사 정보 확인
  - 옥션(경매)
    1. 어드민이 설정한 상품, 시간, 시작 가 확인
    2. 입찰이 끝나면 낙찰자에게 구매 권한 부여
  - 세일
    1. 어드민이 상품등록 시 할인율을 설정 후 등록

  

- 개발 언어 : Java 11, HTML, JavaScript, JSP

- 개발 환경 : Spring, Apache Tomcat 9.0

- 데이터베이스 : Oracle

- 간단 소개 : 해외 명품 직구 샵

### 3. 기능 별 요구 사항

- 회원 가입
  - 유효성 검사
    - 이름, 아이디, 이메일, 주소, 비밀번호 미입력시 "xxx를 확인해주세요." 메시지 출력 
    - 아이디 중복 검사
      - 아이디 중복 시 "아이디가 이미 존재합니다." 메시지 출력
    - 이메일 형식은 Emailid 뒤에 @ 과 ###.### 의 형식으로 입력
    - 이메일인증
      - 인증번호 전송 후 일치 여부 검사
    - 우편번호와 주소는 주소API를 이용하여 자신의 집 검색 후 자동 입력
    - 비밀번호는 sha256 알고리즘으로 암호화 되어 DataBase에 저장 
      - 비밀번호 입력칸과 비밀번호 확인칸의 일치 여부 검사
- 로그인
  - 로그인을 하지 않은 경우 아래 페이지만 이용 가능
    - 회원 가입 페이지
    - 로그인 페이지
    - 게시판의 게시글 목록 조회, 상세 보기 페이지
    - Q&N 페이지
    - 브랜드 상품(카테고리) 페이지 상품 리스트
    - 옥션,세일 상품 페이지 상품 리스트
    - Event 페이지 쿠폰 리스트
    - 로그인을 하지 않고 위에 페이지를 제외한 다른 페이지를 이용하려 할 시<br>"로그인 후 사용 가능 합니다." 메시지 출력 후 기능 이용 제한
  - 로그인 검사
    - 아이디와 비밀번호가 일치하지 않을 경우 "ID 또는 비밀번호를 잘못 입력 하셨습니다." 메시지 출력
    - 아이디가 존재하지 않을 경우 "ID 또는 비밀번호를 잘못 입력 하셨습니다." 메시지 출력
    - 아이디와 비밀번호가 일치할 시 메인 페이지로 이동
    - 어드민 계정으로 로그인할 시 관리자 기능을 사용할 수 있는 버튼 생성
- 유저
  - 상품 장바구니 담기
    - 장바구니에 담긴 상품의 수만큼 뱃지에 숫자 표기
  - 장바구니 상품 구매
    - 주문자 정보와 동일 버튼 체크 시 배송지 입력에 유저정보 자동입력
    - 구매 시 이용 약관에 동의 하지 않았거나, 결제 방식을 선택 하지 않았으면<br>각각의 선택 메시지 출력
    - 위의 이용 약관과 결제 방식을 선택 후 구매 버튼을 눌렀다면, 결제API 모듈 실행
  - 옥션 상품 입찰, 구매
    - 낙찰된 최종 입찰자만 구매 가능
  - 세일 상품 구매
    - 어드민이 등록한 상품 할인율 만큼 할인된 가격으로 구매 가능
  - 자유 게시판 이용
    - 등록, 수정, 삭제 가능(수정과 삭제는 작성한 본인만 가능)
    - 댓글 작성, 수정, 삭제(수정과 삭제는 작성한 본인만 가능 )
  - QnA 게시판 이용
    - 등록, 수정, 삭제는 어드민만 가능
  - Event 페이지 이용
    - 상품 구매 시 지급된 누적point에 따른 등급 별로 쿠폰수령 가능
    - 등급 조건이 맞지 않을 시 "등급이 낮아 수령 할 수 없습니다." 메시지 출력
    - 이미 수령한 쿠폰의 수령을 시도할 시 "이미 지급된 쿠폰입니다." 메시지 출력 
- 마이 페이지
  - 내 정보 수정
    - 비밀번호 입력 후 수정 페이지로 이동
    - 아이디와 이름을 제외한 정보들을 수정 가능
    - 각 입력란이 비어있을 시  <br>"xxx를 입력해주세요." 메시지 출력
    - 비밀번호를 재차 확인하여 비밀번호 일치 여부 확인
    - 이메일 인증으로 이메일 확인
    - 탈퇴 진행 여부를 재차 확인 후 회원 탈퇴 진행
  - 주문 내역
    - 자신이 구매한 주문 번호, 주문 날짜, 상태 목록 출력
    - 주문 번호 클릭 시 구매한 해당 주문 번호의 상품과 가격 등의 상세 내역을 확인
  - 내가 쓴 글
    - 자유 게시판에 작성한 나의 글 확인 및 수정, 삭제 가능
    - 내가 단 댓글 확인 및 수정, 삭제 가능
  - 보유 쿠폰
    - 수령한 쿠폰의 리스트를 확인
- 어드민
  - 상품관리
    1. 브랜드 관리
       - 브랜드 로고 이미지와 브랜드 명을 추가, 삭제 가능
       - 브랜드 추가 시 categories 와 main page에 해당 브랜드 자동 추가
    2. 상품 관리
       - 브랜드, 상품 카테고리, 상품 이름, 사이즈, 가격, 성별, 이미지,<br>상품 설명, 재고량, 할인율 등을 설정하여 추가, 삭제 가능
  - 회원 관리
    1. 회원 정보 수정
       - 가입 일자, 비밀번호를 제외한 모든 정보 수정 가능

    2. 회원 삭제

  - 게시판 관리
    1. 자유 게시판 관리
       - 자유 게시판 등록, 수정, 삭제 가능
       - 댓글 등록, 수정, 삭제 가능
       - 어드민이 작성한 게시글은 상단에 위치

    2. QnA 게시판 관리
       - QnA 게시판 등록, 수정, 삭제 가능

  - 옥션
    1. 옥션 상품 등록
       - 등록할 상품을 선택 후 시작가, 제한시간 설정

  - 세일
    1. 상품 할인률을 설정하여 등록하면  Sale페이지에 노출

  - 매출 관리
    1. 판매한 총 매출 가격과 구매자, 주문 번호 출력
- 상품
  - 브랜드 로고 클릭 시 해당 브랜드의 모든 상품 리스트 출력
  - 카테고리 클릭 시 해당 브랜드의 카테고리가 설정된 상품 리스트 출력
  - 상품 검색 시 해당 검색어가 들어가는 상품 리스트 출력
  - 검색한 상품이 존재 하지 않을 시 "상품 "xx"의 검색 결과 없음" 메시지 출력
- Contact
  - Kakao지도API를 활용하여 설정한 임의의 회사 위치를 지도로 확인 가능
  - 회사의 상세 정보를 확인 가능
- 디자인
  - Bootstrap을 이용하여 모바일과 pc에 따른 반응형 웹으로 제작



### 4. DB 설계

![_config.yml]({{ site.baseurl }}/img/SpringDB/DBdiagram.png)

#### USER

|  컬럼명  | 데이터 타입 |      조건       |   설명   |
| :------: | :---------: | :-------------: | :------: |
|  USERID  |  VARCHAR2   |       PK        |  아이디  |
|   PASS   |  VARCHAR2   |    NOT NULL     | 비밀번호 |
|   NAME   |  VARCHAR2   |    NOT NULL     |   이름   |
|  EMAIL   |  VARCHAR2   |    NOT NULL     |  이메일  |
| ADDRESS1 |  VARCHAR2   |    NOT NULL     | 우편번호 |
| ADDRESS2 |  VARCHAR2   |    NOT NULL     |   주소   |
| ADDRESS3 |  VARCHAR2   |    NOT NULL     | 상세주소 |
|  PHONE   |  VARCHAR2   |                 | 전화번호 |
|  GENDER  |   NUMBER    |                 |   성별   |
|  GRADE   |   NUMBER    |    DEFAULT 0    |   등급   |
|  POINT   |   NUMBER    |    DEFAULT 0    |  포인트  |
|  ENTER   |    DATE     | DEFUALT STSDATE | 가입일자 |

```sql
/* 누적 포인트 등급 상승 admin grade = 1 */
CREATE OR REPLACE TRIGGER update_user_grade
BEFORE UPDATE OF point ON shopuser
FOR EACH ROW
DECLARE
  new_grade NUMBER(10);
BEGIN
  IF :new.point >= 500000 THEN
    new_grade := 4; 
  ELSIF :new.point >= 100000 THEN
    new_grade := 3;
  ELSIF :new.point >= 30000 THEN
    new_grade := 2;
  ELSE
    new_grade := 0;
  END IF;
  
  :new.grade := new_grade;
END;
/
```

상품 구매에 따라 총 구매 금액의 0.1% 만큼 포인트가 지급되고, 누적된 포인트에 다른 등급을 설정하는 SQL문을 추가한다.

#### BOARD

|  컬럼명   | 데이터 타입 |      조건       |      설명      |
| :-------: | :---------: | :-------------: | :------------: |
|    NUM    |   NUMBER    |       PK        |  게시글 번호   |
|  USERID   |  VARCHAR2   |       FK        | 아이디(작성자) |
|   TITLE   |  VARCHAR2   |    NOT NULL     |      제목      |
|  CONTENT  |  VARCHAR2   |    NOT NULL     |      내용      |
| CATEGORY  |  VARCHAR2   |    NOT NULL     |     말머리     |
| WRITEDATE |    DATE     | DEFAULT SYSDATE |    작성일자    |
| READCOUNT |   NUMBER    |    DEFAULT 0    |     조회수     |

#### BOARD REPLY

|   컬럼명    | 데이터 타입 |         조건         |    설명     |
| :---------: | :---------: | :------------------: | :---------: |
|     RNO     |   NUMBER    |          PK          |  댓글 번호  |
|     NUM     |   NUMBER    |          FK          | 게시글 번호 |
|   WRITER    |  VARCHAR2   |       NOT NULL       |   작성자    |
| CONTENTVARH |  VARCHAR2   |       NOT NULL       |    내용     |
|   REGDATE   |  TIMESTAMP  | DEFAULT SYSTIMESTAMP |  작성일자   |

#### COUPON

|    컬럼명     | 데이터 타입 |   조건    |    설명     |
| :-----------: | :---------: | :-------: | :---------: |
|    USERID     |  VARCHAR2   |    FK     |   아이디    |
|  COUPONNAME   |  VARCHAR2   | NOT NULL  |  쿠폰 이름  |
|     CNUM      |   NUMBER    |    PK     |  쿠폰 번호  |
| DISCOUNTPRICE |   NUMBER    | NOT NULL  |  할인 가격  |
| COUPONRESULT  |   NUMBER    | DEFAULT 1 |  사용 유무  |
|    IMGURL     |   VARCHAR   | NOT NULL  | 쿠폰 이미지 |

#### PRODUCT

|    컬럼명    | 데이터 타입 |   조건    |     설명      |
| :----------: | :---------: | :-------: | :-----------: |
|     NUM      |   NUMBER    |    PK     |   상품 번호   |
|   PGENDER    |   NUMBER    | NOT NULL  |   상품 성별   |
|    BNAME     |  VARCHAR2   | NOT NULL  |   브랜드명    |
|     KIND     |   NUMBER    | NOT NULL  | 상품 카테고리 |
|    PNAME     |  VARCHAR2   | NOT NUILL |    상품 명    |
|    IMGURL    |  VARCHAR2   | NOT NULL  |  상품 이미지  |
|    PSIZE     |  VARCHAR2   | NOT NULL  |  상품 사이즈  |
| DISCOUNTRATE |   NUMBER    | DEFAULT 0 |    할인율     |
|   BALANCE    |   NUMBER    | DEFAULT 0 |    재고량     |
|    PRICE     |   NUMBER    | NOT NULL  |   상품 가격   |
|   EXPLAIN    |  VARCHAR2   | NOT NULL  |   상품 설명   |

PRODUCT TABLE에서 사용하지 않는 컬럼 [PURCHASEDNUM(구매번호), WRITEDATE(상품등록일), READCOUNT(조회수)] 는 제거한다.

#### BRAND

| 컬럼명 | 데이터 타입 |   조건   |     설명      |
| :----: | :---------: | :------: | :-----------: |
| BNAME  |  VARCHAR2   |    PK    |   브랜드명    |
| IMGURL |  VARCHAR2   | NOT NULL | 브랜드 이미지 |

#### CART

|  컬럼명   | 데이터 타입 |      조건       |        설명        |
| :-------: | :---------: | :-------------: | :----------------: |
|  CARTNUM  |   NUMBER    |       PK        |   장바구니 번호    |
|  USERID   |  VARCHAR2   |       FK        |       아이디       |
|    NUM    |   NUMBER    |       FK        |     상품 번호      |
|   PSIZE   |  VARCHAR2   |    NOT NULL     | 선택한 상품 사이즈 |
| QUANTITY  |   NUMBER    |    NOT NULL     |  선택한 상품 수량  |
|   PRICE   |   NUMBER    |    NOT NULL     |  선택한 상품 가격  |
| ORDERDATE |    DATE     | DEFAULT SYSDATE | 장바구니 담은 일자 |
|  RESULT   |    CHAR     |    DEFAULT 1    |     주문 유무      |

#### ORDERS

|   컬럼명    | 데이터 타입 |      조건       |      설명      |
| :---------: | :---------: | :-------------: | :------------: |
| ORDERNUMBER |   NUMBER    |       PK        |   주문 번호    |
|   USERID    |  VARCHAR2   |       FK        | 아이디(주문자) |
|   INDATE    |    DATE     | DEFAULT SYSDATE |    주문일자    |

#### ORDER_DETAIL

|      컬럼명       | 데이터 타입 |   조건    |      설명      |
| :---------------: | :---------: | :-------: | :------------: |
| ORDERDETAILNUMBER |   NUMBER    |    PK     | 주문상세 번호  |
|    ORDERNUMBER    |   NUMBER    |    FK     |   주문 번호    |
|        NUM        |   NUMBER    |    FK     |   상품 번호    |
|     QUANTITY      |   NUMBER    | NOT NULL  |   상품 수량    |
|       PRICE       |   NUMBER    | DEFAULT 0 |      가격      |
|    TOTALPRICE     |   NUMBER    | NOT NULL  |    총 가격     |
|       PSZIE       |  VARCHAR2   | NOT NULL  |  상품 사이즈   |
|       NAME        |  VARCHAR2   | NOT NULL  | 배송받는 사람  |
|       EMAIL       |  VARCHAR2   | NOT NULL  |     이메일     |
|     ADDRESS1      |  VARCHAR2   | NOT NULL  |    우편번호    |
|     ADDRESS2      |  VARCHAR2   | NOT NULL  |      주소      |
|     ADDRESS3      |  VARCHAR2   | NOT NULL  |    상세주소    |
|       PHONE       |  VARCHAR2   | NOT NULL  |    전화번호    |
|      RESULT       |    CHAR     | DEFAULT 1 | 주문 진행 과정 |

```sql
/* join table(order, order_detail) */
CREATE VIEW order_view AS
SELECT
  orders.orderNumber,
  orders.userid,
  orders.indate,
  order_detail.orderDetailNumber,
  order_detail.num,
  order_detail.quantity,
  order_detail.totalprice,
  order_detail.price,
  order_detail.psize,
  order_detail.name,
  order_detail.phone,
  order_detail.address1,
  order_detail.address2,
  order_detail.address3,
  order_detail.result
FROM
  orders
JOIN
  order_detail
ON
  orders.orderNumber = order_detail.orderNumber;

```

ORDERS TABLE 과 ORDER_DETAIL TABLE 을 JOIN 하여 두 TABLE을 엮어 원하는 값을 불러올수있게 VIEW를 생성하는 SQL문을 추가한다.

#### AUCTION

|   컬렴명   | 데이터 타입 |   조건   |         설          |
| :--------: | :---------: | :------: | :-----------------: |
|    NUM     |   NUMBER    |    PK    |      상품 번호      |
|   USERID   |  VARCHAR2   |    FK    |   아이디(낙찰자)    |
|   BNAME    |  VARCHAR2   | NOT NULL |      브랜드 명      |
|   PNAME    |  VARCHAR2   | NOT NULL |       상품 명       |
|   PRICE    |   NUMBER    | NOT NULL |      상품 가격      |
| STARTPRICE |   NUMBER    | NOT NULL |       시작가        |
|   IMGURL   |  VARCHAR2   | NOT NULL |     상품 이미지     |
|  ENDPRICE  |   NUMBER    | NOT NULL |       입찰가        |
|  ENDTIME   |    DATE     | NOT NULL |      종료일자       |
|   ONOFF    |   NUMBER    | NOT NULL | 옥션 시작/종료 여부 |

### 5. API 설계

#### 1. 유저 관련 API

|   Description   |      Return Page       |          url          |                           Request                            | Response |
| :-------------: | :--------------------: | :-------------------: | :----------------------------------------------------------: | :------: |
| 회원가입 페이지 | 회원가입 페이지로 이동 |    GET /user/join     |                              -                               |    -     |
|    회원가입     |  회원가입 환영 페이지  |    POST /user/join    | String userid<br />String pass<br />String name<br />String email<br />String address1<br />String address2<br />String address3<br />String phone<br />Int gender<br />Int point<br />Int grade<br />SYSDATE enter |    -     |
| 아이디 중복검사 |    회원가입 페이지     | POST /user/userIDChk  |                        String userid                         |  user[]  |
|   이메일 인증   |    회원가입 페이지     |  GET /user/mailCheck  |                              -                               |    -     |
|  로그인 페이지  |  로그인 페이지로 이동  |    GET /user/login    |                              -                               |    -     |
|     로그인      |      메인 페이지       |   POST /user/login    |                String userid<br />String pass                |  user[]  |
|    로그아웃     |      메인 페이지       |   GET /user/logout    |                              -                               |    -     |
|  내 정보 수정   |    본인 인증 페이지    |  GET /user/mypagechk  |                              -                               |    -     |
|    본인 인증    |  내 정보 수정 페이지   | POST /user/mypagechk  |                String userid<br />String pass                |  user[]  |
|  내 정보 수정   |      메인 페이지       | POST /user/userupdate |                            user[]                            |    -     |
|    회원탈퇴     |      메인 페이지       |  POST /user/userexit  |                        String userid                         |    -     |

### 6. 화면 설계서

#### 로그인을 하지 않았을 경우<br>

<details>
<summary>>펼치기<</summary>
<div markdown="1">
<iframe width="560" height="315" src="//www.youtube.com/embed/h38ZMZWg_ew" frameborder="0"> </iframe>
</div>
</details>


로그인을 하지 않았을 경우엔 게시판 등록, 상품 구매, 장바구니, 쿠폰수령등의 기능들을 이용할 수 없다.

#### 회원 가입 <br>

<details>
<summary>>펼치기<</summary>
<div markdown="1">
<iframe width="560" height="315" src="//www.youtube.com/embed/v-okLiZIZWw" frameborder="0"> </iframe>
</div>
</details>


회원 가입은 유효성 검사를 거쳐 진행된다.

회원 가입시 입력한 패스워드는 아래처럼 암호화 되어 저장된다.

![_config.yml]({{ site.baseurl }}/img/DiamondBlack/sha256.png)

#### 로그인 & 로그아웃<br>

<details>
<summary>>펼치기<</summary>
<div markdown="1">
<iframe width="560" height="315" src="//www.youtube.com/embed/DU73zKQwWbg" frameborder="0"> </iframe>
</div>
</details>



#### 내 정보 수정 & 탈퇴<br>

<details>
<summary>>펼치기<</summary>
<div markdown="1">
<iframe width="560" height="315" src="//www.youtube.com/embed/pD0cKIQ_gNk" frameborder="0"> </iframe>
</div>
</details>


PW을 한번 더 확인하여 수정 페이지로 넘어가고 회원가입과 같은 유효성 검사를 진행하여 정보 수정을 완료한다.

내 정보 수정 페이지 에서 confirm을 이용해 탈퇴 진행 여부를 한번 더 확인 후 탈퇴를 한다.

#### 어드민<br>



회원 관리, 게시판 관리, 상품 관리, 및 옥션(경매) 상품을 등록 할 수 있다.

#### 상품 검색<br>



검색한 단어를 포함한 상품을 출력하며, 검색한 단어를 포함한 상품이 존재하지 않을 시<br>
검색 결과 없음 페이지를 보여준다.

#### 상품 구매<br>



옥션(경매)에 낙찰되면 낙찰자는 구매버튼을 이용하여 상품을 구매 할 수 있고, 핫딜<br>
을 통해 할인된 상품을 구매, 상품 페이지에서 장바구니에 담은 상품을 일괄적으로 구매할 수 있다.<br> 결제는 실제로 이루어지며, 결제testAPI이기때문에 자정이되면 payback된다.<br>
상품의 금액이 높기 때문에 테스트를 위해 결제를 취소해도 결제 완료 페이지로 넘어가게 구현했다.

#### 게시판 이용<br>

​              

자유 게시판 모든유저가 열람 가능하지만 등록은 회원가입한 유저만 이용 할 수 있고,<br> 본인이 작성한 글만 수정 및 삭제를 할 수 있다.<br>QnA 게시판은 모든 유저가 열람 가능하지만 등록, 수정, 삭제는 어드민만 가능 하다.

#### 마이 페이지<br>



본인이 구매한 상품들의 주문내역을 확인 할 수 있고, 내가 작성한 글의 목록을 볼 수 있다.<br>
내 정보에 들어가면 내 정보를 수정 할 수 있고, 탈퇴가 가능하다.

### 7. 개발 내용

[Spring 초기 설정](https://greenteapie.github.io/DBSpringVer-first-setting/)<br>[Main 페이지 추가](https://greenteapie.github.io/DBSpringVer-main-page/)<br>[회원가입 페이지 & 기능 추가 ](https://greenteapie.github.io/DBSpringVer-add-join/)<br>[로그인(로그아웃) 페이지 & 기능 추가 ](https://greenteapie.github.io/DBSpringVer-add-login/)<br>[내 정보 수정(탈퇴) 페이지 & 기능 추가](https://greenteapie.github.io/DBSpringVer-add-myinfo/)<br>[]

### 8. 개선 사항과 느낀 점 

1. 개선 사항

   1. 로그인 부분
      1. 카카오톡/ 구글 API를 이용하여 로그인 가능하게 구현
      1. 이메일 인증 기능 구현
   2. 유저
      1. 찜하기 기능
      2. 장바구니에 담은 상품 수를 장바구니 뱃지에 표현
      3. 결제 시 결제한 금액에 따른 포인트 지급 및 포인트에 따른 회원 등급 조정
      4. 회원 등급에 따른 상품 할인 쿠폰 지급
   3. 상품
      1. 상품 구매 할 시 재고량의 변동을 구현
      2. 할인 쿠폰 구현
      3. 취소 및 반품 구현
      4. 옥션의 낙찰자가 상품 구매 후 해당 상품의 구매하기 버튼을 사라지게 구현
      5. 상품 페이지의 페이징 처리
      6. 핫딜 기능을 코드로 직접 고쳐서 반영 시키는게 아닌 어드민 페이지에서<br>조정 할 수 있게 구현
   4. 게시판의 답글 달기 및 API를 이용한 댓글이 아닌 웹페이지 자체의 댓글 기능 구현

2. 느낀 점
   MCV1 패턴과 MVC2 패턴을 이용하여 CRUD 웹 페이지를 구현 해봤는데 확실히<br>MVC2 패턴이 팀원들과 협업 했을 때에 기능을 나누어 만들기 편했고, 취합 하는 것 또한 수월했다.

   

   이번 프로젝트를 진행 하면서 더 완벽하게 더 깔끔한 그런 웹을 작업하고 싶었지만,  <br>현재 배운것들로는 구현하고 싶었던 다른 기능들은 AJAX나 배우지 못한 다른 기능들을 이용해야 해서 구현하지 못한게 아쉬웠다.

   

   다음 프로젝트는 Spring과 Framework를 이용하여 이번 프로젝트를 이어서 서버의 응답을 더 빠르게<br>코드들을 간결화 하여 기능들을 보안하고 정말 운영하고 있는 사이트로 배포할 수 있을 만큼의 퀄리티로<br>완벽하게 만들 예정이다.

## [프로젝트 주소](https://github.com/GreenteaPIE/TeamProjectDBSpringVer)
