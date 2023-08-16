---
layout: post
title: JAVA 용어 정리 - 18
date: 2023-08-16
excerpt: "자바 용어 정리 - 변수와 메서드, 데이터 타입(기본형, 참조)"
tags: [java, study]
comments: true


---

#### 변수?

데이터를 저장하기 위해 메모리에 공간을 만들어 할당하고, 이름을 부여한 

![_config.yml]({{ site.baseurl }}/img/study/variable.png)

##### 클래스 변수(Class variables)

- 클래스에서 선언된 변수
- static 이 붙으면 클래스 변수, 클래스 메서드가 된다.(아무것도 없으면 인스턴스 변수, 인스턴스 메서드가 됨)
- 클래스가 로딩될 때 클래서 변수가 생성되고 JVM에 의해 클래스가 메모리에 올라가면 초기화 됨. 클래스 변수는 언제라도 바로 사용할 수 있고 프로그램 종료까지 유지된다.
- 모든 인스턴스가 공통된 값을 공유할 때, 인스턴스를 생성할 필요가 없는 값을 저장할때 사용한다.
- JVM 메모리의 method 영역에 올라간다.

##### 인스턴스 변수(Instance variables)

- 클래스에서 선언된 변수
- 클래스의 인스턴스를 생성할 때 만들어진다. 따라서 인스턴스 변수 값을 읽거나 저장하기 전에는 먼저 인스턴스를 생성해야한다.
- 인스턴스마다 고유한 상태를 유지해야할 경우 인스턴스 변수로 선언
- JVM 메모리의 heap 영역에 올라간다.

##### 지역 변수(Local varialbes)

- 메서드 내에서 선언되어 메서드 내부에서만 사용 가능. 메서드가 종료되면 사용할 수 없다.
- JVM 메모리의 stack 영역에 올라간다.

#### 메서드?

변수가 물체의 상태라면 메서드는 물체의 행동에 해당한다.

![_config.yml]({{ site.baseurl }}/img/study/variable2.png)

#### 클래스 메서드(Class methods)

- static 이 붙은 메서드

##### 인스턴스 메서드(Instance methods)

- static 이 붙지않은 메서드

##### 데이터타입

데이터타입은 운영체제에 따라 밤위와 크기가 조금씩 다르다.

###### 기본/원시형 자료형(primitive data type)

> 변수에 변수의 값이 저장됨.

기본형 타입 변수인 i와 c에는 각각 15, "a"의 변수 값이 저장됨.

- primitive parameter -> 변수의 값을 읽기만 가능. 단순히 저장된 값만 얻음

![_config.yml]({{ site.baseurl }}/img/study/variable3.png)

| 종류 | 데이터 타입 | 크기  |                        범위                        |
| :--: | :---------: | :---: | :------------------------------------------------: |
| 문자 |    char     | 2Byte |                     0 ~ 65,535                     |
| 정수 |    byte     | 1Byte |                     -128 - 127                     |
|      |    short    | 2Byte |                  -32,768 ~ 32,767                  |
|      |     int     | 4Byte |           -2,147,438,648 ~ 2,147,438,647           |
|      |    long     | 8Byte | -9,223,372,036,854,775,808 ~ 9,223,036,854,775,807 |
| 실수 |    float    | 4Byte |             1.4X10^(-45) ~ 3.4X10^(38)             |
|      |   double    | 8Byte |              4.9X10^324 ~ 1.8X10^308               |
| 논리 |   boolean   | 1Byte |                     true/false                     |

###### 참조형 자료형(reference data type)

> 변수에 변수 값이 할당된 주소가 저장됨.

s에는 "hello"라는 string 객체가 할당된 메모리 주소 값인 1004가 저장됨. 그래서 s라는 참조형 변수를 통해 "hello"라는 string 객체를 참조하는 형태가 됨.

- reference parameter -> 변수의 값을 읽고, 변경이 가능. 값이 저장된 곳의 주소를 얻음

![_config.yml]({{ site.baseurl }}/img/study/variable4.png)

|   자료형   | 설명                                                         |
| :--------: | ------------------------------------------------------------ |
|   class    | 오브젝트에 대한 설계를 담고 있다.                            |
|   array    | 여러 같은 자료형의 데이터를 정적인 크기로 저장하는 자료구조를 제공한다. |
| annotation | 특정 속성 정보(metadata)를 프로그램 요소에 적용하기 위한 방법으로 제공한다. |
| interface  | class의 일종이지만, 일반적인 class와는 달리 메서드의 정의만을 담고 있으며, interface를 상속받는 class에서 해당 메서드들을 구현한다. |
|    enum    | 특수한 형태의 class로, enum 안에 있는 요소들은 해당 enum 타입의 구현체이다. |

##### 데이터 타입과 메모리 할당

```java
public static void main(String[] args){
    int i1 = 5;
    int i2 = i1;
    
    String s1 = "hello";
    String s2 = s1;
    
    s2 = "bye";
}
```

![_config.yml]({{ site.baseurl }}/img/study/variable5.png)

![_config.yml]({{ site.baseurl }}/img/study/variable6.png)

![_config.yml]({{ site.baseurl }}/img/study/variable7.png)

![_config.yml]({{ site.baseurl }}/img/study/variable8.png)

###### wrapper class

> 기본 자료형(primitive type)을 객체로 다루기 위해 사용하는 클래스.
>
> 모든 기본 자료형은 자신만의 wrapper class를 가지고 있다.

|     primitive 자로형      |           wrapper 클래스            |
| :-----------------------: | :---------------------------------: |
|     산술 연산이 가능      | unboxing 하지 않으면 산술 연산 불가 |
| null로 초기화 할 수 없다. |         null로 초기화 가능          |

###### boxing / unboxing

![_config.yml]({{ site.baseurl }}/img/study/variable9.png)

```java
// to int i from Integer ii
int i = ii.intValue();

// to Integer ii from int i
Integer ii = new Integer( i );
```

자바에서는 거의 대부분의 경우에 자동으로 boxing/unboxing을 해준다.

```java
public class Wrapper_EX{
    public static void main(String[] args){
        Integer num = 17; // 자동 박싱
        int n = num; // 자동 언박싱
        System.out.println(n);
    }
}
```

