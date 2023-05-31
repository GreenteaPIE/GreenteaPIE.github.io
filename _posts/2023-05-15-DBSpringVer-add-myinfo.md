---
layout: post
title: 5 - DB Spring 내 정보 수정과 탈퇴
date: 2023-05-15
excerpt: "팀 프로젝트 DB Spring Version"
tags: [project, java, jsp, Oracle, css, HTML, BootStrap, API, JQuery, JavaScript, Spring, FrameWork]
feature: /img/SpringDB/logo.png
comments: true



---


> **사용한 플랫폼 : Spring, Oracle**



### 1.  myPageCheck.jsp

정보 수정 전 본인 확인을 위한 페이지를 만든다.

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<script src="https://code.jquery.com/jquery-3.4.1.js" integrity="sha256-WpOohJOqMqqyKL9FccASB9O0KwACQJpFTUBLTYOVvVU=" crossorigin="anonymous">
	
</script>
<%@ include file="../header.jsp"%>
</head>
<body>
	<hr>
	<form id="infochk_form" method="post">
		<div class="form-group input-group fg-x700">
			<div class="col-lg-4"></div>
			<div class="col-lg-4">
				<div class="jumbotron" style="padding-top: 20px;">
					<h3 style="text-align: center;">정보 확인</h3>
					<div class="form-group">
						<input type="text" class="form-control id_input" placeholder="아이디" name="userid" value="${user.userid }" maxlength="20" readonly="readonly">
					</div>
					<div class="form-group">
						<input type="password" class="form-control pw_input" placeholder="비밀번호" name="pass" maxlength="20">
					</div>
					<input type="button" class="btn btn-primary form-control info_chk_btn" value="확인"> <input type="reset" class="btn btn-primary form-control" value="취소">
					<hr>
					<c:if test="${result==0}">
						<div class="login_warn">ID 또는 비밀번호를 잘못 입력 하셨습니다.</div>
					</c:if>
				</div>
			</div>
			<div class="col-lg-4"></div>
		</div>
	</form>
	<script>
		/* 로그인 버튼 클릭 */
		$(".info_chk_btn").click(function() {
			//alert("로그인 버튼 작동");

			// 로그인 메서드 서버 요청
			$("#infochk_form").attr("action", "/user/mypagechk");
			$("#infochk_form").submit();
		});
	</script>
	<hr>
</body>
<%@ include file="../footer.jsp"%>
</html>
```

해당 페이지에선 로그인 되어있는 아이디와 입력한 비밀번호가 일치하는지 확인한다.

![_config.yml]({{ site.baseurl }}/img/SpringDB/mycheck.png)

### 2. 본인 확인 기능 추가

#### UserController.java 에 추가

```java
// 내 정보 본인 확인 페이지로 이동
	@GetMapping("mypagechk")
	public String mypagechkGET(HttpServletRequest request, UserVO user) {

		logger.info("mypagecheck 페이지 진입");

		try {

		} catch (Exception e) {
			e.printStackTrace();
		}

		return "/user/myPageCheck";

	}

// 내 정보 수정 본인 확인
	@PostMapping("mypagechk")
	public String mypagechkPOST(HttpServletRequest request, UserVO user, RedirectAttributes rttr) throws Exception {
		HttpSession session = request.getSession();
		System.out.println("mypagecheck 메소드 진입");
		String rawPw = "";
		String encodePw = "";
		UserVO vo = userService.myPageCheck(user);

		// 비밀번호 인코딩
		if (vo != null) { // 아이디&비밀번호 일치
			rawPw = user.getPass(); // 사용자가 제출한 비밀번호
			encodePw = vo.getPass(); // DB에 저장 되어있는 인코딩된 비밀번호
			if (true == pwEncoder.matches(rawPw, encodePw)) {
				vo.setPass("");
				session.setAttribute("user", vo);
				return "/user/myPageInfo";
			} else { // 비밀번호가 일치 하지 않을때
				request.setAttribute("result", 0);
				return "/user/myPageCheck";
			}
		} else { // 일치하는 아이디가 없을때
			request.setAttribute("result", 0);
			return "/user/myPageCheck";
		}
	}
```

로그인과 마찬가지로 DB에 저장되어있는 비밀번호와 입력한 비밀번호를 비교하여 일치 여부를 확인 후 정보 수정 페이지로 이동시킨다.

#### UserMapper.java 에 추가

```java
	// 내 정보 수정 본인 확인
	public UserVO myPageCheck(UserVO user);
```

#### UserService.java 에 추가

```java
    // 내 정보 수정 본인 확인
	public UserVO myPageCheck(UserVO user) throws Exception;
```

#### UserServiceImpl.java 에 추가

```java
	@Override
	public UserVO myPageCheck(UserVO user) throws Exception {

		return usermapper.myPageCheck(user);
	}
```

#### UserMapper.xml 에 추가

```xml
<!-- 내 정보 수정 본인 확인 -->
	<select id="myPageCheck" resultType="com.db.model.UserVO">

		select * from shopuser where
		userid = #{userid}

	</select>
```

#### header.jsp 내 정보 수정 버튼 수정

```jsp
<a href="/user/mypagechk" class="dropdown-item">내 정보 수정</a>
```



### 3. myPageInfo.jsp

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
			<form id="userUpdate_form" method="post">
				<div class="form-group input-group fg-x700">
					<div class="input-group-prepend" style="height: 38px">
						<span class="input-group-text">
							<i class="fa fa-user"></i>
						</span>
					</div>
					<input name="name" class="form-control user_input" placeholder="이름 입력" type="text" value="${user.name }" readonly="readonly">
					<div class="btn-group" date-toggle="buttons">
						<c:if test="${user.gender == 1 }">
							<label class="btn  active" style="background-color: #D9D9D9;"> <input type="radio" name="gender" autocomplete="off" value="1" checked>남자
							</label>
							<label class="btn " style="color: #737272; background-color: #D9D9D9;"> <input type="radio" name="gender" autocomplete="off" value="2">여자
							</label>
						</c:if>
						<c:if test="${user.gender == 2 }">
							<label class="btn  active" style="background-color: #D9D9D9;"> <input type="radio" name="gender" autocomplete="off" value="1">남자
							</label>
							<label class="btn " style="color: #737272; background-color: #D9D9D9;"> <input type="radio" name="gender" autocomplete="off" value="2" checked>여자
							</label>
						</c:if>
					</div>
				</div>
				<!-- form-group// -->
				<div class="form-group input-group fg-x700">
					<div class="input-group-prepend">
						<span class="input-group-text">
							<i class="fa fa-user"></i>
						</span>
					</div>
					<input id="userid" name="userid" class="form-control id_input" placeholder="ID 입력" type="text" value="${user.userid }" readonly="readonly">
				</div>
				<!-- form-group// -->
				<span class="final_mail_ck">메일을 입력해주세요.</span>
				<div class="form-group input-group fg-x700">
					<div class="input-group-prepend">
						<span class="input-group-text">
							<i class="fa fa-envelope"></i>
						</span>
					</div>
					<input id="email" name="email" class="form-control mail_input" placeholder="Email 입력" value="${user.email }" type="email" required />
				</div>
				<span class="mail_input_box_warn"></span>
				<div class="mail_check_wrap">
					<div class="form-group input-group fg-x700">
						<input class="mail_check_input form-control mail_check_input_box" id="mail_check_input_box_false" disabled="disabled"> <input type="button" class="btn btn-primary mail_check_button" value="인증번호 전송">
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
					<input id="phone" name="phone" class="form-control" placeholder="휴대폰번호 입력('-' 제외)" value="${user.phone }" type="text" required />
				</div>
				<!-- form-group// -->
				<span class="final_addr_ck">상세 주소를 입력해주세요.</span>
				<div class="form-group input-group fg-x700">
					<div class="input-group-prepend">
						<span class="input-group-text">
							<i class="fa fa-building"></i>
						</span>
					</div>
					<input type="text" id="address1" class="form-control address_input_1" name="address1" placeholder="우편번호" value="${user.address1 }" readonly="readonly" /> <br />
					<div class="form-group input-group fg-x700">
						<input type="text" id="address2" class="form-control address_input_2" name="address2" placeholder="주소" value="${user.address2 }" readonly="readonly" />
					</div>
					<br /> <input type="text" id="address3" class="form-control address_input_3" name="address3" placeholder="상세주소" value="${user.address3 }" /> <br /> <input type="button" onclick="execution_daum_address()" class="rbutton xsmall white btn btn-primary" value="우편번호 찾기"> <br>
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
					<input id="pass" name="pass" class="form-control pw_input" placeholder="비밀번호 입력" type="password" required> <input id="passCheck" name="" class="form-control pwck_input" placeholder="비밀번호 확인" type="password" required>
				</div>
				<span class="pwck_input_re_1">비밀번호가 일치합니다.</span>
				<span class="pwck_input_re_2">비밀번호가 일치하지 않습니다.</span>
				<!-- form-group// -->
				<div class="fg-x700 form-group">
					<input type="button" class="btn btn-primary btn-block update_button" value="정보 수정">
				</div>
				<!-- form-group// -->
				<p class="text-right">
					<input class="btn" type="button" value="회원 탈퇴" id="user_exit_btn">
				</p>
			</form>
		</article>
	</div>
	<!-- card.// -->
	<!--container end.//-->
	<hr>
	<script>
		// 이메일전송 인증번호 저장을 위한 코드
		var code = "";

		/* 유효성 검사 통과유무 변수 */
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
					$(".update_button")
							.click(
									function() {
										

										/* 입력값 변수 */

										var pw = $('.pw_input').val(); // 비밀번호 입력란
										var pwck = $('.pwck_input').val(); // 비밀번호 확인 입력란
										var name = $('.user_input').val(); // 이름 입력란
										var mail = $('.mail_input').val(); // 이메일 입력란
										var addr = $('.address_input_3').val(); // 주소 입력란

										

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
										if (pwckCheck
												&& pwckcorCheck && nameCheck
												&& mailCheck && mailnumCheck
												&& addressCheck) {
											$("#userUpdate_form").attr("action",
													"/user/userupdate");
											$("#userUpdate_form").submit();
										}
										return false;
									});
				});



		/* 인증번호 이메일 전송 */
		$(".mail_check_button").click(function() {
			var email = $(".mail_input").val(); 		// 입력한 이메일
			var checkBox = $(".mail_check_input"); 		// 인증번호 입력란
			var boxWrap = $(".mail_check_input_box"); 	// 인증번호 입력란 박스
			var warnMsg = $(".mail_input_box_warn");	// 이메일 입력 경고글
			
			/* 이메일 형식 유효성 검사 */
			if(mailFormCheck(email)){
				warnMsg.html("메일이 전송 되었습니다. 메일을 확인해주세요.");
				warnMsg.css("display", "inline-block");
			}else{
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
		
		/* 입력 이메일 형식 유효성 검사 */
		function mailFormCheck(email){
			var form = /^([\w-]+(?:\.[\w-]+)*)@((?:[\w-]+\.)*\w[\w-]{0,66})\.([a-z]{2,6}(?:\.[a-z]{2})?)$/i;
			return form.test(email);
		}
		
	</script>
</body>
<%@ include file="../footer.jsp"%>
</html>

```

회원가입과 거의 동일하게 만들어 놓은 후 수정이 불가능한 부분은 readonly로 수정을 막고, 수정이 가능한 변수만 입력 할 수 있게 만든다.

![_config.yml]({{ site.baseurl }}/img/SpringDB/myinfo.png)

### 4. 내 정보 수정 기능 추가

#### UserController.java 에 추가

```java
// 내 정보 수정
	@PostMapping("/userupdate")
	public String userUpdatePOST(UserVO user, RedirectAttributes rttr) throws Exception {

		String rawPw = ""; // 인코딩 전 비밀번호
		String encodePw = ""; // 인코딩 후 비밀번호

		rawPw = user.getPass(); // 비밀번호 데이터 얻음
		encodePw = pwEncoder.encode(rawPw); // 비밀번호 인코딩
		user.setPass(encodePw); // 인코딩된 비밀번호 member객체에 다시 저장

		userService.userUpdate(user);

		return "redirect:/";

	}
```

수정한 비밀번호는 다시 인코딩을 거친 후 DB에 저장된다.

#### UserMapper.java 에 추가

```java
	// 내 정보 수정
	public int userUpdate(UserVO user);
```

#### UserService.java 에 추가

```java
	// 내 정보 수정
	public int userUpdate(UserVO user) throws Exception;
```

#### UserServiceImpl.java 에 추가

```java
	@Override
	public int userUpdate(UserVO user) throws Exception {

		return usermapper.userUpdate(user);
	}
```

#### UserMapper.xml 에 추가

```xml
	<!-- 내 정보 수정 -->
	<update id="userUpdate">

		update shopuser set pass = #{pass}, name = #{name},
		email = #{email},
		address1 = #{address1}, address2 = #{address2},
		address3 = #{address3}, phone = #{phone}, gender = #{gender}
		where
		userid = #{userid}

	</update>
```

### 5. 탈퇴 기능 추가

#### myPageInfo 에 스크립트 추가

```js
$(document).ready(function() {
			  $('#user_exit_btn').on('click', function() {
			    if(confirm("DB에서 탈퇴 시 모든 정보는 되돌릴 수 없습니다.\n탈퇴 하시겠습니까?")) {
			      var form = $('form');
			      form.attr('action', '/user/userexit');
			      form.attr('method', 'post');
			      form.submit();
			    }
			  });
			});
```

오른쪽 아래에 있는 탈퇴하기 버튼을 클릭 하면 confirm 후 해당 메서드를 실행한다.

#### UserController.java 에 추가

```java
	// 회원 탈퇴
	@PostMapping("/userexit")
	public String userExitPOST(UserVO user, RedirectAttributes rttr, HttpServletRequest request) throws Exception {

		userService.userExit(user);

		HttpSession session = request.getSession();

		session.invalidate(); // 탈퇴 메소드 실행 후 세션 종료

		return "redirect:/";
	}
```

#### UserMapper.java 에 추가

```java
	// 회원 탈퇴
	public int userExit(UserVO user);
```

#### UserService.java 에 추가

```java
	// 회원 탈퇴
	public int userExit(UserVO user) throws Exception;
```

#### UserServiceImpl.java 에 추가

```java
	@Override
	public int userExit(UserVO user) throws Exception {

		return usermapper.userExit(user);
	}
```

#### UserMapper.xml 에 추가

```xml
	<!-- 회원 탈퇴 -->
	<delete id="userExit">

		delete from shopuser where userid = #{userid}

	</delete>
```

## [프로젝트 주소](https://github.com/GreenteaPIE/TeamProjectDBSpringVer)
