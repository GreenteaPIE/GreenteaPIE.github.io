---
layout: post
title: DB Spring 초기설정
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

### 2.  web.xml에서 한글 처리

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

이 설정을 사용하면 프로젝트에 필요한 의존성을 쉽체 추가하고 관리할 수 있다.

### 5. Dependency(의존성) 설정




## [프로젝트 주소](https://github.com/GreenteaPIE/TeamProjectDBSpringVer)
