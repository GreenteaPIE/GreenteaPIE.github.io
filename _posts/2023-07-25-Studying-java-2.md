---
layout: post
title: JAVA 용어 정리 - 2
date: 2023-07-25
excerpt: "자바 용어 정리 - 오류(Error)와 예외(Exception)"
tags: [java, study]
comments: true



---

#### 컴파일 오류와 런타임 오류

프로그램을 실행하다가 보면 어떠한 원인 때문에 비정상적인 동작을 일으켜 프로그램이 종료되는 상황을 자주 볼 수 있다. 이를 우리는 프로그램에 오류가 발생 했다고 말한다. 에러의 종류로는 컴파일 할 때 발생할 수 있는 컴파일 오류와 실행 중에 발생하는 런타임 오류 두 종류가 있다.

#### 컴파일 오류

컴파일 오류는 소스코드를 .class 파일을 컴파일하는 과정에서 JVM이 던지는 오류로, 대부분 소스코드 자체의 문법적 오류로 인해 발생하는 경우가 대부분이며, 프로그램 자체에서 처리 할 수 있는 방법은 없다.

컴파일 오류의 예로는 `ClassNotFoundException`, `IllegalAccessException`, `NoSuchMethodException` 등이 있다.

#### 런타임 오류

런타임 오류는 문법적인 오류가 없어 컴파일 시에는 정상적으로 컴파일 됐지만 프로그램을 실행하는 과정에서 발생하는 오류를 말한다. 이는 개발자가 직접 오류를 확인하여 처리해야 한다.

런타임 오류의 예로는 `NullPointerException`, `ArithmeticException`, `IndexOutOfBoundsException` 등이 있다.

#### 오류와 예외

자바에서는 오류를 오류(Error)와 예외(Exception)으로 나눈다. 오류는 시스템이 종료되어야 할 수준과 같이 수습할 수 없는 심각한 문제를 의미한다.

이는 개발자가 미리 예측하여 방지 할 수 없다. 반면 예외는 개발자가 구현한 로직에서 발생한 실수나 사용자의 영향에 의해 발생한다. 이는 개발자가 미리 예측하여 방지 할 수 있기 때문에 상황에 맞는 예외 처리(Exception Handle)를 해야 한다.

오류와 예외 모두 Object 클래스를 상속 받는 `Throwable` 클래스를 상속 받는다. Throwable 객체는 오류나 예외에 대한 메시지를 담고, 예외가 연결될 때 연결된 예외의 정보를 기록한다. 이를 위해 Throwable 클래스에는 `getMessage()` 와 `printStackTrace()` 함수가 구현되어 있다. 

#### 오류(Error)

몇가지 대표적인 오류에 대해 알아보자.

`StackOverflowError` : 호출의 깊이가 깊어지거나 재귀가 지속되어 stack overflow 발생 시 던져지는 오류이다.<br>`OutOfMemoryError` : JVM이 할당한 메모리의 부족으로 더 이상 객체를 할당할 수 없을 때 던져지는 오류이다. 이는 Garbage Collector에 의해 추가적인 메모리가 확보되지 못하는 상황이기도 하다.

#### 예외(Exception)

몇 가지 대표적인 예외에 대해 알아보자.

`NullPointerException` : 객체가 필요한 경우에 null을 사용하려고 시도할 경우 던져지는/던져질 수 있는 예외이다.<br>`IllegalArgumentException` : 메서드가 허가되지 않거나 부적절한 argument를 받았을 경우에 던져지는/던져질 수 있는 예외이다.

<u>예외는 오류와 다르게 개발자가 임의로 예외를 던질 수 있다.</u>

예외는 Runtime Exception 과 이외의 Exception으로 나뉘어져 있다. 이둘의 차이를 알아보자.

#### Checked Exception 과 Unchecked Exception

Exception은 Checked Exception과 Unchecked Exception으로 나눌 수 있는데, 간단하게 RuntimeException을 상속하는 클래스를 Unchecked Exception, 상속하지 않는 클래스를 Checked Exception으로 분류할 수 있다. RuntimeException 또한 Exception의 일종이지만, 명시적으로 예외 처리를 하지 않아도 되기 때문에 이를 특별하게 취급한다.

|     구분      |           Checked Exception            |             Unchecked Exception             |
| :-----------: | :------------------------------------: | :-----------------------------------------: |
|   확인 시점   |          컴파일(Compile) 시점          |             런타임(Runtime) 시              |
|   처리 여부   |        반드시 예외 처리해야 함         |          명시적으로 하지 않아도 됨          |
| 트랜잭션 처리 | 예외 발생 시 롤백(Rollback) 하지 않음  |     예외 발생 시 롤백(Rollback) 해야 함     |
|     종류      | IOException, ClassNotFoundException 등 | NullPointerException, ClassCastException 등 |

#### 예외 처리 방법

예외를 처리하는 방법에는 예외 복구, 예외 회피, 예외 전환이 있다.

#### 예외 복구

예외 복구는 예외 상황을 파악하고 문제를 해결하여 정상 상태로 돌려놓는 방법 이다. 예외를 잡아서 일정 시간이나 조건만큼 대기하고 재시도를 반복한다. 최대 재시도 횟수를 넘기게 될 경우에 예외를 발생한다.

```java
final int MAX_RETRY = 100;
    int maxRetry = MAX_RETRY;
    while(maxRetry > 0) {
        try {
            ...
        } catch(SomeException e) {
            // 로그 출력. 정해진 시간만큼 대기한다.
        } finally {
            // 리소스 반납 및 정리 작업
        }
    }
    // 최대 재시도 횟수를 넘기면 직접 예외를 발생시킨다.
    throw new RetryFailedException();
```

#### 예외 회피

예외 회피는 예외 처리를 직접 담당하지 않고 호출한 메서드로 위임해 회피하는 방법이다.

예외 처리의 필요성이 있다면 어느 정도는  처리하고 위임하는 것이 좋다.

```java
// 예시 1
public void add() throws SQLException {
    // ...생략
}

// 예시 2 
public void add() throws SQLException {
    try {
        // ... 생략
    } catch(SQLException e) {
        // 로그를 출력하고 다시 날린다!
        throw e;
    }
}
```

#### 예외 전환

예외 회피와 비슷하게 메서드 밖으로 예외를 위임하지만, 그냥 위임하지 않고 적절한 예외로 전환해서 넘기는 방법이다. 조금 더 명확한 의미를 전달 하기 위해 적합한 의미를 가지는 예외로 변경 한다.

예외 처리를 단순하게 만들기 위해 포장(wrap)할 수도 있다.

 

```java
// 조금 더 명확한 예외로 던진다.
public void add(User user) throws DuplicateUserIdException, SQLException {
    try {
        // ...생략
    } catch(SQLException e) {
        if(e.getErrorCode() == MysqlErrorNumbers.ER_DUP_ENTRY) {
            throw DuplicateUserIdException();
        }
        else throw e;
    }
}

// 예외를 단순하게 포장한다.
public void someMethod() {
    try {
        // ...생략
    }
    catch(NamingException ne) {
        throw new EJBException(ne);
        }
    catch(SQLException se) {
        throw new EJBException(se);
        }
    catch(RemoteException re) {
        throw new EJBException(re);
        }
}
```

