---
layout: post
title: First Team Project (Pet Talk)
date: 2023-03-22
excerpt: "첫 번째 팀 프로젝트"
tags: [project, java, MySQL]
feature: /img/PetTalk/main.png
project: true
comments: true

---



> **사용한 플랫폼 : Eclipse, MySQL**


# **시작하며**

1인 가구 증가로 반려 동물을 키우는 가구 수가 꾸준히 증가하며,<br>반려 동물의 사회성을 길러주고 코로나로 인해 지난 2년 간 고립되어 있던 사람들 간의<br>친목을 다지기 위해서 채팅 프로그램과 반려 동물에 관련한 정보와 간단한 즐거움을 줄 수 있는<br>사이드 기능을 추가하여 Pet Talk이란 프로젝트를 고안해 보았다.

## 프로젝트 명세서

1. [프로젝트 진행 순서](#1-프로젝트-진행-순서)
2. [개요](#2-개요)
3. [기능 별 요구 사항](#3-기능-별-요구-사항)
4. [DB 설계](#4-db-설계)
5. [역할 분담](#5-역할-분담)
6. [화면 설계서](#6-화면-설계서)
7. [개선 사항 과 느낀 점](#7-개선-사항과-느낀-점)

### 1. 프로젝트 진행 순서

DB 설계와 메인 틀을 먼저 잡은 후<br>회원 가입과 로그인 기능을 구현 후 부가적인 기능을 추가한다.

### 2. 개요

- 프로젝트 명 : Pet Talk
- 인원 : 5명
- 기간 : 2023.03.15 ~ 2023.03.21
- 기능 :
  - 유저 - 회원 가입, 로그인, 나의 정보, 반려 동물 정보 입력, 친구 추가 기능
  - 어드민 - 회원 관리 기능
  - 채팅 - 다중 접속 채팅, 귓속말 기능
  - 그림판 - 그림 그리기 기능
  - 동물병원 정보 찾기
  - 
- 개발 언어 : Java 11
- 개발 환경 : Eclipse
- 데이터베이스 : MySQL
- 간단 소개 : 반려동물의 사회성을 기르고 견주들의 친목을 위한 채팅프로그램

### 3. 기능 별 요구 사항

- 회원 가입
  - 유효성 검사
    - 비밀번호를 재차 확인하여 비밀번호가 일치하는지 확인
- 로그인
  - 로그인 검사
    - 아이디와 비밀번호가 일치하지 않거나, 아이디가 존재하지 않을 경우 <br>"아이디와 비밀번호를 확인하세요" 메시지 출력
    - 아이디와 비밀번호가 일치할 시 프로그램 실행
- 나의 정보
  - 회원가입 시 입력했던 이름, 나이, 연락처, 주소를 불러옴
  - 반려동물 정보를 추가로 입력 할 수 있음
  - 추가한 반려동물 정보를 함께 불러옴
- 채팅
  - 닉네임을 입력하여 입장
  - 귓속말을 하는 명령어를 사용하면 귓속말을 할 유저에게만 채팅 가능 

- 병원정보
  - 하이퍼링크로 병원정보를 모아볼 수 있는 사이트를 연결

- 그림판
  - 여러가지 팬 색과 지우개, 크기 조절 

- 친구추가
  - ID를 검색 해 아이디가 존재하면 친구목록에 추가
  - ID가 존재 하지 않을 경우 "친구를 찾을 수 없습니다." 메시지 출력
  - 자신의 아이디를 검색 할 경우 "자신의 ID는 추가할 수 없습니다." 메시지 출력
  - 이미 추가 된 ID일 경우 "이미 추가된 ID 입니다." 메시지 출력




### 4. DB 설계

![_config.yml]({{ site.baseurl }}/img/PetTalk/db.png)

### 5. 역할 분담

- 정자윤
  - 전체UI 디자인
  - 로그인, 회원가입 구현
- 박권능
  - 채팅 구현(귓속말)
  - 백그라운드 음악 삽입
- 이규동
  - 반려동물 정보 입력
  - 마이페이지 정보 불러오기
  - 동물병원 정보 링크 연결
  - 친구검색 추가
- 정주호
  - 관리자 기능 구현
- 이연화
  - 그림 판 기능 구현
  - 일부UI 디자인

### 6.  화면 설계서

#### 회원 가입<br>

![_config.yml]({{ site.baseurl }}/img/PetTalk/join.png)

![_config.yml]({{ site.baseurl }}/img/PetTalk/joinsucs.png)

중복된 ID는 가입 할 수 없다.

#### 로그인<br>![_config.yml]({{ site.baseurl }}/img/PetTalk/mainlogin.png)

![_config.yml]({{ site.baseurl }}/img/PetTalk/login.png)

ID 와 PASSWORD 가 일치해야 로그인이 가능 하다.

##### 친구 추가<br>

<iframe width="560" height="315" src="//www.youtube.com/embed/4ugEFmOfpNQ" frameborder="0"> </iframe>

회원 가입을 할 때 Friendlist table 에 userid를 저장하고 ID검색 시  Friendlist table에서 userid를 찾아 값이 존재 할 때 로그인한 userid 값과 검색한 userid값을 table에 저장하고 목록에 보여 준다.

#### 나의 정보(반려 동물 정보 입력)<br>

![_config.yml]({{ site.baseurl }}/img/PetTalk/petup.jpg)

![_config.yml]({{ site.baseurl }}/img/PetTalk/myinfo.jpg)

회원 가입 시에 입력한 정보를 불러오고 반려 동물 정보를 추가로 입력하면 반려 동물의 정보도 불러온다.

#### 채팅<br>

![_config.yml]({{ site.baseurl }}/img/PetTalk/chat.jpg)

닉네임을 설정하고 입장하면 입장한 유저들과 대화를 할 수 있다.

#### 동물 병원 정보<br>

![_config.yml]({{ site.baseurl }}/img/PetTalk/petho.jpg)

하이퍼링크를 통해 모든 동물 병원의 정보를 검색할 수 있는 홈페이지로 이동한다.

#### 그림 판<br>

![_config.yml]({{ site.baseurl }}/img/PetTalk/draw.jpg)

팬 색과 굵기, 지우개와 굵기를 설정하여 그림을 그릴 수 있다.

#### 관리자<br>

![_config.yml]({{ site.baseurl }}/img/PetTalk/adminlogin.jpg)

![_config.yml]({{ site.baseurl }}/img/PetTalk/admin.jpg)

가입한 유저들의 정보를 보고 관리 할 수 있다.

### 7. 개선 사항과 느낀 점 

1. 개선 사항

   1. 로그인 부분
      1. 비밀번호 유효성 검사 부재
      2. 비밀번호에 특수문자, 영어, 숫자 등 조합 필수 조건 걸기
   2. 관리자 창 부분
      1. 회원 정보 수정 기능 추가
      2. 관리창 UI개선
   3. 채팅창 부분
      1. 1:1 대화방 구현
      2. 채팅창 욕설 필터
      3. DB 와 연결하여 채팅내역 저장 및 접속기록 관리
   4. 나의 정보 부분
      1. 나의 정보 수정 기능 추가
      2. 프로필 이미지 추가
   5. 친구 추가 부분
      1. 추가한 친구에게 1:1 대화 구현
      2. 친구정보 보기
   6. 동물 병원 정보 부분
      1. 사이트 이동이 아닌 사이트 내에서 정보 데이터를 직접 불러오기

2. 느낀 점

   머리로는 만들고 싶은 것들을 구상만 넘쳐 날 정도로 했으나 아직 java언어를 배운지 2~3주차의 배움단계라
   손이 아이디어를 따라잡지 못해 막막했다.

   하지만 팀원들의 화합을 통해 하나 씩 해결하며 원하는 기능을 구현하기 위해 이것저것 찾아보고 수정,보안하다 보니 맨 처음 이곳에 발을 들였을 때의 나보다 한층 더 개발자의 길에 들어선 기분이 였다. 

   시행착오도 많았지만 구상한 그림이 그려진 것에 대해 팀원 모두에게 감사한 마음 뿐이다.

    여기서 더 나아가 이번 프로젝트 경험을 토대로 다음 프로젝트 때의 더 견고한 역할 분담과 진행 과정을 어떻게 해야 할 지 효율적인 방법과 계획 구상했고, 개인 프로젝트 또한 어떻게 진행 할 지 구상하게 되었다.

## [프로젝트 주소](https://github.com/GreenteaPIE/FirtTeamProjectPetTalk)