---
layout: post
title: 3 - DB Spring 회원가입
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

join.jsp 이미지

![_config.yml]({{ site.baseurl }}/img/SpringDB/join.png)

### 2.  com.db.controller 에 UserController 추가

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

### 3. 아이디 중복검사 기능 추가

#### join.jsp 에 스크립트 추가

```js
//아이디 중복검사
		$('.id_input').on(
				"propertychange change keyup paste input",
				function() {
					//alert("ddd");
					//console.log("keyup 테스트");   

					var userid = $('.id_input').val(); // .id_input에 입력되는 값
					var data = {
						userid : userid
					} // '컨트롤에 넘길 데이터 이름' : '데이터(.id_input에 입력되는 값)'

					$.ajax({
						type : "post",
						url : "/user/userIdChk",
						data : data,
						success : function(result) {
							// console.log("성공 여부" + result);
							// console.log("성공 여부" + result);
							if (result != 'fail') {
								$('.id_input_re_1').css("display",
										"inline-block");
								$('.id_input_re_2').css("display", "none");
								idckCheck = true;
							} else {
								$('.id_input_re_2').css("display",
										"inline-block");
								$('.id_input_re_1').css("display", "none");
								idckCheck = false;
							}
						}// success 종료      
					}); // ajax 종료   
				});// function 종료
```

#### UserController.java 에 메서드 추가

```java
// 아이디 중복 검사
	@PostMapping("/userIdChk")
	@ResponseBody
	public String userIdChkPOST(String userid) throws Exception {

		logger.info("userIdChk() 진입");

		int result = userService.idCheck(userid);

		logger.info("결과값 = " + result);

		if (result != 0) {
			return "fail"; // 중복 아이디가 존재
		} else {
			return "success"; // 중복 아이디 x
		}
	} // userIdChkPOST()종료
```

#### UserMapper.java 인터페이스 생성 후 추가

```java
package com.db.mapper;

public interface UserMapper {

	// 아이디 중복 검사
	public int idCheck(String userid);

}
```

#### UserService.java 인터페이스 생성 후 추가

```java
package com.db.service;

public interface UserService {
	
	// 아이디 중복 검사
	public int idCheck(String userid) throws Exception;
}
```

#### UserServiceImpl 생성 후 추가

```java
package com.db.service;

import org.springframework.beans.factory.annotation.Autowired;

import com.db.mapper.UserMapper;

public class UserServiceImpl implements UserService{

	@Autowired
	UserMapper usermapper;
	
	@Override
	public int idCheck(String userid) throws Exception {
		
		return usermapper.idCheck(userid);
	}

}
```

UserMapper를 @Autowired 어노테이션으로 의존성 주입 대상으로 지정해준다.

#### UserMapper.xml 생성 후 추가

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
  <mapper namespace="com.db.mapper.UserMapper">
  
  <!-- 아이디 중복검사 -->
	<select id="idCheck" resultType="int">
		select count(*) from shopuser
		where userid = #{userid}
	</select>
  
  </mapper>
```

### 4. 이메일 인증 기능 추가

#### join.jsp 에 스크립트 추가

```js
		/* 인증번호 이메일 전송 */
		$(".mail_check_button").click(function() {
			var email = $(".mail_input").val(); // 입력한 이메일
			var checkBox = $(".mail_check_input"); // 인증번호 입력란
			var boxWrap = $(".mail_check_input_box"); // 인증번호 입력란 박스
			var warnMsg = $(".mail_input_box_warn"); // 이메일 입력 경고글

			/* 이메일 형식 유효성 검사 */
			if (mailFormCheck(email)) {
				warnMsg.html("메일이 전송 되었습니다. 메일을 확인해주세요.");
				warnMsg.css("display", "inline-block");
			} else {
				warnMsg.html("올바르지 못한 메일 형식입니다.");
				warnMsg.css("display", "inline-block");
				return false;
			}

			$.ajax({
				type : "GET",
				url : "mailCheck?email=" + email,
				success : function(data) {
					//console.log("data : " + data);
					checkBox.attr("disabled", false);
					boxWrap.attr("id", "mail_check_input_box_true");
					code = data;
				}
			});
		});
```

#### UserController 에 추가

```java
@Autowired
	private UserService userService;
@Autowired
	private JavaMailSender mailSender;

// 회원가입 이메일 인증
	@GetMapping("/mailCheck")
	@ResponseBody
	public String mailCheckGET(String email) throws Exception {
		logger.info("이메일 데이터 전송 확인");
		logger.info("인증 메일 : " + email);

		// 인증번호 생성 (6자리)
		Random random = new Random();
		int checkNum = random.nextInt(888888) + 111111;
		logger.info("인증 번호 : " + checkNum);

		// 이메일 보내기
		String setFrom = "dbad010101@gmail.com";
		String toMail = email;
		String title = "회원가입 인증 메일 입니다.";
		String content = "홈페이지를 방문해주셔서 감사합니다." + "<br><br>" + "인증 번호는 " + checkNum + "입니다." + "<br>"
				+ "해당 인증번호를 인증번호 확인란에 기입하여 주세요.";
		try {
			MimeMessage mail = mailSender.createMimeMessage();
			MimeMessageHelper mailHelper = new MimeMessageHelper(mail, true, "UTF-8");

			mailHelper.setFrom(setFrom);
			mailHelper.setTo(toMail);
			mailHelper.setSubject(title);
			mailHelper.setText(content, true);
			mailSender.send(mail);
		} catch (Exception e) {
			e.printStackTrace();
		}
		String num = Integer.toString(checkNum);
		return num;
	}
```

UserService 를 @Autowired 어노테이션으로 의존성 주입 대상으로 지정해준다.

메일 발송을 위해 JavaMailSender를 @Autowired 해준다.

#### 메일인증 번호 비교 스크립트 추가

```js
// 인증번호 비교 (마우스 커서가 벗어났을때 비교)
		$(".mail_check_input").blur(function() {
			var inputCode = $(".mail_check_input").val(); // 입력코드 
			var checkResult = $("#mail_check_input_box_warn"); // 비교 결과   

			if (inputCode == code) { // 일치할 경우
				checkResult.html("인증번호가 일치합니다.");
				checkResult.attr("class", "correct");
				mailnumCheck = true; // 일치할 경우
			} else { // 일치하지 않을 경우
				checkResult.html("인증번호를 다시 확인해주세요.");
				checkResult.attr("class", "incorrect");
				mailnumCheck = false; // 불일치할 경우
			}
		});

		// 인증번호 비교 (인증번호를 입력하는 동시에 비교)
		$(".mail_check_input").on("input", function() {
			var inputCode = $(".mail_check_input").val(); // 입력코드
			var checkResult = $("#mail_check_input_box_warn"); // 입력결과

			if (inputCode == code) { // 인증번호가 일치할 경우
				checkResult.html("인증번호가 일치합니다.");
				checkResult.attr("class", "correct");
				mailnumCheck = true; // 일치할 경우
			} else { // 인증번호가 불일치할 경우
				checkResult.html("인증번호를 다시 확인해주세요.");
				checkResult.attr("class", "incorrect");
				mailnumCheck = false; // 불일치 할경우
			}
		});
```

#### 주소API 스크립트 추가

```js
// 다음 주소 연동
		function execution_daum_address() {

			new daum.Postcode(
					{
						oncomplete : function(data) {
							// 팝업에서 검색결과 항목을 클릭했을때 실행할 코드를 작성하는 부분.

							// 각 주소의 노출 규칙에 따라 주소를 조합한다.
							// 내려오는 변수가 값이 없는 경우엔 공백('')값을 가지므로, 이를 참고하여 분기 한다.
							var addr = ''; // 주소 변수
							var extraAddr = ''; // 참고항목 변수

							//사용자가 선택한 주소 타입에 따라 해당 주소 값을 가져온다.
							if (data.userSelectedType === 'R') { // 사용자가 도로명 주소를 선택했을 경우
								addr = data.roadAddress;
							} else { // 사용자가 지번 주소를 선택했을 경우(J)
								addr = data.jibunAddress;
							}

							// 사용자가 선택한 주소가 도로명 타입일때 참고항목을 조합한다.
							if (data.userSelectedType === 'R') {
								// 법정동명이 있을 경우 추가한다. (법정리는 제외)
								// 법정동의 경우 마지막 문자가 "동/로/가"로 끝난다.
								if (data.bname !== ''
										&& /[동|로|가]$/g.test(data.bname)) {
									extraAddr += data.bname;
								}
								// 건물명이 있고, 공동주택일 경우 추가한다.
								if (data.buildingName !== ''
										&& data.apartment === 'Y') {
									extraAddr += (extraAddr !== '' ? ', '
											+ data.buildingName
											: data.buildingName);
								}
								// 표시할 참고항목이 있을 경우, 괄호까지 추가한 최종 문자열을 만든다.
								if (extraAddr !== '') {
									extraAddr = ' (' + extraAddr + ')';
								}
								// 조합된 참고항목을 해당 필드에 넣는다.
								// document.getElementById("sample6_extraAddress").value = extraAddr;
								addr += extraAddr;

							} else {
								//document.getElementById("sample6_extraAddress").value = '';
								addr += ' ';
							}

							// 우편번호와 주소 정보를 해당 필드에 넣는다.
							//document.getElementById('sample6_postcode').value = data.zonecode;
							//document.getElementById("sample6_address").value = addr;
							// 커서를 상세주소 필드로 이동한다.
							$(".address_input_1").val(data.zonecode);
							$(".address_input_2").val(addr);

							//  document.getElementById("sample6_detailAddress").focus();

							// 상세주소 입력란 disabled 속성 변경 및 커서를 상세주소 필드로 이동한다.
							$(".address_input_3").attr("readonly", false);
							$(".address_input_3").focus();
						}
					}).open();
		}
```

#### 비밀번호 일치 확인  스크립트 추가

```js
/* 비밀번호 확인 일치 유효성 검사 */
		$('.pwck_input').on("propertychange change keyup paste input",
				function() {

					var pw = $('.pw_input').val();
					var pwck = $('.pwck_input').val();
					$('.final_pwck_ck').css('display', 'none');

					if (pw == pwck) {
						$('.pwck_input_re_1').css('display', 'block');
						$('.pwck_input_re_2').css('display', 'none');
						pwckcorCheck = true;
					} else {
						$('.pwck_input_re_1').css('display', 'none');
						$('.pwck_input_re_2').css('display', 'block');
						pwckcorCheck = false;
					}
				});
```

#### 이메일 형식 유효성 검사 스크립트 추가

```js
/* 입력 이메일 형식 유효성 검사 */
		function mailFormCheck(email) {
			var form = /^([\w-]+(?:\.[\w-]+)*)@((?:[\w-]+\.)*\w[\w-]{0,66})\.([a-z]{2,6}(?:\.[a-z]{2})?)$/i;
			return form.test(email);
		}
```

#### 유효성 검사 스크립트 추가

```js
// 이메일전송 인증번호 저장을 위한 코드
		var code = "";

		/* 유효성 검사 통과유무 변수 */
		var idCheck = false; // 아이디
		var idckCheck = false; // 아이디 중복 검사
		var pwCheck = false; // 비번
		var pwckCheck = false; // 비번 확인
		var pwckcorCheck = false; // 비번 확인 일치 확인
		var nameCheck = false; // 이름
		var mailCheck = false; // 이메일
		var mailnumCheck = false; // 이메일 인증번호 확인
		var addressCheck = false; // 주소

		// 회원가입 버튼(회원가입 기능 작동)
		$(document).ready(
				function() {
					$(".join_button")
							.click(
									function() {
										//$("#join_form").attr("action", "/member/join");
										//$("#join_form").submit();

										/* 입력값 변수 */
										var id = $('.id_input').val(); // id 입력란
										var pw = $('.pw_input').val(); // 비밀번호 입력란
										var pwck = $('.pwck_input').val(); // 비밀번호 확인 입력란
										var name = $('.user_input').val(); // 이름 입력란
										var mail = $('.mail_input').val(); // 이메일 입력란
										var addr = $('.address_input_3').val(); // 주소 입력란

										/* 아이디 유효성검사 */
										if (id == "") {
											$('.final_id_ck').css('display',
													'block');
											idCheck = false;
										} else {
											$('.final_id_ck').css('display',
													'none');
											idCheck = true;
										}

										/* 비밀번호 확인 유효성 검사 */
										if (pwck == "") {
											$('.final_pwck_ck').css('display',
													'block');
											pwckCheck = false;
										} else {
											$('.final_pwck_ck').css('display',
													'none');
											pwckCheck = true;
										}

										/* 이름 유효성 검사 */
										if (name == "") {
											$('.final_name_ck').css('display',
													'block');
											nameCheck = false;
										} else {
											$('.final_name_ck').css('display',
													'none');
											nameCheck = true;
										}

										/* 이메일 유효성 검사 */
										if (mail == "") {
											$('.final_mail_ck').css('display',
													'block');
											mailCheck = false;
										} else {
											$('.final_mail_ck').css('display',
													'none');
											mailCheck = true;
										}

										/* 주소 유효성 검사 */
										if (addr == "") {
											$('.final_addr_ck').css('display',
													'block');
											addressCheck = false;
										} else {
											$('.final_addr_ck').css('display',
													'none');
											addressCheck = true;
										}

										/* 최종 유효성 검사 */
										if (idCheck && idckCheck && pwckCheck
												&& pwckcorCheck && nameCheck
												&& mailCheck && mailnumCheck
												&& addressCheck) {
											$("#join_form").attr("action",
													"/user/join");
											$("#join_form").submit();
										}
										return false;
									});
				});
```

### 5. 회원가입 기능 추가

#### UserController.java 에 추가

```java
@Autowired
	private BCryptPasswordEncoder pwEncoder;

// 회원가입
	@PostMapping("join")
	public String joinPost(UserVO user) throws Exception {

		logger.info("join 진입");
		// 비밀번호 인코딩 후
		String rawPw = ""; // 인코딩 전 비밀번호
		String encodePw = ""; // 인코딩 후 비밀번호

		rawPw = user.getPass(); // 비밀번호 데이터 얻음
		encodePw = pwEncoder.encode(rawPw); // 비밀번호 인코딩
		user.setPass(encodePw); // 인코딩된 비밀번호 member객체에 다시 저장

		// 회원가입 쿼리 실행
		userService.userJoin(user);
		logger.info("회원 가입 성공");

		return "/user/welcome";

	}
```

입력한 비밀번호 값을 인코딩하여 DB로 전송해준다.

#### UserMapper.java 에 추가

```java
// 회원가입
	public void userJoin(UserVO user);
```

#### UserService.java 에 추가

```java
// 회원가입
	public void userJoin(UserVO user) throws Exception;
```

#### UserServiceImpl.java 에 추가

```java
@Override
	public void userJoin(UserVO user) throws Exception {

		usermapper.userJoin(user);

	}
```

#### UserMapper.xml 에 추가

```xml
<!-- 회원가입 -->
	<insert id="userJoin">
		insert into shopuser values(#{userid}, #{pass},
		#{name}, #{email}, #{address1},
		#{address2}, #{address3}, #{phone},
		#{gender}, 0, 0, sysdate)
	</insert>
```

#### header.jsp 에 회원가입 버튼 수정

```jsp
<a href="/user/join" class="nav-item nav-link">회원가입</a>
```


## [프로젝트 주소](https://github.com/GreenteaPIE/TeamProjectDBSpringVer)
