---
layout: post
title: JAVA 용어 정리 - 13
date: 2023-08-08
excerpt: "자바 용어 정리 - 접근 제한자 public, protected, default, private"
tags: [java, study]
comments: true


---



접근 제한자는 글자 그대로 접근을 제한하기 위해 사용한다.<br>접근이라는 것은 클래스, 인터페이스, 멤버 등에 대한 접근을 의미한다.

접근 제한자로는 public, protected, default, private 4가지 종류가 있다.

public : 외부 클래스가 자유롭게 접근이 가능하다.<br>protected : 같은 패키지이거나 자식 클래스에서 접근이 가능하다.<br>default : 같은 패키지에 소속된 클래스에서만 접근이 가능하다.<br>private : 선언한 클래스 내부에서만 접근이 가능하다.

즉 접근의 개방 정도는 public > protected > default > private 순으로 열려있다고 보면 된다.<br>여기서 default 접근 제한은 public, proctect, private 을 모두 생략한 상태가 default 이다.

```java
public class A{
    public int a;
    protected int b;
    private int c;
    int d;
}
```

접근 제한자는 클래스, 인터페이스, 그리고 클래스나 인터페이스에 속하는 멤버에 사용된다.

#### 클래스의 접근 제한자

```java
public class A{}
class B{}
```

다음과 같이 public, default 접근 제한자로 나뉜다.

public 으로 선언한 클래스는 어떤 패키지이던  간에 사용할 수 있다.<br>default 으로 선언한 클래스는 다른 패키지일 때 사용할 수 없다.

A, B 클래스를 ex1 패키지에 생성 했고, ex2 패키지에서 두 클래스를 사용하고자 한다면,

```java
package ex2;

import ex1.*;

public class C{
    A a; // <- public 이므로 o
    B b; // <- default 이므로 x
}
```

A 클래스는 public 접근 제한자를 가지므로, 사용할 수 있지만 B 클래스는 default 접근 제한자의 성질에 의해 사용할 수 없다.

#### 생성자의 접근 제한자

```java
public class C{
    public C(...){
        ...
    }
    protected C(...){
        ...
    }
    private C(...){
        ...
    }
    C(...){
        ...
    } // Default 접근 제한에 해당한다.
}
```

생성자는 다음과 같이 모든 접근 제한자를 가질 수 있다.

public 으로 선언한 생성자는 어떤 패키지 이던 간에 사용 할 수 있다.<br>protected는 다른 패키지에서 생성자 호출을 제한하지만, 다른 패키지의 클래스가 해당 클래스의 자식 클래스이면 생성자 호출이 가능하다.<br>default는 접근 제한자 표기를 생략한 형태로 다른 패키지에서의 생성자 호출을 제한한다.<br>private는 해당 클래스 내부에서만 생성자를 호출하도록 하므로, private로 선언된 생성자를 해당 클래스의 외부에서 이용하려면 해당 클래스 내부에서 생성자를 이용해 객체를 만들고 메서드를 이용해 객체를 받는 방법을 이용해야 한다.

private를 이용한 싱글톤에 대한 예시는 다음과 같다.

> 싱글톤(Singleton) 패턴의 정의는 단순하다. 객체의 인스턴스가 오직 1개만 생성되는 패턴을 의미한다. 
>
> 최초 한번의 new 연산자를 통해서 고정된 메모리 영역을 사용하기 때문에 추후 해당 객체에 접근할 때 메모리 낭비를 방지 할 수 있고, 싱글톤 인스턴스가 전역으로 사용되는 인스턴스이기 때문에 다른 클래스의 인스턴스들이 접근하여 사용할 수 있다.

```java
public class ABC{
    ABC object1 = new ABC();
    
    private ABC(){
        
    }
    static ABC getInstance(){
        return object1;
    }
}

ABC var1 = ABC.getInstance();
ABC var2 = ABC.getInstance();
```

#### 멤버(필드, 메서드)의 접근 제한자

생성자의 예시와 동일하게 사용할 수 있다.

public으로 선언한 멤버는 어떤 패키지이던 간에 사용할 수 있다.<br>protected는 다른 패키지에서의 멤버 호출을 제한하지만, 다른 패키지의 클래스가 해당 클래스의 자식 클래스이면 생성자 호출이 가능하다.<br>default는 접근 제한자 표기를 생략한 형태로 다른 패키지에서의 멤버 호출을 모두 제한한다.<br>private는 오직 해당 클래스 내부에서만 사용하도록 제한한다.

다음과 같은 네 가지 접근 제한자를 가지고 클래스, 인터페이스, 멤버를 선언할 때 해당 필드와 메서드에 대한 접근을 어디까지 허용하거나 제한할 것인지를 결정하고 그에 맞는 접근 제한자를 사용해야한다.
