---
layout: post
title: Project(DiamondBlack) Spring Version 
date: 2023-06-15
excerpt: "프로젝트 DB Spring Version[23.05.11 ~ 23.06.01]"
tags: [project, java, jsp, Oracle, css, HTML, BootStrap, API, JQuery, JavaScript, Spring, FrameWork]
feature: /img/SpringDB/logo.png
project: true
comments: true
---

> **사용한 플랫폼 : Spring, Oracle**

<details>
<summary><span class="summary-text" style="font-size:25pt; font-weight:bold; color:gray;">>목차 접기/펼치기<</span></summary>
<div markdown="1">
- [**시작하며**](#시작하며)
  * [프로젝트 명세서](#프로젝트-명세서)
    + [1. 프로젝트 진행 순서](#1-프로젝트-진행-순서)
    + [2. 개요](#2-개요)
    + [3. 기능 별 요구 사항](#3-기능-별-요구-사항)
    + [4. DB 설계](#4-db-설계)
    + [5. API 설계](#5-api-설계)
    + [6.  화면 설계서](#6-화면-설계서)
    + [7. 개발 내용](#7-개발-내용)
    + [8. 완료한 개선 사항 / 개선 사항과 느낀 점](#8-완료한-개선-사항--개선-사항과-느낀-점)
  * [이전 DB 프로젝트 보기 / 프로젝트 주소](#이전-프로젝트-보러가기--프로젝트-주소)
</div>
</details>

# **시작하며**

기존에 만들었던 프로젝트 DB를 Spring의 Framework 라이브러리를 이용하여 프로젝트에 기능을 구현하고 싶었지만 못 만들었던 기능들을 추가로 구현하고, 아쉬웠던 기능들의 Detail을 살려 다시 만들어볼 예정이다.

## 프로젝트 명세서

1. 프로젝트 진행 순서
2. 개요
3. 기능 별 요구 사항
4. DB 설계
5. API 설계
6. 화면 설계서
7. 개발 내용
8. 완료한 개선 사항 / 개선 사항과 느낀 점

### 1. 프로젝트 진행 순서

기존에 있던 DB에서 구현할 기능들에 필요한 컬럼들을 정리 후 추가하고 스프링에 설정할 종속성들을 pom.xml에 넣고 기본적인 설정을 마친 후 설계한 API를 토대로 기존의 jsp파일들을 가져와 수정하고 추가한다. 

기능들을 하나씩 구현할때마다 JUnit 테스트를 통해 모듈들이 정상적으로 동작하는지 확인한다.

### 2. 개요

- 프로젝트 명 : DiamondBlack Spring Version

- 인원 : 1명

- 기간 : 2023.05.11 ~ 2023.06.01 

  (~ 2023.06.13 리펙터링)

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
       - 상품 등록, 수정, 삭제
    4. 브랜드 관리
       - 브랜드 추가, 삭제
    5. 옥션
       - 옥션 상품 관리
       - 시간 설정 및 시작가격 설정 등
  - 게시판
    1. 자유&질문&공지 게시판
       - CRUD기능, 조회수, 페이징 처리, 댓글, 멀티 이미지 첨부, 게시글 검색
    2. QnA
       - 자주 묻는 질문 확인
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
    1. 어드민이 상품 등록 시 할인율을 1% 이상 설정 후 등록
  
- 개발 언어 : Java 11, HTML, JavaScript, JSP, JQuery

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
    - 장바구니 수량 조절, 0이하로 수량을 조정하려 하면 "장바구니에서 삭제 하시겠습니까?" 메시지 출력
  - 장바구니 상품 구매
    - 장바구니가 비어있는 상태로 구매 페이지로 이동하려 하면 "구매할 상품이 없습니다." 메시지 출력
    - 주문자 정보와 동일 버튼 체크 시 배송지 입력에 유저정보 자동입력
    - 쿠폰 적용시 해당 쿠폰의 할인 가격 만큼 총 결제 금액에서 제외
    - 구매 시 이용 약관에 동의 하지 않았거나, 결제 방식을 선택 하지 않았으면<br>각각의 선택 메시지 출력
    - 위의 이용 약관과 결제 방식을 선택 후 구매 버튼을 눌렀다면, 결제API 모듈 실행
    - 구매 완료 시 주문 정보 확인 가능
  - 옥션 상품 입찰, 구매
    - 최종 낙찰된 입찰자만 1회 구매 가능
  - 세일 상품 구매
    - 어드민이 등록한 상품 할인율 만큼 할인된 가격으로 구매 가능
  - 자유&질문 게시판 이용
    - 등록, 수정, 삭제 가능(수정과 삭제는 작성한 본인만 가능)
    - 댓글 작성, 수정, 삭제(수정과 삭제는 작성한 본인만 가능)
    - 게시물 검색 가능(제목, 내용, 작성자)
  - 공지사항 이용
    - 등록, 수정, 삭제는 어드민만 가능
  - Q&A 이용
    - 열람만 가능
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
    - 자유&질문 게시판에 작성한 나의 글 확인 및 수정, 삭제 가능
    - 내가 단 댓글 확인 및 수정, 삭제 가능
  - 보유 쿠폰
    - 수령한 쿠폰의 리스트를 확인
- 어드민
  - 상품관리
    1. 브랜드 관리
       - 브랜드 로고 이미지와 브랜드 명을 추가, 삭제 가능
       - 브랜드 추가 시 categories 와 main page에 해당 브랜드 자동 추가
    2. 상품 관리
       - 브랜드, 상품 카테고리, 상품 이름, 사이즈, 가격, 성별, 이미지,<br>상품 설명, 재고량, 할인율 등을 설정하여 추가, 수정, 삭제 가능
       - 필수값을 입력하지 않으면 경고 메시지 출력
       - 상품 검색이 가능
  - 회원 관리
    1. 회원 정보 수정
       - 가입 일자, 비밀번호를 제외한 모든 정보 수정 가능
  
    2. 회원 삭제
  - 게시판 관리
    1. 자유&질문&공지 게시판 관리
       - 자유 게시판 등록, 수정, 삭제 가능
       - 댓글 등록, 수정, 삭제 가능
  - 옥션
    1. 옥션 상품 등록
       - 등록할 상품을 선택 후 시작가, 제한시간 설정
       - 기간이 만료 된 후엔 입찰 불가능
  - 세일
    1. 어드민이 상품 할인률을 설정하여 등록하면  Sale페이지에 할인율이 적용된 가격으로 노출
  - 매출/주문 관리
    1. 판매한 총 매출 가격과 구매자, 주문 번호 출력
    1. 각 단계 별로 주문 요청을 확인->배송요청 처리 가능
    1. 취소 요청 주문을 확인하여 주문 취소 가능
- 상품
  - 브랜드 로고 클릭 시 해당 브랜드의 모든 상품 리스트 출력
  - 카테고리 클릭 시 해당 브랜드의 카테고리가 설정된 상품 리스트 출력
  - 상품 검색 시 해당 검색어가 들어가는 상품 리스트 출력
  - 검색한 상품이 존재 하지 않을 시 "상품 "xx"의 검색 결과 없음" 메시지 출력
  - 상세 페이지에서 수량과 사이즈를 선택하여 장바구니에 추가 가능
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

<br>

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

#### BOARD_REPLY

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

PRODUCT TABLE에서 컬럼 [PURCHASEDNUM(구매번호)는 사용하지 않음으로 제거한다.

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

<br>

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

ORDERS TABLE 과 ORDER_DETAIL TABLE 을 JOIN 하여 두 TABLE을 엮어 원하는 값을 불러올 수 있게 VIEW를 생성하는 SQL문을 추가한다.

#### AUCTION

|   컬럼명   | 데이터 타입 |   조건   |        설명         |
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

#### 유저 관련 API

|   Description   |      Return Page       |             url             |                           Request                            |        Response        |
| :-------------: | :--------------------: | :-------------------------: | :----------------------------------------------------------: | :--------------------: |
| 회원가입 페이지 | 회원가입 페이지로 이동 |       GET /user/join        |                              -                               |           -            |
|    회원가입     |  회원가입 환영 페이지  |       POST /user/join       | String userid<br />String pass<br />String name<br />String email<br />String address1<br />String address2<br />String address3<br />String phone<br />Int gender<br />Int point<br />Int grade<br />SYSDATE enter |           -            |
| 아이디 중복검사 |    회원가입 페이지     |    POST /user/userIDChk     |                        String userid                         |         user[]         |
|   이메일 인증   |    회원가입 페이지     |     GET /user/mailCheck     |                              -                               |           -            |
|  로그인 페이지  |  로그인 페이지로 이동  |       GET /user/login       |                              -                               |           -            |
|     로그인      |      메인 페이지       |      POST /user/login       |                String userid<br />String pass                |         user[]         |
|    로그아웃     |      메인 페이지       |      GET /user/logout       |                              -                               |           -            |
|  내 정보 수정   |    본인 인증 페이지    |     GET /user/mypagechk     |                              -                               |           -            |
|    본인 인증    |  내 정보 수정 페이지   |    POST /user/mypagechk     |                String userid<br />String pass                |         user[]         |
|  내 정보 수정   |      메인 페이지       |    POST /user/userupdate    |                            user[]                            |           -            |
|    회원탈퇴     |      메인 페이지       |     POST /user/userexit     |                        String userid                         |           -            |
|  Event 페이지   |  Event 페이지로 이동   |         GET /event          |                              -                               |           -            |
|    쿠폰 발급    |      Event 페이지      |    POST /user/addCoupon     | String userid<br />String couponname<br />String imgurl<br />Int discountprice |           -            |
|    보유 쿠폰    |    보유 쿠폰 페이지    |     GET /user/myCoupon      |                        String userid                         |        coupon[]        |
| 나의 주문 내역  |    주문 내역 페이지    |    GET /user/myPurchased    |                        String userid                         |        order[]         |
| 주문 내역 상세  |    주문 상세 페이지    | GET /user/myPurchasedDetail |                       Int ordernumber                        | order[]<br />product[] |
|    주문 취소    |      비동기 작동       |   GET /user/withdrawOrder   |                       Int ordernumber                        |           -            |
|  나의 작성 글   |  나의 작성 글 페이지   |     GET /user/myWriting     |                        String userid                         |        board[]         |
#### 상품 관련 API

|      Description       |       Return Page       |              url               |                           Request                            |                           Response                           |
| :--------------------: | :---------------------: | :----------------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
|     브랜드 리스트      |       메인 페이지       |             GET /              |                              -                               |                           brand[]                            |
|   브랜드 상품 리스트   |   상품 리스트 페이지    | GET /product/brandProductList  |                         String bname                         |                          product[]                           |
| 카테고리별 상품 리스트 |   상품 리스트 페이지    |  GET /product/categoriesList   |                  String bname<br />Int kind                  | String pname<br />Int price<br />Int discountrate<br />String imgurl<br />Int num |
|       상품 검색        |   상품 리스트 페이지    |   GET /product/searchProduct   |                         String pname                         | String pname<br />Int price<br />Int discountrate<br />String imgurl<br />Int num |
|    세일 상품 리스트    |       상품 리스트       |     GET /product/saleList      |                              -                               | String pname<br />Int price<br />Int discountrate<br />String imgurl<br />Int num |
|     상품 상세보기      |   상품 디테일 페이지    |   GET /product/productDetail   |                  Int num<br />String pname                   |                          product[]                           |
|     장바구니 추가      |   상품 디테일 페이지    |     POST /product/addCart      | String userid<br />Int num<br />String psize<br />Int quantity<br />Int price |                              -                               |
|     나의 장바구니      | 장바구니 리스트 페이지  |      GET /product/myCart       |                        String userid                         |                            cart[]                            |
|     장바구니 뱃지      |       비동기 작동       |   GET /product/countCartAjax   |                        String userid                         |                        count int num                         |
|   장바구니 수량 감소   |       비동기 작동       |  POST /product/quantityMinus   |                         Int cartnum                          |                              -                               |
|   장바구니 수량 증가   |       비동기 작동       |   POST /product/quantityPlus   |                         Int cartnum                          |                              -                               |
|   장바구니 상품 삭제   |       비동기 작동       |    POST /product/deleteCart    |                         Int cartnum                          |                              -                               |
|     상품 주문 결제     |     체크아웃 페이지     |     GET /product/checkOut      |                        String userid                         |             product[]<br />cart[]<br />coupon[]              |
|       결제 완료        |    주문 완료 페이지     |    POST /product/purchased     | Integer cnum<br />Int cartnum<br />String userid<br />Int totalprice<br />String email<br />String phone<br />String address1<br />String address2<br />String address3<br />Cart[] |                    order[]<br />product[]                    |
|     옥션 체크아웃      | 체크아웃 페이지로 이동  |  GET /product/auctionCheckOut  |                          Int aunum                           |                          auction[]                           |
|       옥션 결제        | 결제 완료 페이지로 이동 | POST /product/auctionPurchased | String userid<br />Int totalprice<br />Int aunum<br />Int endprice<br />String email<br />String phone<br />String address1<br />String address2<br />String address3<br />Cart[] |                              -                               |

#### 게시판 관련 API

|     Description     |        Return Page        |        url         |                           Request                            |       Response       |
| :-----------------: | :-----------------------: | :----------------: | :----------------------------------------------------------: | :------------------: |
| 게시판 등록 페이지  | 게시판 등록 페이지로 이동 | GET /board/enroll  |                              -                               |          -           |
|     게시판 등록     |       게시판 리스트       | POST /board/enroll | String userid<br/>Int num<br/>String title<br />String content<br />String category |          -           |
|     게시판 목록     |       게시판 리스트       |  GET /board/list   |                       String caregory                        |       board[]        |
| 게시판 목록(페이징) |       게시판 리스트       |  GET /board/list   |                              -                               |      criteria[]      |
|     게시판 조회     |    게시판 상세 페이지     |   GET /board/get   |                           Int num                            | board[]<br />reply[] |
| 게시판 수정 페이지  | 게시판 수정 페이지로 이동 | GET /board/modify  |                           Int num                            |       board[]        |
|     게시판 수정     |       게시판 리스트       | POST /board/modify | String userid<br/>Int num<br/>String title<br />String content<br />String category |          -           |
|     게시판 삭제     |       게시판 리스트       | POST /board/delete |                           Int num                            |          -           |
|      댓글 작성      |    게시판 상세 페이지     | POST /reply/write  |        Int num<br />String content<br />String writer        |          -           |
|  댓글 수정 페이지   |  댓글 수정 페이지로 이동  | GET /reply/modify  |                           Int num                            |       reply[]        |
|      댓글 수정      |    게시판 상세 페이지     | POST /reply/modify |        Int num<br />String content<br />String writer        |          -           |
|      댓글 삭제      |    게시판 상세 페이지     | GET /reply/delete  |                           Int num                            |          -           |

#### 어드민 관련 API

|       Description       |           Return Page            |                   url                   |                           Request                            |    Response     |
| :---------------------: | :------------------------------: | :-------------------------------------: | :----------------------------------------------------------: | :-------------: |
|    회원 관리 페이지     |     회원 관리 페이지로 이동      |      GET /admin/userMangementPage       |                              -                               |     users[]     |
|  회원 정보 수정 페이지  |     회원 수정 페이지로 이동      |      GET /admin/userManagementEdit      |                        String userid                         |     user[]      |
|     회원 정보 수정      |         회원 관리 페이지         |     POST /admin/userEditComplete.do     | String userid<br />String name<br />String email<br />String address1<br />String address2<br />String address3<br />String phone<br />Int gender<br />Int point<br />Int grade |        -        |
|     회원 정보 삭제      |         회원 관리 페이지         |          GET /admin/userDelete          |                        String userid                         |        -        |
| 브랜드/상품 관리 페이지 |  브랜드/상품 관리 페이지로 이동  |    GET /admin/productManagemnetPage     |                              -                               |        -        |
|   브랜드 관리 페이지    |    브랜드 관리 페이지로 이동     |        GET /admin/adminBrandList        |                              -                               |     brand[]     |
|   브랜드 등록 페이지    |    브랜드 등록 페이지로 이동     |     GET /admin/adminbrandWriteForm      |                              -                               |        -        |
|       브랜드 등록       |        브랜드 관리 페이지        |        POST /admin/brandWrite.do        |               String bname<br />String imgurl                |        -        |
|   브랜드 삭제 페이지    |    브랜드 삭제 페이지로 이동     |       GET /admin/adminBrandDelete       |                              -                               |     brand[]     |
|       브랜드 삭제       |        브랜드 관리 페이지        |       POST /admin/deleteBrand.do        |                         String bname                         |        -        |
|    상품 관리 페이지     |     상품 관리 페이지로 이동      |       GET /admin/adminProductList       |                              -                               |   products[]    |
|    상품 등록 페이지     |     상품 등록 페이지로 이동      |    GET /admin/adminProductWriteForm     |                              -                               |        -        |
|        상품 등록        |         상품 관리 페이지         |    POST /admin/adminProductWriteForm    | Int num<br />Int pgender<br />String bname<br />String pname<br />Int kind<br />String imgurl<br />String psize<br />Int price<br />Int discountrate<br />Int balance<br />String explain |        -        |
|    상품 이미지 등록     |          이미지 업로드           |      POST /admin/uploadAjaxAction       |                        String imgurl                         |        -        |
|       브랜드 선택       |         상품 등록 페이지         |           GET /admin/brandPop           |                              -                               |  String bname   |
|        상품 정보        |     상품 정보 페이지로 이동      |      GET /admin/adminProductDetail      |                           Int num                            |    product[]    |
|  상품 이미지 불러오기   |         상품 정보 페이지         |           GET /admin/display            |                           Int num                            | String fileName |
|  상품 정보 수정 페이지  |     상품 수정 페이지로 이동      |      GET /admin/adminProductModify      |                           Int num                            |    product[]    |
|     상품 정보 수정      |         상품 수정 페이지         |     POST /admin/adminProductModify      | Int pgender<br />String bname<br />String pname<br />Int kind<br />String imgurl<br />String psize<br />Int price<br />Int discountrate<br />Int balance<br />String explain |        -        |
|        상품 삭제        |         상품 관리 페이지         |     POST /admin/adminProductDelete      |                           Int num                            |        -        |
|     매출/주문 관리      |   매출/주문 관리 페이지로 이동   |    GET /admin/sales_OrderManagement     |                          Int result                          |     order[]     |
|     취소 주문 확인      |   취소 요청 주문 페이지로 이동   |      GET /admin/withDrawOrderCheck      |                       Int ordernumber                        |     order[]     |
|     취소 요청 처리      |           비동기 작동            |        POST /admin/withdrawOrder        |                       Int ordernumber                        |        -        |
|     신규 주문 확인      |   신규 주문 확인 페이지로 이동   |         GET /admin/orderProcess         |                       Int ordernumber                        |     order[]     |
|     신규 주문 처리      |           비동기 작동            |         POST /admin/checkOrder          |                       Int ordernumber                        |        -        |
|     배송 요청 확인      |   배송 요청 확인 페이지로 이동   |       GET /admin/shipmentProcess        |                       Int ordernumber                        |     order[]     |
|     배송 요청 처리      |           비동기 작동            |        POST /admin/shopmentOrder        |                       Int ordernumber                        |        -        |
|        옥션 등록        |   브랜드 리스트 페이지로 이동    |    GET /admin/adminAuctionBrandList     |                              -                               |     brand[]     |
|        옥션 등록        | 브랜드 상품 리스트 페이지로 이동 | GET /admin/adminAuctionBrandProductList |                         String bname                         |    product[]    |
|        옥션 등록        |    상품 디테일 페이지로 이동     |  GET /admin/auctionBrandProductDetail   |                         String pname                         |    product[]    |
|        옥션 등록        |           메인 페이지            |        POST /admin/addAuction.do        | String userid<br />String bname<br />String pname<br />Int price<br />Int startprice<br />Date endtime<br />Int onoff<br />String psize<br />String imgurl<br />Int num |        -        |
|   옥션 리스트 페이지    |    옥션 리스트 페이지로 이동     |        GET /product/auctionView         |                              -                               |    auction[]    |
|    옥션 상세 페이지     |     옥션 상세 페이지로 이동      |       GET /product/auctionDetail        |                  Int num<br />String pname                   |    auction[]    |
|        옥션 입찰        |         옥션 상세 페이지         |      POST /product/dealAuction.do       |          Int price<br />Int num<br />String userid           |        -        |
|        옥션 만료        |         옥션 상세 페이지         |     POST /product/expireAuction.do      |                           Int num                            |        -        |
|       옥션 입찰가       |           비동기 작동            |         POST /product/getPrice          |                           Int num                            |        -        |

### 6. 화면 설계서

#### 로그인을 하지 않았을 경우<br>

<details>
<summary class="summary-text">>접기/펼치기<</summary>
<div markdown="1">
<iframe width="560" height="315" src="//www.youtube.com/embed/h38ZMZWg_ew" frameborder="0"> </iframe>
    로그인을 하지 않았을 경우엔 게시판 등록, 상품 구매, 장바구니, 쿠폰수령등의 기능들을 이용할 수 없다.
</div>
</details>
로그인을 하지 않았을 경우엔 게시판 등록, 상품 구매, 장바구니, 쿠폰수령등의 기능들을 이용할 수 없다.

#### 회원 가입 <br>
<details>
<summary class="summary-text">>접기/펼치기<</summary>
<div markdown="1">
<iframe width="560" height="315" src="//www.youtube.com/embed/v-okLiZIZWw" frameborder="0"> </iframe>
</div>
</details>
회원 가입은 유효성 검사를 거쳐 진행된다.

회원 가입시 입력한 패스워드는 아래처럼 암호화 되어 저장된다.

![_config.yml]({{ site.baseurl }}/img/DiamondBlack/sha256.png)

#### 로그인 & 로그아웃<br>
<details>
<summary class="summary-text">>접기/펼치기<</summary>
<div markdown="1">
<iframe width="560" height="315" src="//www.youtube.com/embed/DU73zKQwWbg" frameborder="0"> </iframe>
</div>
</details>

#### 내 정보 수정 & 탈퇴<br>
<details>
<summary class="summary-text">>접기/펼치기<</summary>
<div markdown="1">
<iframe width="560" height="315" src="//www.youtube.com/embed/pD0cKIQ_gNk" frameborder="0"> </iframe>
</div>
</details>
 PW을 한번 더 확인하여 수정 페이지로 넘어가고 회원가입과 같은 유효성 검사를 진행하여 정보 수정을 완료한다.

내 정보 수정 페이지 에서 confirm을 이용해 탈퇴 진행 여부를 한번 더 확인 후 탈퇴를 한다.

#### 상품(검색, 카테고리, 세일) 리스트 & 디테일<br>

<details>
<summary class="summary-text">>접기/펼치기<</summary>
<div markdown="1">
<iframe width="560" height="315" src="//www.youtube.com/embed/AxOU4ZN1FGg" frameborder="0"> </iframe>
</div>
</details>
메인 페이지의 브랜드 로고를 클릭하면 해당 브랜드의 모든 상품을 불러오고, 카테고리의 각 브랜드 별 Top, Bottom, Boutique를 클릭하면 카테고리에 맞는 상품 리스트를 불러온다. 

상품 검색 창에 검색한 단어를 포함한 상품을 출력하며, 검색한 단어를 포함한 상품이 존재하지 않을 시 검색 결과 없음 페이지를 보여준다.

#### 장바구니 담기 & 나의 장바구니<br>

<details>
<summary class="summary-text">>접기/펼치기<</summary>
<div markdown="1">
<iframe width="560" height="315" src="//www.youtube.com/embed/At4hkN3_9JA" frameborder="0"> </iframe>
</div>
</details>

상품 디테일 페이지에서 상품을 장바구니에 담으면 Ajax를 이용해 비동기로 장바구니를 추가하는 메서드를 실행한다. 

추가 되어있는 장바구니 수만큼 오른쪽 상단 뱃지에 숫자로 표기 되고 클릭하면 유저의 장바구니 리스트를 불러온다.

#### 쿠폰 발급 & 보유 쿠폰<br>

<details>
<summary class="summary-text">>접기/펼치기<</summary>
<div markdown="1">
<iframe width="560" height="315" src="//www.youtube.com/embed/5qZ8n1p8x6A" frameborder="0"> </iframe>
</div>
</details>
#### 상품 결제<br>

<details>
<summary class="summary-text">>접기/펼치기<</summary>
<div markdown="1">
<iframe width="560" height="315" src="//www.youtube.com/embed/tbXL0L7wo-A" frameborder="0"> </iframe>
</div>
</details>

옥션(경매)에 낙찰되면 낙찰자는 구매버튼을 이용하여 상품을 구매 할 수 있고, 상품 페이지에서 장바구니에 담은 상품을 일괄적으로 구매할 수 있다.

결제는 실제로 이루어지며, 결제test모듈이기때문에 자정이되면 payback된다.<br>
상품의 금액이 높기 때문에 테스트를 위해 결제를 취소해도 결제 완료 페이지로 넘어가게 구현했다.

#### 나의 주문 내역 & 주문 취소

<details>
<summary class="summary-text">>접기/펼치기<</summary>
<div markdown="1">
<iframe width="560" height="315" src="//www.youtube.com/embed/BtGtNDj5HqM" frameborder="0"> </iframe>
</div>
</details>

주문 번호를 클릭하면 상세 주문 내역을 볼 수 있고, 주문 취소 요청을 통해 어드민이 요청을 확인하고 취소 처리를 할 수 있다.

#### 게시판<br>

<details>
<summary class="summary-text">>접기/펼치기<</summary>
<div markdown="1">
<h3>게시판 등록</h3>
<iframe width="560" height="315" src="//www.youtube.com/embed/7uV3IuUpsYI" frameborder="0"> </iframe><br>자유/질문 게시판은 모든 유저가 열람 가능 하지만 게시판 등록은 회원가입한 유저만 이용 할 수 있고, 게시글이 10개가 넘어가면 다음 페이지로 이동된다. 공지사항 게시판의 등록과 수정, 삭제는 어드민에게만 권한이 있다.<br>
<h3>게시판 수정/삭제</h3> 
<iframe width="560" height="315" src="//www.youtube.com/embed/GQn5aW6B7tc" frameborder="0"> </iframe><br>본인이 작성한 게시글만 수정 및 삭제가 가능하다. 어드민은 모든 수정, 삭제 권한이 있어 모니터링하며 관리가 가능하다.<br>
<h3>게시판 검색</h3>
<iframe width="560" height="315" src="//www.youtube.com/embed/HaBYEj4gytE" frameborder="0"> </iframe><br>제목과 작성자, 내용 등 으로 게시판을 검색 할 수 있다.<br>
<h3>댓글 등록/수정/삭제</h3>
<iframe width="560" height="315" src="//www.youtube.com/embed/OuPH-l6NVy8" frameborder="0"> </iframe><br>본인이 작성한 댓글만 수정, 삭제가 가능하다. 로그인을 하지 않았을 때는 댓글작성이 불가능 하다.<br>
<h3>나의 작성 글</h3>
<iframe width="560" height="315" src="//www.youtube.com/embed/nmREYGqOFPQ" frameborder="0"> </iframe><br>내가 작성한 글을 확인 할 수 있다.<br>
</div>
</details>

#### 어드민<br>

<details>
<summary class="summary-text">>접기/펼치기<</summary>
<div markdown="1">
<h3>회원 관리</h3>
<iframe width="560" height="315" src="//www.youtube.com/embed/-kaAgJiXiaY" frameborder="0"> </iframe><br>관리자는 회원의 정보를 수정 및 삭제 동작을 실행 할 수 있다.<br>
<h3>브랜드 관리</h3> 
<iframe width="560" height="315" src="//www.youtube.com/embed/iRe5N6e0-iE" frameborder="0"> </iframe><br>브랜드를 추가/삭제를 할 수 있고 추가한 브랜드를 삭제 시 해당 브랜드의 상품 정보또한 같이 삭제된다.<br>
<h3>상품 관리</h3>
<iframe width="560" height="315" src="//www.youtube.com/embed/c3Dn0kGsDZY" frameborder="0"> </iframe><br>상품 추가/삭제를 할 수 있고 할인율을 1%이상으로 설정 시 SALE 항목으로 들어간다.<br>
<h3>매출/주문 관리</h3>
<iframe width="560" height="315" src="//www.youtube.com/embed/IVmFyfdtFoo" frameborder="0"> </iframe><br>회원이 취소요청한 주문을 처리할 수 있고, 구매요청 주문을 확인->배송->완료 처리 할 수 있다. 완료 처리된 주문은 해당 사용자에게 구매한 금액의 일부분이 포인트로 지급된다.<br>
<h3>옥션(경매) 관리</h3>
<iframe width="560" height="315" src="//www.youtube.com/embed/7ZTdFIejO3w" frameborder="0"> </iframe><br>옥션(경매)를 등록하여 시작 할 수 있고 설정한 시간이 도달하면 옥션(경매)가 만료된다. 낙찰자는 상품을 1회 구매할 수 있다.<br>
</div>
</details>

게시판 같은 경우는 직접 모니터링 하며 수정/삭제를 할 수 있게 만들었기 때문에 게시판 관리창을 추가하지 않았다.

### 7. 개발 내용

Junit 을 통한 Test 과정은 아래와 같이 하나의 메소드를 추가 할때마다 진행 하였고, 개발 내용에서 생략 하였다.

![_config.yml]({{ site.baseurl }}/img/SpringDB/test.png)

[1 - Spring 초기 설정](https://greenteapie.github.io/DBSpringVer-first-setting/)<br>[2 - Main 페이지 추가](https://greenteapie.github.io/DBSpringVer-main-page/)<br>[3 - 회원가입 페이지 & 암호화(인코딩)기능 추가 ](https://greenteapie.github.io/DBSpringVer-add-join/)<br>[4 - 로그인(로그아웃) 페이지 & 기능 추가 ](https://greenteapie.github.io/DBSpringVer-add-login/)<br>[5 - 내 정보 수정(탈퇴) 페이지 & 기능 추가](https://greenteapie.github.io/DBSpringVer-add-myinfo/)<br>[6 - 상품 리스트(카테고리, 검색, 세일) 페이지 & 기능 추가](https://greenteapie.github.io/DBSpringVer-add-productlist/)<br>[7 - 상품 디테일 페이지 & 기능 추가](https://greenteapie.github.io/DBSpringVer-add-productdetail/)<br>[8 - 장바구니(담기, 리스트) 페이지 & 기능 추가](https://greenteapie.github.io/DBSpringVer-add-mycart/)<br>[9 - 쿠폰(발급, 나의 쿠폰) 페이지 & 기능 추가](https://greenteapie.github.io/DBSpringVer-add-coupon/)<br>[10 - 상품 결제 페이지 & 기능 추가](https://greenteapie.github.io/DBSpringVer-add-purchased/)<br>[11 - 나의 결제내역(주문취소) 페이지 & 기능 추가](https://greenteapie.github.io/DBSpringVer-add-mypurchased/)<br>[12 - 게시판 등록 페이지 & 기능 추가](https://greenteapie.github.io/DBSpringVer-add-enrollboard/)<br>[13 - 게시판 리스트(페이징) 페이지 & 기능 추가](https://greenteapie.github.io/DBSpringVer-boardlist/)<br>[14 - 게시판 검색 기능 추가](https://greenteapie.github.io/DBSpringVer-add-boardsearch/)<br>[15 - 게시판 페이지 조회&수정&삭제 기능 추가](https://greenteapie.github.io/DBSpringVer-add-modifyboard/)<br>[16 - 게시판 댓글(등록, 수정, 삭제) 기능 추가](https://greenteapie.github.io/DBSpringVer-add-reply/)<br>[17 - 나의 작성 글 페이지 & 기능 추가](https://greenteapie.github.io/DBSpringVer-add-myboard/)<br>[18 - 어드민 회원 관리 기능 추가](https://greenteapie.github.io/DBSpringVer-add-admincustomer/)<br>[19 - 어드민 브랜드 관리 기능 추가](https://greenteapie.github.io/DBSpringVer-add-adminbrand/)<br>[20 - 어드민 상품 관리 기능 추가](https://greenteapie.github.io/DBSpringVer-add-adminproduct/)<br>[21 - 어드민 매출/주문 관리 기능 추가](https://greenteapie.github.io/DBSpringVer-add-adminorders/)<br>[22 - 어드민 옥션(경매) 관리 기능 추가](https://greenteapie.github.io/DBSpringVer-add-adminauction/)<br>[23 - QnA 페이지와 Contact 페이지 추가](https://greenteapie.github.io/DBSpringVer-add-subpage/)<br>

### 8. 완료한 개선 사항 / 개선 사항과 느낀 점

1. 완료 개선 사항

   - 로그인 부분
      1. 이메일 인증 기능 구현
   - 유저
      1. 장바구니에 담은 상품 수를 장바구니 뱃지에 표현
      2. 결제 시 결제한 금액에 따른 포인트 지급 및 포인트에 따른 회원 등급 조정
      3. 회원 등급에 따른 상품 할인 쿠폰 지급
      4. 장바구니 담은 상품 삭제 & 수량 조정
   - 어드민
      1. 주문(확인, 배송, 취소) 처리 구현
      1. 회원 검색 기능 추가 & 리스트 페이징 구현
      1. 상품 검색 추가 & 리스트 페이징 구현
   - 상품
      1. 브랜드 추가 시 헤더와 메인 페이지에 추가된 브랜드 출력
      2. 주문지 변경 추가
      3. 할인 쿠폰 & 쿠폰 적용 구현
      4. 결제 취소 구현
      5. 옥션의 낙찰자가 상품 구매 후 해당 상품의 구매하기 버튼을 사라지게 구현
      6. 상품 디테일 페이지 ui/ux 디테일 추가
   - 게시판
      1. 게시판의 API를 이용한 댓글이 아닌 웹페이지 자체의 댓글 기능 구현
      2. 게시판 검색 기능 추가
      3. 사진 멀티업로드 기능 추가
      4. 위지윅 ck 에디터로 게시판 글 편집기 기능 추가
   - 웹 디자인
      1. 로고 변경
      2. 마우스 커서 디자인 적용
   - 리펙터링
      1. 불필요한 코드들을 삭제하고, 중복되는 코드들을 간추림

2. 개선 사항

   - 로그인 관련
     1. 구글, 카카오 API 를 이용한 회원가입
     2. 인터셉터를 사용하여 자격이 없는 사용자가 접근할 수 없게 보안
     3. 매크로 회원가입을 막는 CAPTCHA 기능
   - 상품 관련
     1. 최종 구매 혹은 주문 시에 상품 재고량 변동
     2. 상품 필터 재정렬

   - 게시판 관련
     1. 좋아요 기능
     2. 신고 기능

3. 느낀 점

   전에 만들었던 프로젝트에서 참고할 수 있는 코드는 jsp의 view단 밖에 없어서 소요되는 시간이 많았다. 하지만 Spring으로 재구성하며 기능을 구현 할때 내가 만들지 않았던 팀원들이 만든 코드들의 이해도를 높힐 수 있는 기회가 되고, 보이지 않았던 불필요한 요소들을 찾아낼 수 있었다.

   내가 만든 결과물을 호스팅 해보고 싶은 마음에 서칭을 통해서 aws, oracle cloud, 카페24로 호스팅 하는 방법도 알아보았다. 

   서버를 할당 받아서 filezilla로 호스팅 서버에 접속하고 로컬파일을 서버에 업로드 후 putty로 세션을 열어 호스팅 한다고 하는데.. 가지고있는 DB를 어떻게 활용해야 하는지 정보량도 많지 않고 개발환경이 각자 달라서 공부를 더 해야겠다고 생각했다. 

   또 git brench 로 협업하는 방법도 알아보고 각 brench를 만들어 프로젝트를 commit하고 master branch로 push 해봤는데 익숙하지 않아서 그런지 변경점이 꼬여 정상적으로  push가 되지않는 현상이 일어났다.. 뭐든 처음은 어려운법 그래서 그냥 source tree로 master branch에 바로 commit/push 해서 업로드 했다. 첫 시도는 실패였지만 다음 시도는 성공하고 말 것이다..!

## [이전 프로젝트 보러가기](https://greenteapie.github.io/Team-Project-mvc2-pattern(DiamondBlack)/) / [프로젝트 주소](https://github.com/GreenteaPIE/TeamProjectDBSpringVer)

<style>
 @keyframes blink {
  0%, 100% { color: transparent; }
  50% { color: black; }
}
.summary-text {
  animation: blink 2s linear infinite;
}
table {
  border-collapse: collapse;
}
th, td {
  border: 1px solid black;
}
th {
  font-weight: bold;
  font-size: 17px;
}
td {
  font-size: 13px;
}
</style>
