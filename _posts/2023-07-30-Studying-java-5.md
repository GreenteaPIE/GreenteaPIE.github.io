---
layout: post
title: JAVA 용어 정리 - 5
date: 2023-07-30
excerpt: "자바 용어 정리 - Scanner 와 BufferedReader"
tags: [java, study]
comments: true


---

#### Scanner란?

Scanner 클래스는 입력받은 데이터(바이트)를 다양한 타입으로 변환하여 반환하는 클래스이다. 간단하게 기본형과 String 타입을 정규표현식을 사용해 파싱(parse)할 수 있다.

##### Scanner의 특징

- java.util 패키지에 속한다. (java.util.Scanner)
- 공백(띄어쓰기) 및 개행(줄 바꿈)을 기준으로 읽는다.(' ', '\t', '\r', '\n' 등)
- 원하는 타입으로 읽을 수 있다.
- 버퍼의 사이즈가 1024byte(1KB) 이다.
- UnChecked(Runtime) Exception으로 별도로 예외 처리를 명시 할 필요가 없다.
- Thread unsafe 성질을 지니기에 멀티스레드 환경에서 문제가 발생 할 수 있다.
- 데이터를 입력받을 경우 즉시 사용자에게 전송되며 입력받을 때 마다 전송되어야 하기에 많은 시간이 소요된다.

#### BufferedReader란?

데이터를 한번에 읽어와 버퍼에 보관한 후 버퍼에서 데이터를 읽어오는 방식으로 동작하는 클래스이다. 즉 사용자가 입력한 문자 스트림을 읽는 것(read) 라고 한다.

여기에서 `버퍼(buffer)`란 데이터를 한 곳에서 다른 한 곳으로 전송하는 동안 일시적으로 해당 데이터를 보관하는 임시 메모리 영역이다. 주로 입출력 속도 향상을 위해 버퍼를 사용한다.

자바에서 버퍼를 BufferedReader와 BufferedWriter라는 클래스를 제공하여 다룰 수 있다.

##### BufferedReader의 특징

- java.io 패키지에 속한다. (import java.io.BufferedReader)
- 데이터를 파싱하지 않고 String 으로만 읽고 가져온다.
- 버퍼의 사이즈가 8192byte(8KB)이다.
- Checked Exception으로 반드시 예외 처리를 명시해야한다.(I/O Exception을 throw 하거나 try/catch 해야한다.)
- Thread safe 성질을 지니기에 멀티스레드 환경에서도 안전하다.
- 버퍼가 가득차거나 개행문자가 나타나면 버퍼의 내용을 한번에 프로그램으로 전달하기에 Scanner보다 소요되는 시간을 절약할 수 있다.

##### 간단하게 Scanner의 작성법을 알아보자.

```java
import java.util.Scanner;
...
Scanner sc = new Scanner(System.in);
String st = sc.nextLine();
```

위와 같이 System.in을 통해 Scanner 객체를 생성한다.

`System.in` 이란 사용자로부터 입력을 받기 위한 입력 스트림이다. Scanner 클래스뿐 아니라 다른 입력 클래스들도 System.in을 통해 사용자 입력을 받아야 한다.

##### 다음으로 BufferedReader 작성법도 알아보자.

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.io.IOException;
...
public static void main(String[] args) throws IOException{
  BufferedReader br = new BufferedReader(InputStreamReader(System.in));
    String st = br.readLine();
    
    int a = Integer.parseInt(br.readLine());
    int b = Integer.paresInt(st);
}
```

위와 같이 BufferedReader는 매개변수로 InputStreamReader를 사용하여 객체를 생성한다.

`InputStreamReader`란 문자 기반의 스트림으로써 바이트 기반 스트림을 문자 기반 스트림으로 연결시켜 주는 역할을 한다.

또한 패키지의 경우 java.io 하위의 BufferedReader, InputStreamReader 와 예외처리를 위한 IOException 를 사용하며 모두 간단히 java.io.* 로 모두 포함 할 수 있다.

#### Scanner와 BufferedReader의 차이점

본인은 Scanner 는 Parse로써 사용자가 입력한 텍스트를 token 단위로 잘라 특정한 형태로 반환하는 것 이며 BufferedReader는 Read로 사용자가 입력한 데이터 자체를 그대로 읽어들이는 것 이라고 이해했다.

1. Scanner는 BufferedRear보다 타입에 구애받지 않는다.
   - BufferedReader는 String 형식으로만 읽고 저장하기에 형변환을 위한 추가적인 코드 작성이 불가피한 반면에 Scanner는 원하는 타입으로 읽고 파싱할 수 있다.
   - Scanner의 경우 int, long, short, float, double의 경우 nextInt(), nextShort(), nextFloat(), nextDouble()과 같은 함수들을 사용 할 수 있다.
   - BufferedReader는 readLine() 함수만을 사용한다.
2. BufferedReader는 Scanner 보다 효율적인 메모리 용량을 가진다.
   - BufferedReader의 버퍼 메모리가 8KB 로 Scanner의 버퍼 메모리 1KB보다 크기에 많은 입력이 있을 경우 더 효율적이다.
   - 다만, BufferedReader의 경우 일단 큰 메모리를 잡아먹게 된다.
3. BufferedReader는 Scanner보다 안전하다.
   - Scanner는 Thread-unsafe 하기에 멀티스레드 환경에서 안전하지 않지만 BufferedReader는 안전하다.
   - 스레드 간 Scanner는 공유할 수 없지만 BufferedReader는 공유 할 수 있다.
   - 동기화를 지원하는 BufferedReader는 싱글스레드인 Scanner보다 약간 느린데, Scanner의 경우 정규식을 사용하여 입력을 받으므로 BufferedReader가 문자열을 더욱 빠르게 입력받을 수 있다.
4. BufferedReader가 Scanner보다 실행 속도가 빠르다.
   - 왜 빠를까? 여기선 CPU를 고려해야 하는데 문자가 입력 될 때마다 CPU가 하나하나 입출력을 하는 것 보다 버퍼에 어느정도 쌓아두고 가득차거나 개행이 일어날 때 마다 입출력을 처리하는 것이 훨씬 효율적이다.
