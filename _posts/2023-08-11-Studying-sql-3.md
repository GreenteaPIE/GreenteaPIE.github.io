---
layout: post
title: SQL 용어 정리 - 3
date: 2023-08-11
excerpt: SQL 용어 정리 - 데이터베이스 설계 기초와 정규화"
tags: [SQL, study]
comments: true



---

#### 데이터베이스 객체 정의

데이터베이스를 스키마라고 한다. 그리고 스키마 안에 여러 가지 종류의 객체가 들어간다. 테이블, 인덱스, 트리거, 뷰와 같은 다양한 객체들을 스키마 안에 생성하고 관리한다. 그리고 객체 들과의 관계를 정의함으로써 하나의 데이터베이스를 완성한다.

그중에서 가장 중요하고, 많은 부분을 차지하는 것이 테이블의 구조를 정의하고, 테이블과 테이블의 관계를 맺는 일이다.<br>테이블을 정의한 내역을 담은 문서를 "테이블 정의서"라고 한다. 테이블 안에 어떤 열이 있고, 어떤 데이터 형을 가진 어떤 조건을 가지고 있는지를 서술한 문서이다.<br>"desc 테이블 이름"으로 확인하는 데이터베이스의 테이블 명세가 그대로 있다고 생각하면 된다.

그리고 테이블과 테이블의 관계는 "ER 다이어그램"이라고 하는 관계도로 표현을 한다. 테이블이 다른 테이블과 어떻게 연결되어 있고, 테이블의 열이 테이블의 열에 어떻게 사용되는지를 다이어그램으로 표현한 것이다.

ER은 Entity Relationship의 줄임 말이다. Entity는 요소, 즉 데이터베이스의 객체를 말한다. 데이터베이스의 다른 객체들을 대부분 테이블 한 개에 종속되거나 데이터베이스에 종속되어 있기 때문에 관계를 설명할 필요가 없다. 테이블은 다른 테이블과 관계를 맺고 서로 연결되어 있기 때문에 ER 다이어그램으로 테이블의 관계를 표시해 구조를 표현해야 한다.

건축으로 말하면 테이블 정의서는 건축 시방서와 비슷하고 ER 다이어그램은 건축 설계도와 비슷한 것이다.<br>테이블 정의서와 ER 다이어그램이 있으면 하나의 데이터베이스를 완성 할 수 있다.

#### 테이블 정의서

##### 논리이름

실제 사용하는 테이블 이름이나 열 이름과는 별개로 용도를 잘 알 수 있도록 표시하는 별도의 이름을 논리 이름이라고 한다. "주문정보" 테이블이라고 하면 쉽게 용도를 알 수 있지만 실제 사용하는 테이블 이름인 "p_order"는 용도를 정확하게 알기에는 많이 부족하다.<br>열 이름도 "link", "description" 이라는 이름 만으로는 용도가 무엇인지 정확하게 알 수가 없다. "관련 상품", "상세정보"와 같은 논리 이름으로 테이블 정의서에 표시를 해야 나중에 관리를 할 때 혼동을 하지 않게 된다.

##### 데이터 형

컬럼의 데이터 형을 표시한다. 데이터베이스마다 데이터 형이 조금씩 다르기 때문에 대상 데이터베이스에 맞춰서 데이터 형을 표시하면 된다. 특정 데이터베이스에 종속되지 않도록, "숫자", "문자열", "시간" 과 같이 범용 데이터 형 표현을 써서 데이터베이스 특성에 맞게 구현을 각자 하기도 한다.<br>숫자형으로 사용해야 하는 데이터 형을 문자열로 지정하거나, 단순 편의를 위해 모든 열의 데이터 형을 문자열로 지정하는 설계는 해서는 안된다.<br>최대한 열의 용도에 맞춰서 데이터 형이 근접하도록 사용해야 한다. 문자열 형이면 고정 길이인지, 가변형인지 구분을 해서 사용해야 하고, 숫자 형이면 양수인지 긴 정수 형인지 등을 구분해서 용도에 맞춰 사용하는 것이 좋다.

##### 제약 조건

데이터 형을 정했으면, 데이터 형의 구체적인 용도와 제한 사항을 제약 조건에 표시해야 한다. 숫자 데이터 형이면 값의 범위를 제한할 수 도 있고, 널 허용, 기본값 등 구체적으로 열의 용도를 구분해야 한다.<br>데이터 무결성을 잘 유지하기 위해서는 제약 조건이 구체적이고, 범위가 좁은 것이 좋다. 반대로 제약조건이 너무 많으면 데이터 출력 및 관리가 번거로워 진다.

##### 기본 키(Primary Key)와 외래 키(Foreign Key)

테이블의  행 데이터를 식별하는 중요한 역할을 하는 기본키는 데이터베이스를 설계할 때 가장 중요한 제약 조건이다. 테이블의 관계를 설정할 때 다른 테이블의 열과 연결을 하는 참조의 대상이 주로 기본 키이다. 다른 테이블의 기본키를 참조한 열은 외래 키가 되기 때문에 기본키는 그만큼 중요한 기능을 한다.<br>테이블을 생성 할 때는 특별한 이유가 있지 않는 한 기본키와 외래 키를 사용해 테이블 사이의 관계를 설정하는 것이 기본 정의 방법이다.

#### ER 다이어그램

객체 관계도, 또는 테이블 관계도라고 다르게 표현할 수 있다. 건축물의 설계도와 같은 것이다. 배관은 어떻게 지나가고, 문과 창문은 어디에 배치하는 것과 같은 건축 설계도의 구조처럼 그림으로 표현된 테이블의 구조도라고 할 수 있다.<BR>테이블 사이의 관계를 정의할 때 어떤 관계를 가지는지에 따라 다음과 같이 3가지로 나뉜다.

##### 일대일 관계

테이블 두 개가 일대일로 매칭 되는 관계이다. 상품 재고 테이블과 상품 정보 테이블의 관계를 대표적으로 들 수 있다. 상품 코드 한 개에 대해서 재고 정보도 한 개만 있어야 한다. 재고 테이블의 입장에서는 상품 코드 한 개와 유일한 관계를 가지기 때문에 일대일 관계가 된다.

##### 일대다 관계

한 개의 테이블의 여러 테이블과 관계를 맺는 관계이다.<BR>상품 정보와 다른 테이블과의 관계가 일대다 관계이다.<BR>상품 정보 테이블은 재고 정보, 제조사, 원산지 테이블들의 정보를 연결해서 담고 있다.<br>상품 정보 테이블이 여러 테이블과 관계를 맺고 있기 때문에 상품 정보 테이블은 일대다 관계를 가진다.

##### 다대다 관계

주문정보 테이블은 다대다 관계이다. 주문정보 테이블은 상품정보, 회원정보 등의 정보를 참조해서 연결하고 있고, 반대로 SMS와 이메일 발송 테이블은 주문정보 테이블을 참조하고 있다.<BR>이렇게 여러 테이블의 정보를 참조하고, 반대로 다른 테이블에서 참조를 하는 관계가 다대다 관계이다.

이렇게 테이블 사이의 관계를 참조하는 관계를 선으로 이어서 도식화하면 ER 다이어그램이 된다.

예제에 사용한 shop 데이터베이스의 테이블들은 다음처럼 ER 다이어그램으로 표현된다.

![_config.yml]({{ site.baseurl }}/img/study/erdiagram.png)

##### MySQL 워크벤치로 ER 다이어그램 자동 생성하기

MySQL 워크벤치에는 이미 생성되어있는 데이터베이스의 테이블들을 기초로 ER 다이어그램을 자동 생성 해주는 기능이 있다.

![_config.yml]({{ site.baseurl }}/img/study/creatediagram.png)

Database > Reverse Engineer 메뉴를 선택한 후 ER 다이어그램을 생성하고 싶은 데이터베이스를 선택하면 자동으로 ER 다이어그램이 생성된다.

![_config.yml]({{ site.baseurl }}/img/study/creatediagram2.png)

예제로 사용하는 shop 데이터베이스를 리버스 엔지니어(Reverse Engineer)를 이용해서 ER 다이어그램을 생성해보면 테이블들이 어떤 관계를 가지고 서로 연결되어있는지 쉽게 파악할 수 있다.

##### 정규화

정규화는 중복되는 데이터가 없도록 데이터를 나누어 저장하고, 데이터를 저장한 테이블을 기본키와 외래 키로 연결해서 상호 참조가 이루어지는 관계가 되도록 하는 것을 말한다.<br>정규화는 관계형 데이터베이스의 기본 원칙인 중복이 없는 데이터를 유지하기 위한 필수 개념이다. 하나의 데이터는 한 곳에만 위치하며, 한 곳에만 위치한 데이터를 필요로 하는 다른 테이블은 참조를 통해 데이터를 사용한다.<br>데이터 변경은 중복 없이 유일한 데이터를 수정하기만 하면 데이터를 참조하는 다른 테이블들은 기본키로 연결되어 갱신된 데이터를 읽게 된다.<br>원본 데이터가 들어있는 테이블은 각 행을 유일하게 구분할 수 있도록 기본키를 사용한다. 그리고 다른 테이블들은 이 기본키를 가져와 필요로 하는 원본 데이터를 참조하게 된다.

##### 주문 테이블 정규화하기

예제 데이터베이스의 주문정보를 정규화하지 않고 데이터를 하나의 저장한다고 가정하면 다음과 같은 중복이 발생한다.<br>기본키가 없는 상품정보 테이블의 상품명은 다른 테이블에서 여러 상품정보들을 참조할 유일한 방법이 없기 때문에 상품명을 다른 테이블에도 똑같이 저장해야 한다. 주문정보 테이블에도 주문한 상품 이름을 저장하고, 상품 재고 테이블과 상품에 대한 질문과 답변을 저장하는 테이블에도 상품명을 저장해야 한다.<br>여기까지는 상품명을 저장하는 열의 길이만 조금 길어져서 데이터 저장량이 늘어나는 것 말고는 크게 문제가 되지 않는다.<br>그러나 상품정보 테이블의 상품명이 바뀌면 문제가 발생한다. 이 상품명을 사용한 다른 모든 테이블의 상품명을 함께 변경해야 한다. 주문정보, 상품 재고, 상품 Q&A 테이블의 상품명을 UPDATE 쿼리문으로 모두 변경해야 한다. 그래야 상품정보의 정보를 다른 테이블에서 상품명으로 참조를 할 수 있기 때문이다.<BR>테이블에 들어잇는 행 수가 얼마 안 되면 이런 방식이 가능하지만, 몇 십만 개의 행이 있는 테이블에 이런 방식으로 상품명을 업데이트를 하면 데이터베이스의 급격한 성능 저하가 생기며, 데이터 무결성에 문제가 생길 수도 있다.<BR>그래서 기본키를 사용해 테이블에서 유일한 값을 가지는 열을 만들고,다른 테이블은 이 열을 참조하도록 함으로써 상품명이 변경되어도 다른 테이블을 수정할 필요가 없도록 만들 수 있다.<BR>이렇게 중복이 발생하는 데이터를 각각의 테이블에 나누어 저장하고, 기본키와 외래 키를 사용해 연결하면 데이터 중복이 없고 데이터를 수정할 때도 간편하게 관리할 수 있다.

![_config.yml]({{ site.baseurl }}/img/study/normalization.png)

##### 정규화 절차

어떤 중복인지를 파악하고, 중복이 되는 대상을 형태별로 구분한 것을 정규형이라고 한다. 그리고, 정규형에 따라 데이터 중복을 없애기 위해 수행하는 중복 제거 방법을 정규화라고 한다.<BR>정규화는 1차 정규화부터 5차 정규화까지 5가지가 있으며(마찬가지로 정규형도 1차~5차 정규형이 있다.) 보통은 3차 정규화까지만 실행한다.

정규화는 개념이나 사용하는 단어 자체가 어렵기 때문에 처음 데이터베이스 설계를 하면서 적용을 하면 제대로 된 정규화가 안되는 경우가 많다.<BR>한 번에 완전히 정규화된 테이블 구조를 만들려고 하기보다는 먼저 필요한 속성들을 담고 있는 테이블들을 생성한 후 정규화 단계별로 차례차례 정규화를 해서 중복을 제거해나가는 방식으로 개선을 하는 것을 추천한다.

![_config.yml]({{ site.baseurl }}/img/study/normalization2.png)

##### 함수적 종속성

정규화를 설명할 때 등장하는 가장 난해한 용어이다.<BR>정규화는 함수적 종속성을 기초로 해서 실행한다. 함수적 종속성이란 A->B 와 같은 관계 표현에 의해 "A이면 반드시 B이다." 같은 종속적인 관계가 성립하는 것을 말한다. A가 변경되면 B도 반드시 변경되기 때문에 B는 A의 값 변경에 완전히 종속되게 된다.

예를 들어 주문 정보의 회원 ID가 변경되면 주문한 회원과 관련된 모든 정보는 자동으로 변경된다.<BR>주문 정보가 참조하는 회원 ID는 회원 정보를 담고 있는 테이블의 기본키가 되며, 기본키인 회원 ID가 변경되면 그에 종속된 모든 회원 정보(이름, 주소, 연락처 등)도 함께 변경되게 된다.<BR>이때 회원 ID는 A가되고, 나머지 모든 회원 정보는 A에 종속된 B가 된다.<BR>회원 ID가 달라지면 그에 속한 나머지 모든 회원 정보도 자동으로 바꾸게 되기 때문에 B는 A에 종속된 관계에 있게 되고, 이 것을 "함수적으로 종속" 된다고 표현 한다. 

![_config.yml]({{ site.baseurl }}/img/study/normalization3.png)

중복 데이터가 발생하는 테이블을 "함수적 종속성"을 가지도록 기본키를 사용해서 2개 이상의 테이블로 분해를 하는 과정을 제1 정규화라고 한다. 정규화를 한다고 하면 제 1정규화를 가장 먼저 하게 되고, 정규화의 기본 중의 기본이다.<BR>별다른 언급이 없고 정규화를 한다고 하면 기본키를 사용한 테이블 분해 과정을 말한다고 생각하면 쉽다.

제1 정규화의 함수적 종속성은 다른 정규화와 명확하게 구분하기 위해 "완전 함수 종속" 이라고 하기도 한다와 같은 말이다.

여기서 부터 어려워 진다.

앞서 설명한 것 처럼 기본키 1개에 의해서 함수적 종속성이 생기도록 하는 정규화를 제1 정규화라고 한다.

2개 이상의 컬럼을 묶어서 기본키(복합 키라고 한다.)의 역할을 하는 경우, 묶인 복합키 중 1개의 키에만 종속된 컬럼이 있으면 부분 함수 종속성이 발생했다고 하고, 테이블을 분리해서 부분 함수 종속성 문제를 해소해야 한다. 이 과정이 제2 정규화이다.<BR>즉, 복합키로 기본키 역할을 하는 테이블에서만 부분 함수 종속성 문제가 발생하고, 제2 정규화를 하게 된다.

![_config.yml]({{ site.baseurl }}/img/study/normalization4.png)

기본 키(복합 키 포함)를 제외한 일반 컬럼들 사이에 종속성이 발생하면 마찬가지로 이 컬럼들도 별도의 테이블로 분리해야 한다. 이렇게 일반 컬럼들 사이에 종속성이 발생한 것을 "이행 함수 종속성"이 발생했다고 한다.<BR>그리고, 이행 함수 종속성 문제를 해소하는 것을 제3 정규화라고 한다.

![_config.yml]({{ site.baseurl }}/img/study/normalization5.png)

이행 함수 종속성의 해소는 반드시 제1 정규화와 제2 정규화가 완료된 후에 해야 한다. 그렇지 않으면 제3 정규화 이후 제1 정규화, 또는 제2 정규화를 다시 해야 하는 문제가 생길 수 있다. 예외적인 경우가 있지만 무조건 1 -> 2 -> 3의 순서로 진행한다고 생각하면 된다.

정규화를 하는 단계는 제1 정규화 -> 제2 정규화 -> 제3 정규화 순서로 진행하며, 함수적 종속성(기본키) -> 부분 함수 종속성(복합 키) -> 이행 함수 종속성(일반 컬럼 종속성)의 순서로 종속 관계를 구현하는 것이라고 할 수 있다.

정규화에 대해서 하나씩 자세히 알아보겠다.

기본적인 방향은 "한 개의 데이터는 한 곳에만 잇어야 한다."는 원칙에 부합하도록 세분화를 해서 중복을 제거한다는 것을 항상 기억해야 한다.

앞서 설명했던 내용들 중 일부가 반복되어 나오지만, 정규화 단계별 과정은 뒤돌아서면 잊어버리게 되기 때문에 눈에 익숙해질 때까지 봐두는 것이 좋다.

> 정규화 용어 중에 "컬럼" 과 "속성"을 혼합해 사용하는 경우가 잇는데 같은 것이라고 이해하면 된다.<BR>객체를 기준으로 한 논리적인 개념으로는 속성이 되고, 데이터베이스에 실제 구현한 구현체가 컬럼이 된다.

#### 제1 정규화

테이블 안의 속성 값에는 중복된 데이터가 없어야 한다.<BR>하나의 컬럼(속성)에 들어있는 값은 중복이 없어야 한다.<BR>제1 정규화는 컬럼 방향을 정규화와 행 방향의 정규화 2개로 나눌 수 있다.

예를 들어 주문 정보 테이블의 주문자 정보 컬럼이 "홍길동, 010-1234-5678, email@domain.com"처럼 여러 개의 정보가 들어가 있다면, 이 컬럼은 "주문자명", "연락처", "이메일", "주소" 4개의 컬럼으로 분리해서 하나의 컬럼에 하나의 데이터만 들어가도록 개선을 해야 한다.<br>이 과정이 제1 정규화의 1단계이며, 컬럼의 개수를 늘려나가면서 1개의 컬럼에는 1개의 데이터만 들어가도록 중복을 제거한다.

![_config.yml]({{ site.baseurl }}/img/study/normalization6.png)

제1 정규화의 2단계는 행 단위로 중복되는 데이터들을 2개 이상의 테이블로 나누고 기본키를 사용해 서로 연결을 해주는 단계이다. 한 번에 여러 개의 상품을 주문할 수 있는 주문 정보를 한 개의 테이블에 저장할 경우, 주문한 상품에 대한 정보는 각각 다르지만 주문에 대한 정보는 여러 행에 걸쳐 중복이 나타나게 된다.<br>이런 중복을 제거하려면 공통된 주문 정보를 담는 테이블과 주문 상품 정보를 담는 테이블 2개로 컬럼들을 분리한 후 주문 코드를 기본키로 해서 연결하면 주문 정보에 대한 중복이 사라지고 함수적 종속성이 생기게 된다.

![_config.yml]({{ site.baseurl }}/img/study/normalization7.png)

나누어진 테이블들 중 주문 정보를 담은 테이블도 중복이 발생하므로 함수적 종속성을 가지도록 분해를 해야 한다.<br>예를 들어 한 사용자가 여러 번의 주문을 하면 주문 정보에 주문자 정보에 대한 중복이 발생하는 행들이 생기게 된다.<br>앞서 아래와 같은 방법으로 주문 정보 테이블에서 회원 정보만을 별도의 테이블로 분리하고 회원 아이디를 기본키로 해서 회원 정보와 주문 정보 테이블을 연결하면 주문자 정보에 대한 중복을 제거 할 수 있다.

![_config.yml]({{ site.baseurl }}/img/study/normalization8.png)

대부분의 정규화 과정은 제1 정규형만 만족해도 충분히 중복 데이터 없는 데이터 구조를 만들 수 있다. 그만큼 중요하고, 또 반드시 시행해야  하는 정규화 과정이다.

#### 제2 정규화

기본키가 복합 키(2개 이상의 컬럼으로 하나의 행을 식별)로 구성된 경우, 모든 속성은 이 복합 키에만 의존적이어야 한다.<br>복합 키에 의존적이지 않고, 복합 키를 구성하는 하나의 컬럼에만 의존적인 컬럼이 있으면, 이 속성은 별도의 테이블로 분리하거나, 복합키에 의존적이도록 변경해야 한다.<br>이렇게 복합키에 의존적이지 않은 속성이 있으면 이 테이블은 2차 정규화 대상이 된다.

주문 정보를 저장하는 주문 테이블에 주문 코드로 식별하는 여러 개의 주문이 있고, 한 주문자가 여러 개의 상품을 주문했다면, 하나의 주문 코드로 여러 행의 주문 목록이 등록된다.<br>따라서 각각의 주문 행은 주문 코드+상품 코드로 구성되는 복합키로만 식별할 수 있게 된다.

![_config.yml]({{ site.baseurl }}/img/study/normalization9.png)

이때 주문행에 상품명이 저장된 컬림이 있을 경우, 이 상품명 컬럼은 복합 키에 의존적인 것이 아니라 상품코드에만 의존적이 된다. 주문 코드 없이 상품 코드로만 상품명이 식별되기 때문에 해당 정보는 중복 정보가 되고, 상품정보 테이블(또는 주문 상품의 상품명을 저장하는 테이블)을 추가해 상품코드로 식별되는 상품명을 가지도록 분리해야 한다.

#### 제3 정규화

제3 정규화는 기본키 이외의 일반 컬럼들끼리 의존성이 있는지를 확인해서 분리를 하는 것이다.<br>테이블의 속성(컬럼)들이 기본키에 의존적이지 않고, 일반 컬럼들끼리만 의존적인 관계가 생성될 경우, 3차 정규화 대상이 된다.

판매 대리점의 발주 정보를 담는 주문 정보 테이블은 기본 키, 또는 복합 키에 의해서 주문행이 식별된다.<br>그리고 관리되는 지점 주소록 목록에서 배송 주소를 선택하는 방식으로 주문 시스템 기능이 구현되어 있지만, 주문 정보 테이블에 지점 코드와 배송 주소 정보를 모두 저장했다면, 이 주문 테이블은 제 3 정규화의 대상이 된다.<br>지점 코드에 의해서 이미 배송 주소 정보를 확인할 수 있지만, 실제 배송 주소를 저장한 주문 정보의 배송 주소 컬럼은 배송지 식별 코드 컬럼에 의존적인 중복 정보가 된다.

![_config.yml]({{ site.baseurl }}/img/study/normalization10.png)

따라서, 주문 정보 테이블의 기본키, 또는 복합 키가 아니면서 배송지 식별 코드 컬럼에 의존적인 배송 주소 컬럼을 별도의 테이블로 분리하거나 삭제해서 이행 함수 종속성을 해소해야 한다.

너무 당연할 것 같지만, 복잡한 데이터베이스를 설계하다 보면 이런 이행 함수 종속성 문제는 생각보다 빈번하게 발생한다.

#### 만능이 아닌 정규화, 그리고 반정규화

정규화는 관계형 데이터베이스에서 테이블에 들어있는 데이터의 무결성을 보장하는 가장 중요한 기본 원칙이지만, 절대적으로 중복이 없도록 100% 완벽한 정규화를 해서 유지해야만 하는 것은 아니다. 정규화가 세밀하게 될수록 테이블 개수는 더 많아지고 더 많은 테이블은 원하는 쿼리 결과를 얻기 위해 더 많은 테이블을 조인해야만 하는 것을 뜻한다.<br>그만큼 쿼리문이 복잡해지고 테이블의 관계 파악도 더 어려워진다.

정규화를 세밀하게 하려면 테이블 정의서와 ER 다이어그램을 꼼꼼하게 작성해야 하고, 이 문서를 기초로 잘 설계된 데이터 베이스를 구축해야 한다.<br>당연히 이후의 테이블 변경 내역도 테이블 정의서와 ER 다이어그램에 지속적으로 반영을 해야 한다. 그래야 정규화된 데이터베이스를 유지할 수 있고, 정규화된 각 테이블의 용도 또한 파악하기가 쉽다.<br>물론 관리와 유지보수에 더 많은 노력이 들어간다는 뜻이다.

세밀한 정규화가 항상 장점만 있는 것은 아니다.<br>데이터베이스의 복잡도가 증가하는 것과 더불어 테이블 구조 변경이나 테이블 확장에 많은 제약이 생기기 때문에 초기에 설계과 잘못된 경우, 데이터베이스 구조를 확장하는데 여러 가지 문제가 발생하기도 한다.<br>다량의 데이터가 이미 들어가 있는 데이터베이스의 경우, 기본 키와 여러 개의 외래 키로 참조가 되는 복잡한 관계는 구조적으로 테이블 삭제가 어렵기 때문에 설계가 잘못되었다고 바로 다시 만들기가 어렵다.<br>구조를 바꾸지 않고 추가의 컬럼들을 자꾸 추가하게 되면 테이블의 복잡도가 더 증가하게 되고, 데이터 무결성 보장도 힘들어진다.

경우에 따라서는 정규화에 충실할수록 테이블 복잡도가 증가하면서 조인 과정이 복잡해지고, 쿼리 결과를 얻는데 더 오랜 시간이 걸리는 역효과가 발생하기도 한다. 이럴 때는 데이터의 무결성 원칙을 일부 훼손하지만 쿼리 편의성과 쿼리 속도를 올리기 위해 중복 데이터를 허용하는 "반정규화"를 하기도 한다.

데이터베이스의 정규화는 기본 중의 기본이지만, 이 기본을 지키기 힘든 경우가 있다.<br>상품별 월간 판매 실적 내용을 확인하는데 10~20초씩 기다려야 한다면, 판매 실적을 확인하는 것은 아마도 큰 마음을 먹고 한 번씩 실행해야 하는 두려운 조회 작업이 될 것이다.

반정규화를 하는 가장 큰 목적은 쿼리 성능의 향상이다.<br>단, 반정규화를 할 때는 정규화에 충실하면서 예외적으로 성능 향상을 위해 필요한 경우에만 수행해야 한다.<br>반정규화를 남발하게 되면, 데이터 무결성의 원칙이 사라지고, 결국에는 관리할 수 없는 데이터의 덩어리가 되게 된다.

반정규화는 다음과 같은 경우에 수행한다.

- 조회가 빈번한 원격 테이블, 또는 타 데이터베이스의 테이블 접근 속도를 높이기 위해 중복 데이터를 담은 추가 테이블을 로컬 데이터베이스에 생성(일종의 캐싱)
- 요약, 집계 데이터 자주 얻어야  하는 경우, 계산 결과를 미리 담아놓는 테이블, 컬럼을 추가하거나, 요약 집계에 필요한 컬럼들을 모아서 별도의 테이블을 생성
- 빈번하게 자주 조회하는 컬럼 데이터들만을 모아놓은 테이블을 추가해서 많은 테이블 조인이 발생하지 않도록 함
- 매번 조인이 발생하는 정규화된 테이블을 하나로 병합해 불필요한 테이블 조인을 단순화
- 중복 컬럼이나 원래의 컬럼 데이터를 가공한 컬럼을 추가해서 컬럼 가공 시간과 연산이 필요한 함수 사용을 줄임

여러가지 복잡한 예를 들었지만 요약하자면 반복해서 자주 가져오는 컬럼 데이터들을 미리 가공하거나 따로 모아서 빠르게 데이터를 선택 할 수 있도록 정규화 된 데이터베이스에서 예외적으로 허용하는 최적화 방법이다.

반정규화 대상에 따라 다음과 같이 구분할 수도 있다.

- 테이블 단위 - 테이블 추가(중복, 통계, 이력, 부분 테이블), 분할, 병합(조인이 빈번한 테이블 병합)
- 컬럼 단위 - 중복, 파생, 이력 컬럼 추가

반정규화를 하기 전 데이터를 빠르게 가져올 수 있는 다른 최적화 방법이 있다면 그 방법을 사용하는 것이 더 좋다. 어쨌든 반정규화는 데이터의 무결성을 해치는 최적화 방식이기 때문에 데이터의 중복을 피할 수 없기 때문이다.<br>데이터의 무결성을 유지하면서 데이터를 가져오는 최적화 방법으로는 인덱스 클러스터링(Clustering)과 파티셔닝(Partitioning) 같은 방법들이 있다.