---
layout: post
title: Personal Project (FindMyDog)
date: 2023-07-27
excerpt: "개인 프로젝트 - 나의 강아지 찾기[23.07.27 ~ ing]"
tags: [project, SpringBoot, JPA, mysql]
feature: /assets/img/rucy.jpg
project: true
comments: true

---



> **사용한 플랫폼 : Spring, Oracle**

# **시작하며**

반려견을 키운지 어느덧 6년이 지났다.<br>이름은 루시, 암컷 말티즈다.<br>이제는 없어선 안될 소중한 가족구성원중 하나가 되었다.

가끔 주변에 반려견을 키우는 사람들에게 들리는 말로는<br>산책 중 한눈판 사이에 사라진다던가, 잠깐 문을 열어둔 사이 집을 나갔다던가 하는 말이 들려온다.<br>한번 시야에서 놓친 강아지는 정말 찾기 힘들다.

강아지를 키울때 보통 구청에 신고후 칩을 이식하거나 하게되는데,<br>입양 혹은 분양 하고 구청에 등록하지 않은 경우는 누가 찾게된다고 하더라도 누가 주인인지 알 수가 없다.<br>그럴때를 대비해서 잃어버리게 되더라도 찾을 확률을 높힐 수 있게 <br>칩을 등록하고 이식하는게 제일 좋은 방법이지만 그렇게 하지않은 사람과,<br>혹은 발견을 해도 놓치는 경우를 대비하여 사이트를 구상해보았다.

우선 각 지역구마다 커뮤니티를 형성해 자신의 잃어버린 반려견 정보(사진, 종, 특징 등)을<br>게시판에 입력하여 올려서 사람들과 정보를 공유 and 제보를 주고 받을 수 있고,<br>추가로 한국내에 모든 동물보호소 정보를 불러오고 지역별로 나누어 검색을 통해<br>동물보호소의 정보를 안내하고 유기견들이 다시금 새로운 가족들의 품에서<br>행복하게 지냈으면 하는 바램에 입양정보 또한 사용자에게 보여줄 수 있는 기능을 가지고 있다.



## 프로젝트 명세서

1. 프로젝트 진행 순서
2. 개요
3. 기능 별 요구 사항
4. DB 설계
5. API 설계
6. WEB 화면 설계
7. 개발 및 테스트
8. 배포
9. 도메인 설정



### 1. 프로젝트 진행 순서

웹의 기본인 CRUD 게시판을 만든 후 회원 가입, 로그인 순서로 기능을 하나 씩<br>추가해 나가는 식으로 진행

### 2. 개요

- 프로젝트 명 : Find My Dog
- 인원 : 1명
- 기간 : 2023.07.27 ~ ing
- 기능 :
  - 게시판





