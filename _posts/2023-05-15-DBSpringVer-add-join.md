---
layout: post
title: 3-DB Spring 회원가입
date: 2023-05-15
excerpt: "팀 프로젝트 DB Spring Version"
tags: [project, java, jsp, Oracle, css, HTML, BootStrap, API, JQuery, JavaScript, Spring, FrameWork]
feature: /img/SpringDB/logo.png
comments: true



---


> **사용한 플랫폼 : Spring, Oracle**

### 1. com.db.model에 UserVO 추가

```
package com.db.model;

import java.sql.Timestamp;

import lombok.Data;

@Data
public class UserVO {

	private String userid, pass, name, email, address1, address2, address3, phone;
	private int gender, point, grade;
	private Timestamp enter;

}
```

lombok의 @Data 어노테이션을 이용하여 getter 와 setter, to String 설정을 해준다.


### 2.  join.jsp

header.jsp 에 회원가입 클릭 시 이동될 join(회원가입).jsp 를 만들어준다. 

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<head>
<meta charset="UTF-8">
<script src="https://code.jquery.com/jquery-3.4.1.js" integrity="sha256-WpOohJOqMqqyKL9FccASB9O0KwACQJpFTUBLTYOVvVU=" crossorigin="anonymous">
	
</script>
<script src="//t1.daumcdn.net/mapjsapi/bundle/postcode/prod/postcode.v2.js"></script>
<%@ include file="../header.jsp"%>
</head>
<body>
	<hr>
	<div class="frame user-frm">
		<article class="card-body" style="max-width: 700px; margin: auto;">
			<!-- 회원가입 form태그 시작 -->
			<form id="join_form" method="post">
				<span class="final_name_ck">이름을 입력해주세요.</span>
				<div class="form-group input-group fg-x700">
					<div class="input-group-prepend" style="height: 38px">
						<span class="input-group-text">
							<i class="fa fa-user"></i>
						</span>
					</div>
					<input name="name" class="form-control user_input" placeholder="이름 입력" type="text" required />
					<div class="btn-group" date-toggle="buttons">
						<label class="btn  active" style="background-color: #D9D9D9;"> <input type="radio" name="gender" autocomplete="off" value="1" checked>남자
						</label> <label class="btn " style="color: #737272; background-color: #D9D9D9;"> <input type="radio" name="gender" autocomplete="off" value="2" checked>여자
						</label>
					</div>
				</div>
				<!-- form-group// -->
				<span class="final_id_ck">아이디를 입력해주세요.</span>
				<div class="form-group input-group fg-x700">
					<div class="input-group-prepend">
						<span class="input-group-text">
							<i class="fa fa-user"></i>
						</span>
					</div>
					<input id="userid" name="userid" class="form-control id_input" placeholder="ID 입력" type="text" required />
				</div>
				<span class="id_input_re_1">사용 가능한 아이디 입니다.</span>
				<span class="id_input_re_2">아이디가 이미 존재합니다.</span>
				<!-- form-group// -->
				<span class="final_mail_ck">메일을 입력해주세요.</span>
				<div class="form-group input-group fg-x700">
					<div class="input-group-prepend">
						<span class="input-group-text">
							<i class="fa fa-envelope"></i>
						</span>
					</div>
					<input id="email" name="email" class="form-control mail_input" placeholder="Email 입력" type="email" required />
				</div>
				<span class="mail_input_box_warn"></span>
				<div class="mail_check_wrap">
					<div class="form-group input-group fg-x700">
						<input class="mail_check_input form-control mail_check_input_box" id="mail_check_input_box_false" disabled="disabled">
						<input type="button" class="btn btn-primary mail_check_button" value="인증번호 전송">
					</div>
					<div class="clearfix"></div>
					<span id="mail_check_input_box_warn"></span>
				</div>
				<!-- form-group// -->
				<div class="form-group input-group fg-x700">
					<div class="input-group-prepend">
						<span class="input-group-text">
							<i class="fa fa-phone"></i>
						</span>
					</div>
					<input id="phone" name="phone" class="form-control" placeholder="휴대폰번호 입력('-' 제외)" type="text" required />
				</div>
				<!-- form-group// -->
				<span class="final_addr_ck">상세 주소를 입력해주세요.</span>
				<div class="form-group input-group fg-x700">
					<div class="input-group-prepend">
						<span class="input-group-text">
							<i class="fa fa-building"></i>
						</span>
					</div>
					<input type="text" id="address1" class="form-control address_input_1" name="address1" placeholder="우편번호" readonly="readonly" />
					<br />
					<div class="form-group input-group fg-x700">
						<input type="text" id="address2" class="form-control address_input_2" name="address2" placeholder="주소" readonly="readonly" />
					</div>
					<br />
					<input type="text" id="address3" class="form-control address_input_3" name="address3" placeholder="상세주소" />
					<br />
					<input type="button" onclick="execution_daum_address()" class="rbutton xsmall white btn btn-primary" value="우편번호 찾기">
					<br>
				</div>
				<!-- form-group end.// -->
				<span class="final_pw_ck">비밀번호를 입력해주세요.</span>
				<span class="final_pwck_ck">비밀번호를 확인해주세요.</span>
				<div class="form-group input-group fg-x700">
					<div class="input-group-prepend">
						<span class="input-group-text">
							<i class="fa fa-lock"></i>
						</span>
					</div>
					<input id="pass" name="pass" class="form-control pw_input" placeholder="비밀번호 입력" type="password" required>
					<input id="passCheck" name="" class="form-control pwck_input" placeholder="비밀번호 확인" type="password" required>
				</div>
				<span class="pwck_input_re_1">비밀번호가 일치합니다.</span>
				<span class="pwck_input_re_2">비밀번호가 일치하지 않습니다.</span>
				<!-- form-group// -->
				<div class="fg-x700 form-group">
					<input type="button" class="btn btn-primary btn-block join_button" value="가입하기">
				</div>
			</form>
		</article>
	</div>
	<hr>
	<!-- card.// -->
	<!--container end.//-->
</body>
<%@ include file="../footer.jsp"%>
</html>

```

![_config.yml]({{ site.baseurl }}/img/SpringDB/join.png)

### 2.  com.db.controller에 UserController 추가

회원가입 페이지로 이동하기 위한 GET Mapping 을 추가한다.

```java
package com.db.controller;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
@RequestMapping(value = "/user")
public class UserController {
	private static final Logger logger = LoggerFactory.getLogger(UserController.class);

	// 회원가입 페이지 이동
	@GetMapping("join")
	public void joinGET() {

		logger.info("회원가입 페이지 진입");

	}
}
```




## [프로젝트 주소](https://github.com/GreenteaPIE/TeamProjectDBSpringVer)
