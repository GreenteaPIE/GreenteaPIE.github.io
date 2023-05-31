---
layout: post
title: DB Spring -1 초기설정
date: 2023-05-15
excerpt: "팀 프로젝트 DB Spring Version"
tags: [project, java, jsp, Oracle, css, HTML, BootStrap, API, JQuery, JavaScript, Spring, FrameWork]
feature: /img/SpringDB/logo.png
comments: true



---


> **사용한 플랫폼 : Spring, Oracle**



### 1.  Spring MVC Project 생성

![_config.yml]({{ site.baseurl }}/img/SpringDB/createProject.png)

![_config.yml]({{ site.baseurl }}/img/SpringDB/createProject2.png)

### 2.  web.xml에서 한글 처리 및 파일 업로드 설정

한글 처리를 위해 web.xml에 아래 코드를 추가한다.

```html
<!-- 한글설정 -->

	<filter>

		<filter-name>encodingFilter</filter-name>

		<filter-class>

			org.springframework.web.filter.CharacterEncodingFilter

		</filter-class>

		<init-param>

			<param-name>encoding</param-name>

			<param-value>UTF-8</param-value>

		</init-param>

		<init-param>

			<param-name>forceEncoding</param-name>

			<param-value>true</param-value>

		</init-param>

	</filter>

	<filter-mapping>

		<filter-name>encodingFilter</filter-name>

		<url-pattern>/*</url-pattern>

	</filter-mapping>

	<!-- 한글설정 END -->
```

파일 업로드를 위해 appServlet 에 아래 코드를 추가한다.

```xml
<!-- 업로드 관련 설정 -->
      <multipart-config>
         <location>C:\\upload\\temp</location>      
         <max-file-size>20971520</max-file-size>               <!-- 1MB * 20 -->
         <max-request-size>41943040</max-request-size>         <!-- 40MB -->
         <file-size-threshold>20971520</file-size-threshold>      <!-- 20MB -->
      </multipart-config>
```

### 3.  Java 버전 변경 및 Tomcat server module 주소 변경

![_config.yml]({{ site.baseurl }}/img/SpringDB/javaset.png)

해당 url 경로를 추후 작업들의 편의를 위해서 "controller"를 제거 해준다.

![_config.yml]({{ site.baseurl }}/img/SpringDB/moduleset.png)

### 4. pom.xml 설정

Java 설정 등을 이용하기 위해 `pom.xml` 에 있는 기존의 servlet 버전을 아래와 같이 변경해준다.

```html
	<!-- Servlet -->
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>javax.servlet-api</artifactId>
			<version>3.1.0</version>
			<scope>provided</scope>
		</dependency>
```

 `maven-compiler-plugin`설정을 자바 버전에 맞춰준다.

```xml
             <plugin> 
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-compiler-plugin</artifactId>
				<version>2.5.1</version>
				<configuration>
					<source>11</source>
					<target>11</target>
					<compilerArgument>-Xlint:all</compilerArgument>
					<showWarnings>true</showWarnings>
					<showDeprecation>true</showDeprecation>
				</configuration>
			</plugin>
```

이 설정은 모든 경고 및 사용 중단되어 있는 메서드에 대해서 표시된다.

프로젝트에서 사용되는 라이브러리와 관련된 정보를 설정하는 부분의 자바 버전을 동일하게 맞춰준다.

```xml
<modelVersion>4.0.0</modelVersion>
	<groupId>com.db</groupId>
	<artifactId>controller</artifactId>
	<name>DBSpringVersion</name>
	<packaging>war</packaging>
	<version>1.0.0-BUILD-SNAPSHOT</version>
<!--이 밑에 넣어줌-->
<properties>
		<java-version>11</java-version>
		<org.springframework-version>5.2.8.RELEASE</org.springframework-version>
		<org.aspectj-version>1.6.10</org.aspectj-version>
		<org.slf4j-version>1.6.6</org.slf4j-version>
	</properties>
```

이 설정을 사용하면 프로젝트에 필요한 의존성을 쉽게 추가하고 관리할 수 있다.

### 5. Dependency(의존성) 설정

```xml
<!-- https://mvnrepository.com/artifact/org.springframework.security/spring-security-core -->
		<dependency>
			<groupId>org.springframework.security</groupId>
			<artifactId>spring-security-core</artifactId>
			<version>${org.springframework-version}</version>
		</dependency>

		<!-- https://mvnrepository.com/artifact/org.springframework.security/spring-security-web -->
		<dependency>
			<groupId>org.springframework.security</groupId>
			<artifactId>spring-security-web</artifactId>
			<version>${org.springframework-version}</version>
		</dependency>

		<!-- https://mvnrepository.com/artifact/org.springframework.security/spring-security-config -->
		<dependency>
			<groupId>org.springframework.security</groupId>
			<artifactId>spring-security-config</artifactId>
			<version>${org.springframework-version}</version>
		</dependency>
```

Spring Security를 사용하여 보안을 구성하고 관리 할 수 있다.

```xml
<!-- https://mvnrepository.com/artifact/com.sun.mail/javax.mail -->
		<dependency>
			<groupId>com.sun.mail</groupId>
			<artifactId>javax.mail</artifactId>
			<version>1.6.2</version>
		</dependency>

		<!-- https://mvnrepository.com/artifact/javax.mail/javax.mail-api -->
		<dependency>
			<groupId>javax.mail</groupId>
			<artifactId>javax.mail-api</artifactId>
			<version>1.6.2</version>
		</dependency>
```

이메일 처리와 관련된 기능을 제공하는 라이브러리이다. <br>메일 인증 기능을 위해 추가한다.

```xml
<!-- https://mvnrepository.com/artifact/org.springframework/spring-context-support -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-context-support</artifactId>
			<version>${org.springframework-version}</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-context</artifactId>
			<version>${org.springframework-version}</version>
			<exclusions>
				<!-- Exclude Commons Logging in favor of SLF4j -->
				<exclusion>
					<groupId>commons-logging</groupId>
					<artifactId>commons-logging</artifactId>
				</exclusion>
			</exclusions>
		</dependency>
```

Spring 프레임워크의 핵심 기능을 제공한다.<br>컨텍스트 지원은 스프링 애플리케이션 컨텍스트 및 의존성 주입을 사용할 수 있도록 한다.

```xml
<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-webmvc</artifactId>
			<version>${org.springframework-version}</version>
		</dependency>
```

웹 애플리케이션을 구축하기 위한 Spring 프레임워크의 단일 페이지 애플리케이션 모듈을 사용할 수 있도록 한다.

```xml
<!-- AspectJ -->
		<dependency>
			<groupId>org.aspectj</groupId>
			<artifactId>aspectjrt</artifactId>
			<version>${org.aspectj-version}</version>
		</dependency>
```

Aspect-Oriented Programming (AOP)을 지원하는 도구이다.<br>이를 통해 애플리케이션의 관점이 구현 가능하다.

```xml
<!-- Logging -->
		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>slf4j-api</artifactId>
			<version>${org.slf4j-version}</version>
		</dependency>
		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>jcl-over-slf4j</artifactId>
			<version>${org.slf4j-version}</version>
			<scope>runtime</scope>
		</dependency>
		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>slf4j-log4j12</artifactId>
			<version>${org.slf4j-version}</version>
			<scope>runtime</scope>
		</dependency>
		<dependency>
			<groupId>log4j</groupId>
			<artifactId>log4j</artifactId>
			<version>1.2.17</version>
		</dependency>
```

`SLF4J`및 `Log4j`를 사용한 로깅 관련 라이브러리이다.

```xml
	<!-- @Inject -->
		<dependency>
			<groupId>javax.inject</groupId>
			<artifactId>javax.inject</artifactId>
			<version>1</version>
		</dependency>
```

`@Inject` 어노테이션은 객체를 생성하고 초기화하는데 필요한 의존성을 주입하는데 사용된다.

```xml
<!-- Servlet -->
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>javax.servlet-api</artifactId>
			<version>3.1.0</version>
			<scope>provided</scope>
		</dependency>
		<dependency>
			<groupId>javax.servlet.jsp</groupId>
			<artifactId>jsp-api</artifactId>
			<version>2.1</version>
			<scope>provided</scope>
		</dependency>
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>jstl</artifactId>
			<version>1.2</version>
		</dependency>

```

자바 기반 웹 애플리케이션을 개발하기 위한 Servlet API를 제공한다.

```xml
<!-- Test -->
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>4.12</version>
			<!-- <scope>test</scope> -->
		</dependency>
```

`JUnit`은 Java 애플리케이션에서 단위 테스트를 작성하고 실행하기 위한 가장 널리 사용되는 테스트 프레임워크 중 하나이다.

```xml
<!-- https://mvnrepository.com/artifact/org.springframework/spring-test -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-test</artifactId>
			<version>${org.springframework-version}</version>
			<scope>test</scope>
		</dependency>
```

`JUnit` 테스트 클래스를 위한 수많은 테스트 유틸리티와 어노테이션을 제공한다.

```xml
<!-- https://mvnrepository.com/artifact/com.oracle.database.jdbc/ojdbc10 -->
		<dependency>
			<groupId>com.oracle.database.jdbc</groupId>
			<artifactId>ojdbc10</artifactId>
			<version>19.17.0.0</version>
		</dependency>
```

오라클 데이터베이스와 Java 응용 프로그램을 연결하는데 사용되는 JDBC 드라이버이다.

```xml
		<!-- https://mvnrepository.com/artifact/org.projectlombok/lombok -->
		<dependency>
			<groupId>org.projectlombok</groupId>
			<artifactId>lombok</artifactId>
			<version>1.18.24</version>
			<scope>provided</scope>
		</dependency>

```

어노테이션을 사용하여 생성자, getter, setter 같은 반복적인 코드를 자동으로 생성해주므로 개발자의 작업량을 줄여준다.

```xml
<!-- https://mvnrepository.com/artifact/com.zaxxer/HikariCP -->
		<dependency>
			<groupId>com.zaxxer</groupId>
			<artifactId>HikariCP</artifactId>
			<version>4.0.3</version>
		</dependency>
```

고성능 JDBC 커넥션 풀 라이브러리로, 데이터베이스에 대한 커넥션 풀링을 담당한다.

```xml
<!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
		<dependency>
			<groupId>org.mybatis</groupId>
			<artifactId>mybatis</artifactId>
			<version>3.5.6</version>
		</dependency>
```

Java의 SQL 매핑 프레임워크로, XML 혹은 어노테이션을 사용해 Java 객체와 SQL 쿼리를 매핑한다.

```xml
<!-- https://mvnrepository.com/artifact/org.mybatis/mybatis-spring -->
		<dependency>
			<groupId>org.mybatis</groupId>
			<artifactId>mybatis-spring</artifactId>
			<version>2.0.6</version>
		</dependency>
```

 MyBatis와 Spring 프레임워크를 직접 통합하여 사용할 수 있는 기능을 제공하는 라이브러리이다. <br>MyBatis의 기능을 스프링에서 사용하기 용이해진다.

```xml
<!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-jdbc</artifactId>
			<version>${org.springframework-version}</version>
		</dependency>
```

스프링 프레임워크의 JDBC 지원 기능을 제공한다.

```xml
<!-- https://mvnrepository.com/artifact/org.springframework/spring-tx -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-tx</artifactId>
			<version>${org.springframework-version}</version>
		</dependency>
```

스프링 프레임워크의 트랜잭션 관리 기능을 제공하는 라이브러리이다.

```xml
<!-- https://mvnrepository.com/artifact/org.bgee.log4jdbc-log4j2/log4jdbc-log4j2-jdbc4 -->
		<dependency>
			<groupId>org.bgee.log4jdbc-log4j2</groupId>
			<artifactId>log4jdbc-log4j2-jdbc4</artifactId>
			<version>1.16</version>
		</dependency>
```

log4jdbc-log4j2-jdbc4는 로깅을 위한 라이브러리로, JDBC에 대한 모든 호출을 자동으로 로깅해준다.

### 6. 필수 패키지 추가 및 설정

src/main/java 경로에 3개의 패키지를 추가한다.

- com.db.model : VO(value Object) 패키지, 데이터 타입을 저장한다.
- com.db.mapper : DAO(Data Access Obejt) 역할을 하는 패키지, 데이터베이스 접속하는 역할을 한다.
- com.db.service : Service 패키지, Mapper와 Controller 사이를 연결해주는 역할을 한다.

스프링에서 각 패키지를 인식 할 수 있도록 `root-context.xml` 파일 설정을 변경해준다.

먼저 하단의 `namespaces`를 클릭하여 "context" 와 "mybatis-spring"을 체크하고 저장한 후,<br>아래 코드를 추가한다.

```xml
<mybatis-spring:scan base-package="com.db.mapper" />
	<context:component-scan
		base-package="com.db.model"></context:component-scan>
	<context:component-scan
		base-package="com.db.service"></context:component-scan>
```

기타 설정을 위한 Bean 들을 추가한다.

```xml
<!-- Root Context: defines shared resources visible to all other web components -->
	<bean id="hikariConfig" class="com.zaxxer.hikari.HikariConfig">
		<property name="driverClassName"
			value="net.sf.log4jdbc.sql.jdbcapi.DriverSpy" />
		<property name="jdbcUrl"
			value="jdbc:log4jdbc:oracle:thin:@localhost:1521:XE" />
		<property name="username" value="scott" />
		<property name="password" value="tiger" />
	</bean>

	<bean id="datasource" class="com.zaxxer.hikari.HikariDataSource">
		<constructor-arg ref="hikariConfig" />
	</bean>

	<bean id="sqlSessionFactory"
		class="org.mybatis.spring.SqlSessionFactoryBean">
		<property name="dataSource" ref="datasource"></property>
		<property name="mapperLocations" value="classpath*:com/db/mapper/*Mapper.xml"></property>
	</bean>
```

이 구성을 사용하면 HikariCP를 이용한 데이터베이스 연결 풀, MyBatis를 이용한 SQL 매핑 및 세션 생성 등의 기능을 프로젝트에서 사용할 수 있다.

```xml
<!-- googlemail설정 -->
	<bean id="mailSender" class="org.springframework.mail.javamail.JavaMailSenderImpl"> 
 <property name="host" value="smtp.gmail.com"/> <!-- 메이서버 호스트 -->
 <property name="port" value="587"/> <!-- 메이서버 포트번호 -->
 <property name="username" value="dbad010101@gmail.com"/> <!-- 자신의 이메일 아이디 -->
 <property name="password" value="ztimhpwzrpnyaphd"/> <!-- 자신의 비밀번호 -->
   <!-- 보안연결 SSL과 관련된 설정 -->
 <property name="javaMailProperties">
  <props>
  <prop key="mail.smtp.auth">true</prop>
  <prop key="mail.smtp.starttls.enable">true</prop>
  </props>
 </property>
</bean>
```

회원가입시 이메일 인증을 위한 설정이다.

src/main/webapp/WEB-INF/spring/appServlet 경로에 `servlet-context.xml` 에서 css, img, js 를 사용 하기 위한 mapping 설정을 추가 한다.

```xml
<resources mapping="/css/**" location="/resources/css/" />
	<resources mapping="/img/**" location="/resources/img/" />
	<resources mapping="/js/**" location="/resources/js/" />
```

보안을 위해 유저 비밀번호를 인코딩해주는 'security-context.xml' 를 추가 한다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

<bean id="bcryptPasswordEncoder" class="org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder"></bean>

</beans>

```

### 7. Mapper XML 저장 경로 생성

동적 SQL문을 사용할 xml 파일을 저장할 경로를 생성 해준다.

src/main/resources 경로에 mapper 패키지명(com.db.mapper)과 동일한 폴더 경로를 생성한다.

추가로 log4jdbc 라이브러리의 로깅 구현을 결정하는 설정 `log4jdbc.log4j2.properties`을  추가한다.

```xml
log4jdbc.spylogdelegator.name=net.sf.log4jdbc.log.slf4j.Slf4jSpyLogDelegator
```

![_config.yml]({{ site.baseurl }}/img/SpringDB/resourcesset.png)

이로써 기본적인 설정이 완료되었다.


## [프로젝트 주소](https://github.com/GreenteaPIE/TeamProjectDBSpringVer)
