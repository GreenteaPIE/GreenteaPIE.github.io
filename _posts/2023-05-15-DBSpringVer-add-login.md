---
layout: post
title: 4 - DB Spring 로그인과 로그아웃
date: 2023-05-15
excerpt: "팀 프로젝트 DB Spring Version"
tags: [project, java, jsp, Oracle, css, HTML, BootStrap, API, JQuery, JavaScript, Spring, FrameWork]
feature: /img/SpringDB/logo.png
comments: true


---


> **사용한 플랫폼 : Spring, Oracle**



### 1.  login.jsp

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
	<form id="login_form" method="post">
		<div class="form-group input-group fg-x700">
			<div class="col-lg-4"></div>
			<div class="col-lg-4">
				<div class="jumbotron" style="padding-top: 20px;">
					<h3 style="text-align: center;">로그인</h3>
					<div class="form-group">
						<input type="text" class="form-control id_input" placeholder="아이디" name="userid" value="" maxlength="20">
					</div>
					<div class="form-group">
						<input type="password" class="form-control pw_input" placeholder="비밀번호" name="pass" maxlength="20">
					</div>
					<input type="button" class="btn btn-primary form-control login_button" value="로그인"> <input type="reset" class="btn btn-primary form-control" value="취소"> <input type="button" class="btn btn-primary form-control" value="회원가입" onclick="location.href='/user/join'">
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
		$(".login_button").click(function(){
			//alert("로그인 버튼 작동");
			
			// 로그인 메서드 서버 요청
			$("#login_form").attr("action", "/user/login");
			$("#login_form").submit();
		});
		
	</script>
	<hr>
</body>
<%@ include file="../footer.jsp"%>
</html>
```

![_config.yml]({{ site.baseurl }}/img/SpringDB/login.png)

### 2. 로그인 기능 추가

#### UserController 에 추가

```java
// 로그인
	@PostMapping("login")
	public String loginPOST(HttpServletRequest request, UserVO user, RedirectAttributes rttr) throws Exception {

		System.out.println("login 메소드 진입");
		HttpSession session = request.getSession();
		String rawPw = "";
		String encodePw = "";
		UserVO vo = userService.userLogin(user);

		// 비밀번호 인코딩
		if (vo != null) { // 아이디&비밀번호 일치(로그인 성공)
			rawPw = user.getPass(); // 사용자가 제출한 비밀번호
			encodePw = vo.getPass(); // DB에 저장 되어있는 인코딩된 비밀번호
			if (true == pwEncoder.matches(rawPw, encodePw)) {
				vo.setPass("");
				session.setAttribute("user", vo);

				return "redirect:/";
			} else { // 비밀번호가 일치 하지 않을때(로그인 실패)
				rttr.addFlashAttribute("result", 0);
				return "redirect:/user/login";
			}
		} else { // 일치하는 아이디가 없을때(로그인 실패)
			rttr.addFlashAttribute("result", 0);
			return "redirect:/user/login"; // 로그인 페이지로 이동
		}
	}
```

인코딩 되어 저장된 DB와 사용자가 입력한 비밀번호를 인코딩하여 일치하는지 비교한다.

로그인이 성공하면 불러온 user정보를 session에 입력한다.

#### UserMapper.java 에 추가

```java
// 로그인
	public UserVO userLogin(UserVO user);
```

#### UserService.java 에 추가

```java
// 로그인
	public UserVO userLogin(UserVO user) throws Exception;
```

#### UserServiceImpl 에 추가

```java
@Override
	public UserVO userLogin(UserVO user) throws Exception {

		return usermapper.userLogin(user);
	}
```

UserMapper.xml 에 추가

```xml
<!-- 로그인 -->
	<select id="userLogin" resultType="com.db.model.UserVO">

		select * from shopuser where
		userid = #{userid}

	</select>
```

### 3. 로그아웃 기능 추가

#### UserController.java 에 추가

```java
// 로그아웃
	@GetMapping("logout")
	public String logoutMainGET(HttpServletRequest request) throws Exception {

		logger.info("logout 메소드 진입");

		HttpSession session = request.getSession();

		session.invalidate();

		return "redirect:/";

	}
```

## [프로젝트 주소](https://github.com/GreenteaPIE/TeamProjectDBSpringVer)
