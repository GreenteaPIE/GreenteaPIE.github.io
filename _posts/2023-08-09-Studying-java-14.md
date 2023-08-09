---
layout: post
title: JAVA 용어 정리 - 14
date: 2023-08-09
excerpt: "자바 용어 정리 - static, final, static final"
tags: [java, study]
comments: true


---

#### static

static은 "고정된" 이라는 의미<br>객체 생성 없이 사용할 수 있는 필드와 메서드를 생성하고자 할 때 활용한다.

필드나 메서드를 객체마다 다르게 가져야 한다면 인스턴스로 생성하면 되고 공용 데이터에 해당하거나 인스턴스 필드를 포함하지 않는 메서드를 선언하고자 할 때 이용한다.

사용하기 위해선 클래스 내에서 필드나 메서드 선언 시 static 키워드를 붙여주기만 하면 된다.

```java
public class PlusClass{
    static int field1 = 15;
    
    static int plusMethod(int x, int y){
        return x+y;
    }
}
```

다음과 같이 선언하고

```java
int ans1 = PlusClass.plusMethod(15,2);
int ans2 = PlusClass.field1 + 2;
```

다음과 같이 바로 "클래스이름.필드" 로 사용할 수 있다.

물론, 아래와 같이 객체 참조 변수를 이용할 수 있지만 추천되진 않는다. 

```java
PlusClass pc = new PlusClass();
int ans1 = pc.plusMethod(15,2);
```

또한, 정적 메소드는 객체 참조 없이 바로 사용할 수 있는 특징 때문에 인스턴스 필드나 메서드, 그리고 this 키워드를 사용할 수 없다.

```java
public class PlussClass{
    static int field1 = 15;
    int field2;
    
    void method1(){}
    static void method2(){}
    static int plusMethod(int x, int y){
        this.field2 = 10; // <- x
        this.method1(); // <- x
        field1 = 10; // <- o
        method2(); // <- o
    }
}
```

간단하게 말하면, 인스턴스 성질은 객체 생성 후 사용할 수 있으므로 객체 참조 없이 사용하는 정적 메서드에는 사용할 수 없는 것이다.

#### final

final은 "최종적인" 이라는 의미<br>즉 해당 변수는 값이 저장되면 최종적인 값이 되므로, 수정이 불가능하다는 의미이다.

final 필드에 값을 저장하는 방법은 다음 두 가지가 있다.

```java
public class Shop{
    
    final int closeTime = 21;
    final int openTime;
    
    public Shop(int openTime){
        this.openTime = openTime;
    }
}
```

하나는 closeTime과 같이 선언과 동시에 값을 주는 방법이 있고 openTime과 같이 생성하고, 객체를 생성할 때 생성자 public Shop에 의해 값을 주는 방법이 있다.

모든 가게가 오픈 시간은 자유롭지만 한 번 정한 오픈 시간을 바꿀 수 없고, 21시에는 모두 닫아야 할 때 위와 같이 final 키워드를 이용하면, 오픈 시간은 객체마다 다르게 설정이 가능하지만, 닫는 시간은 고정되도록 코딩이 가능하다.

#### static final

static + final = "고정된 + 최종적인" 의 의미로, 상수를 선언하고자 할 때 사용된다.<br>final 만으로 충분히 상수가 되지 않느냐? 라는 질문에는 위에서 충분히 답이 됐을 거라고 본다.

상수란, fixed로 변하지 않는 값을 뜻하는데 위의 예시에서, closeTime은 21로 변하지 않지만, openTime은 객체마다 다를 수 있음을 보였으므로 final 자체만으로는 상수를 의미할 수 없다.

상수의 선언 방법 또한 간단하다.

```java
static final double PI = 3.141592;
```

static + final로 선언된 PI 값은 3.141592 라는 불변의 값을 가진다. <br>해당 값은 객체마다 저장될 필요가 없으며(static의 성질) + 여러 값을 가질 수 없다.(final의 특징)

정리하면, 다음과 같다.

|     종류     | 설명                                                         |
| :----------: | ------------------------------------------------------------ |
|    static    | 객체마다 가질 필요가 없는 공용으로 사용하는 필드 혹은 인스턴스 필드를 포함하지 않는 메서드 |
|    final     | 한 번 값이 정해지고 나면 값을 바꿀 수 없는 필드              |
| static final | 모든 영역에서 고정된 값으로 사용하는 상수                    |

