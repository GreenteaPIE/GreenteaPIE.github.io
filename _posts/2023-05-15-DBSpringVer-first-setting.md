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

### 2.  web.xml에서 한글 처리

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

한글 처리를 위해 web.xml에 위 코드를 추가한다.


## [프로젝트 주소](https://github.com/GreenteaPIE/TeamProjectDBSpringVer)
