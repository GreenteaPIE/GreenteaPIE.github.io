---
layout: post
title: 18 - DB Spring 어드민 회원 관리
date: 2023-05-28
excerpt: "팀 프로젝트 DB Spring Version"
tags: [project, java, jsp, Oracle, css, HTML, BootStrap, API, JQuery, JavaScript, Spring, FrameWork]
feature: /img/SpringDB/logo.png
comments: true



---


> **사용한 플랫폼 : Spring, Oracle**



### 1.  어드민 회원 목록 기능

#### header.jsp 에 수정

어드민으로 로그인 했을 때 각각의 관리 페이지로 넘겨줄 버튼을 만들어 준다.

```jsp
							<c:if test="${user.grade == 1}">
								<div class="nav-item dropdown">
									<a href="#" class="nav-link dropdown-toggle" data-toggle="dropdown">관리자</a>
									<div class="dropdown-menu rounded-0 m-0">
										<a href="/admin/userManagementPage" class="dropdown-item">회원 관리</a>
										<a href="/admin/adminAuctionBrandList" class="dropdown-item">옥션 관리</a>
										<a href="/admin/productManagementPage" class="dropdown-item">브랜드&상품 관리</a>
										<a href="/admin/sales_OrderManagement" class="dropdown-item">매출&주문 관리</a>
									</div>
								</div>
							</c:if>
```

![_config.yml]({{ site.baseurl }}/img/SpringDB/admintab.png)

#### AdminMapper.java 추가

유저 정보를 담은 리스트를 반환하는 getUserList 메서드를 만든다.

```java
package com.db.mapper;

import java.util.ArrayList;

import com.db.model.UserVO;

public interface AdminMapper {
	
	public ArrayList<UserVO> getUserList(); // 유저 목록

}

```

#### AdminMapper.xml 추가

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.db.mapper.AdminMapper">

	<!-- 유저목록 -->
	<select id="getUserList" resultType="com.db.model.UserVO">
		select * from shopuser
	</select>

</mapper>
```

#### AdminService.java 에 추가

```java
package com.db.service;

import java.util.ArrayList;

import com.db.model.UserVO;

public interface AdminService {
	
	public ArrayList<UserVO> getUserList() throws Exception; // 유저 목록

}
```

#### AdminServiceImpl.java 에 추가

```java
package com.db.service;

import java.util.ArrayList;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import com.db.mapper.AdminMapper;
import com.db.model.UserVO;

@Service
public class AdminServiceImpl implements AdminService{
	
	@Autowired
	AdminMapper mapper;
	
	@Override
	public ArrayList<UserVO> getUserList() throws Exception {
		return mapper.getUserList();
	}

}
```

#### userManagementPage.jsp 추가

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt"%>
<!DOCTYPE html>
<html>
<head>
<script src="https://code.jquery.com/jquery-3.4.1.js" integrity="sha256-WpOohJOqMqqyKL9FccASB9O0KwACQJpFTUBLTYOVvVU=" crossorigin="anonymous"></script>
<meta charset="UTF-8">
<style>
th {
	border: solid 1px gray;
	border-collapse: collapse;
	text-align: center;
}

td {
	border: solid 1px #0e0e0e;
	border-collapse: collapse;
	width: 100px;
	white-space: nowrap; /* 줄바꿈 방지 */
	text-align: center;
}

.hiddentd {
	width: 100px;
	text-align: center;
}

.button:hover {
	background-color: #D9D9D9;
}

tr:hover .hiddentd {
	background-color: white;
}

.point {
	width: 10px;
}

table.table {
	font-size: 11px;
}
</style>
<jsp:include page="../header.jsp"></jsp:include>
</head>
<body>
	<hr>
	<div class="container bg-secondary mb-3" style="max-width: 800px;">
		<div class="d-flex flex-column align-items-center justify-content-center" style="min-height: 200px">
			<h1 class="font-weight-semi-bold text-uppercase mb-3">회원 관리</h1>
			<div class="d-inline-flex">
				<p class="m-0">
					<a href="/">Home</a>
				</p>
			</div>
		</div>
	</div>
	<div>
		<div class="row" style="margin-left: 30px; margin-right: 30px">
			<table class="table" style="text-align: center; border: 1px solid #dddddd">
				<thead>
					<tr>
						<th style="background-color: #eeeeee; text-align: center;">아이디</th>
						<th style="background-color: #eeeeee; text-align: center;">이름</th>
						<th style="background-color: #eeeeee; text-align: center;">성별</th>
						<th style="background-color: #eeeeee; text-align: center;">이메일</th>
						<th style="background-color: #eeeeee; text-align: center;">우편번호</th>
						<th style="background-color: #eeeeee; text-align: center;">주소</th>
						<th style="background-color: #eeeeee; text-align: center;">전화번호</th>
						<th style="background-color: #eeeeee; text-align: center;">등급</th>
						<th style="background-color: #eeeeee; text-align: center;">포인트</th>
						<th style="background-color: #eeeeee; text-align: center;">가입일자</th>
						<th style="background-color: #eeeeee; text-align: center;">관리</th>
					</tr>
				</thead>
				<c:forEach var="list" items="${list }">
					<tr class="button">
						<td>${list.userid }</td>
						<td>${list.name }</td>
						<c:choose>
							<c:when test="${list.gender==1}">
								<td>남자</td>
							</c:when>
							<c:when test="${list.gender==2}">
								<td>여자</td>
							</c:when>
						</c:choose>
						<td>${list.email }</td>
						<td>${list.address1 }</td>
						<td>${list.address2 },${list.address3 }</td>
						<td>${list.phone }</td>
						<c:choose>
							<c:when test="${list.grade == 0 }">
								<td>브론즈</td>
							</c:when>
							<c:when test="${list.grade == 1 }">
								<td>관리자</td>
							</c:when>
							<c:when test="${list.grade == 2 }">
								<td>실버</td>
							</c:when>
							<c:when test="${list.grade == 3 }">
								<td>골드</td>
							</c:when>
							<c:when test="${list.grade == 4 }">
								<td>다이아</td>
							</c:when>
							<c:otherwise>
								<td>관리자</td>
							</c:otherwise>
						</c:choose>
						<td>${list.point }</td>
						<td><fmt:formatDate value="${list.enter }" /></td>
						<td class="hiddentd"><input type="button" class="btn-primary px-3 hiddenbutton" value="수정" onclick="open_win('/admin/userManagementEdit?userid=${list.userid }')"> <input type="button" class="btn-primary px-3 delete hiddenbutton" value="삭제" onclick="if (confirm('유저를 탈퇴 처리 하시겠습니까?')) { location.href='/admin/userDelete?shopUserid=${list.userid}' };"></td>
					</tr>
				</c:forEach>
			</table>
		</div>
	</div>
    <!-- 회원 검색 -->
    
    
	<!-- 페이징 -->


	<hr>

</body>
<jsp:include page="../footer.jsp"></jsp:include>
</html>

```

#### AdminController.java 추가

```java
package com.db.controller;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;

import com.db.mapper.PageMakerDTO;
import com.db.model.Criteria;
import com.db.service.AdminService;

@Controller
@RequestMapping("/admin")
public class AdminController {

	@Autowired
	AdminService adminService;
	
	// 회원관리
	// 회원관리 페이지
	@GetMapping("userManagementPage")
	public void userManagementpageGET(Model model, Criteria cri, @RequestParam(required = false) String category)
			throws Exception {
		System.out.println("userManagementPage 접속");

		cri.setCategory(category); // category 값을 저장합니다.

		model.addAttribute("list", adminService.getUserListPaging(cri));

		int total = adminService.getUserTotal(cri);

		PageMakerDTO pageMake = new PageMakerDTO(cri, total);

		model.addAttribute("pageMaker", pageMake);
	}
}

```

### 2. 회원 검색 기능

#### AdminMapper.xml 에 추가

```xml
	<!-- 검색 조건문 -->
	<sql id="criteria">
		<trim prefix="AND (" suffix=")" prefixOverrides="OR">
			<foreach collection="typeArr" item="type">
				<trim prefix="OR">
					<choose>
						<when test="type == 'T'.toString()">
							name like '%'||#{keyword}||'%'
						</when>
						<when test="type == 'C'.toString()">
							phone like '%'||#{keyword}||'%'
						</when>
						<when test="type == 'W'.toString()">
							userid like '%'||#{keyword}||'%'
						</when>
						<when test="type == 'M'.toString()">
							userid = #{keyword}
						</when>
					</choose>
				</trim>
			</foreach>
		</trim>
		 <!-- 카테고리 검색 처리 -->
  <if test="category != null and (category == 'F'.toString() or category == 'S'.toString() or category == 'Q'.toString())">
    AND category = #{category}
  </if>

	</sql>
```

검색 조건문을 정의 해준다. 이름은 T, 전화번호는 C, 유저아이디는 W의 키워드 값으로 조건문을 실행한다.

#### UserManagementPage.jsp 에 추가

```jsp
	<div class="search_wrap">
		<div class="search_area" style="margin-left: 400px;">
			<select name="type">
				<option value="" <c:out value="${pageMaker.cri.type == null?'selected':'' }"/>>--</option>
				<option value="W" <c:out value="${pageMaker.cri.type eq 'W'?'selected':'' }"/>>유저 아이디</option>
				<option value="C" <c:out value="${pageMaker.cri.type eq 'C'?'selected':'' }"/>>전화번호</option>
				<option value="T" <c:out value="${pageMaker.cri.type eq 'T'?'selected':'' }"/>>이름</option>
			</select>
			<input type="text" name="keyword" value="${pageMaker.cri.keyword}">
			<button class="btn btn-primary pull-right">Search</button>
		</div>
	</div>

<script>
		$(".search_area button").on("click", function(e) {
			e.preventDefault();

			let type = $(".search_area select").val();
			let keyword = $(".search_area input[name='keyword']").val();
			let category = $("input[name='category']").val();

			if (!type) {
				alert("검색 종류를 선택하세요.");
				return false;
			}

			if (!keyword) {
				alert("키워드를 입력하세요.");
				return false;
			}

			moveForm.find("input[name='type']").val(type);
			moveForm.find("input[name='keyword']").val(keyword);
			moveForm.find("input[name='category']").val(category);
			moveForm.find("input[name='pageNum']").val(1);
			moveForm.submit();
		});
</script>
```

### 3. 회원 목록 페이징 기능

#### AdminMapper.java 에 추가

```java
	// 유저 목록 페이징
	public List<UserVO> getUserListPaging(Criteria cri);

	// 유저 총원
	public int getUserTotal(Criteria cri);
```

#### AdminMapper.xml 에 추가

```xml
	<!-- 회원 목록(페이징) -->
	<select id="getUserListPaging" resultType="com.db.model.UserVO"> 
	
 	<![CDATA[ -->
		
		select userid, name, gender, email, address1, address2, address3, phone, grade, point, enter from( 
		
	        select /*+INDEX_ASC(shopuser idx_shopuser_enter_desc) */ rownum  as rn, userid, name, gender, email, address1, address2, address3, phone, grade, point, enter
		          
 		        from shopuser where rownum <= #{pageNum} * #{amount}  
 	]]>
		<if test="keyword != null">
			<include refid="criteria"></include>
		</if> 
	
 	<![CDATA[ 
			        
 			    ) 
			        
 		where rn > (#{pageNum} -1) * #{amount} 
	
 	]]>

	</select>
	
	<!-- 총 회원 수 -->
	<select id="getUserTotal" resultType="int">

		select count(*) from free_board

		<if test="keyword != null">

			where num > 0
			<include refid="criteria"></include>

		</if>

	</select>
```

#### AdminService.java 에 추가

```java
	// 유저 목록 페이징
	public List<UserVO> getUserListPaging(Criteria cri) throws Exception;

	// 유저 총원
	public int getUserTotal(Criteria cri) throws Exception;
```

#### AdminServiceImpl.java 에 추가

```java
	@Override
	public List<UserVO> getUserListPaging(Criteria cri) throws Exception {
		return mapper.getUserListPaging(cri);
	}

	@Override
	public int getUserTotal(Criteria cri) throws Exception {
		return mapper.getUserTotal(cri);
	}
```

#### userManagementPage.jsp 에 추가

```jsp
	<div class="col-12 pb-1">
		<div class="pageInfo_wrap">
			<div class="pageInfo_area">
				<nav aria-label="Page navigation">
					<ul id="pageInfo" class="pagination justify-content-center mb-3">
						<!-- 이전 페이지 버튼 -->
						<c:if test="${pageMaker.prev}">
							<li class="page-item"><a class="page-link" href="${pageMaker.startPage-1}" aria-label="Previous">
									<span aria-hidden="true">&laquo;</span>
									<span class="sr-only">Previous</span>
								</a></li>
						</c:if>
						<!-- 각 번호 페이지 버튼 -->
						<c:forEach var="num" begin="${pageMaker.startPage}" end="${pageMaker.endPage}">
							<li class="page-item ${pageMaker.cri.pageNum == num ? 'active' : ''}"><a class="page-link" href="${num}">${num}</a></li>
						</c:forEach>
						<!-- 다음 페이지 버튼 -->
						<c:if test="${pageMaker.next}">
							<li class="page-item"><a class="page-link" href="${pageMaker.endPage + 1}" aria-label="Next">
									<span aria-hidden="true">&raquo;</span>
									<span class="sr-only">Next</span>
								</a></li>
						</c:if>
					</ul>
				</nav>
			</div>
		</div>
		<form id="moveForm" method="get">
			<input type="hidden" name="pageNum" value="${pageMaker.cri.pageNum}">
			<input type="hidden" name="amount" value="${pageMaker.cri.amount}">
			<input type="hidden" name="keyword" value="${pageMaker.cri.keyword}">
			<input type="hidden" name="type" value="${pageMaker.cri.type}">
			<input type="hidden" name="category" value="${pageMaker.cri.category}">
		</form>
	</div>
	
	<script>
		//페이징 스크립트
		let moveForm = $("#moveForm");

		$("#pageInfo a").on("click", function(e) {
			e.preventDefault();
			moveForm.find("input[name='pageNum']").val($(this).attr("href"));
			moveForm.attr("action", "/admin/userManagementPage");
			moveForm.submit();

		});
</script>
```

![_config.yml]({{ site.baseurl }}/img/SpringDB/customerlist.png)

### 4. 회원 정보 수정 기능

#### userManagementPage.jsp 에 추가

```js
		function open_win(url) {
			// 새 창의 속성들을 설정하는 문자열입니다.
			var specs = "width=900,height=600,left=200,top=200";

			// 새 창을 엽니다.
			window.open(url, "_blank", specs);
		}
```

수정 버튼을 누르면 새창을 열어 해당 창에서 수정을 완료 시킨 후 창이 종료되게 만든다.

#### AdminMapper.java 에 추가

```java
	public UserVO getUser(String userid); // 유저 정보
```

#### AdminMapper.xml 에 추가

```xml
	<!-- 유저 상세보기 -->
	<select id="getUser" resultType="com.db.model.UserVO">
		select * from shopuser where
		userid = #{userid}
	</select>
```

#### AdminService.java 에 추가

```java
	public UserVO getUser(String userid) throws Exception; // 유저 정보
```

#### AdminServiceImpl.java 에 추가

```java
	@Override
	public UserVO getUser(String userid) throws Exception {
		return mapper.getUser(userid);
	}
```

#### userManagementEdit.jsp 추가

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt"%>
<!DOCTYPE html>
<html>
<head>
<link href="../resources/css/style.css" rel="stylesheet">
<script src="https://code.jquery.com/jquery-3.4.1.js" integrity="sha256-WpOohJOqMqqyKL9FccASB9O0KwACQJpFTUBLTYOVvVU=" crossorigin="anonymous"></script>
<meta charset="UTF-8">
</head>
<body>
	<div id="wrap" align="center">
		<h1>유저 정보 수정</h1>
		<form action="userEditComplete.do" method="post" name="frm" id="frm">
			<table class="table">
				<tr>
					<th>아이디</th>
					<td><input type="text" class="form-control" name="userid" value="${shopUser.userid }" readonly="readonly"></td>
				</tr>
				<tr>
					<th>비밀번호</th>
					<td><input type="text" class="form-control" name="pass" id="passwordField" value="${shopUser.pass}" data-value="${shopUser.pass}" readonly="readonly"></td>
				</tr>
				<tr>
					<th>이름</th>
					<td><input type="text" class="form-control" name="name" value="${shopUser.name }" required="required"> <c:choose>
							<c:when test="${shopUser.gender ==1}">
								<td><input type="radio" name="gender" value="1" checked="checked"> 남자 <input type="radio" name="gender" value="2"> 여자</td>
							</c:when>
							<c:otherwise>
								<td><input type="radio" name="gender" value="1"> 남자 <input type="radio" name="gender" value="2" checked="checked"> 여자</td>
							</c:otherwise>
						</c:choose></td>
				</tr>
				<tr>
					<th>이메일</th>
					<td><input type="email" class="form-control" name="email" value="${shopUser.email }"></td>
				</tr>
				<tr>
					<th>전화번호</th>
					<td><input type="text" class="form-control" name="phone" value="${shopUser.phone }" placeholder="01012341234"></td>
				</tr>
				<tr>
					<th>포인트</th>
					<td><input type="text" class="form-control" name="point" value="${shopUser.point }"></td>
					</tr>
					<tr>
					<th>등급</th>
					<td><select name="grade">
							<c:choose>
								<c:when test="${shopUser.grade == 0 }">
									<option value="1">관리자</option>
									<option value="0" selected="selected">브론즈</option>
									<option value="2">실버</option>
									<option value="3">골드</option>
									<option value="4">다이아</option>
								</c:when>
								<c:when test="${shopUser.grade == 1 }">
									<option value="1" selected="selected">관리자</option>
									<option value="0">브론즈</option>
									<option value="2">실버</option>
									<option value="3">골드</option>
									<option value="4">다이아</option>
								</c:when>
								<c:when test="${shopUser.grade == 2 }">
									<option value="1">관리자</option>
									<option value="0">브론즈</option>
									<option value="2" selected="selected">실버</option>
									<option value="3">골드</option>
									<option value="4">다이아</option>
								</c:when>
								<c:when test="${shopUser.grade == 3 }">
									<option value="1">관리자</option>
									<option value="0">브론즈</option>
									<option value="2">실버</option>
									<option value="3" selected="selected">골드</option>
									<option value="4">다이아</option>
								</c:when>
								<c:when test="${shopUser.grade == 4 }">
									<option value="1">관리자</option>
									<option value="0">브론즈</option>
									<option value="2">실버</option>
									<option value="3">골드</option>
									<option value="4" selected="selected">다이아</option>
								</c:when>
							</c:choose>
					</select></td>
				</tr>
				<tr>
					<th>우편번호</th>
					<td><input type="text" class="form-control" name="address1" value="${shopUser.address1 }"></td>
				</tr>
				<tr>
					<th>주소</th>
					<td><input type="text" class="form-control" name="address2" value="${shopUser.address2 }"></td>
					<td><input type="text" class="form-control" name="address3" value="${shopUser.address3 }"></td>
				</tr>
				<tr>
					<th>가입일자</th>
					<td><fmt:formatDate value="${shopUser.enter }" /></td>
				</tr>
			</table>
			<input type="submit" class="btn btn-primary" value="수정">
		</form>
	</div>
	<script>
		$(document).ready(function() { // 비밀번호 필드의 원래 값 가져오기 
			var passwordField = $("#passwordField"); var passwordText = passwordField.val();
			// 비밀번호 필드의 값을 별표 문자로 교체
			var obscuredPassword = '*'.repeat(passwordText.length);
			passwordField.val(obscuredPassword);

			// 폼 제출 이벤트 처리
			$('#frm').submit(function(event) {
				event.preventDefault(); // 기본 제출 동작을 막습니다.

				passwordField.val(passwordText); // 전송 전 원래 비밀번호로 복원
				var formData = $(this).serialize(); // 폼 데이터를 직렬화합니다.
				passwordField.val(obscuredPassword); // 전송 후 별표 문자로 다시 변경

				// AJAX 요청
				$.ajax({
					url : '/admin/userEditComplete.do',
					type : 'post', // POST 방식으로 변경
					data : formData,
					success : function(response) {
						if (response == 'success') {
							alert("수정 완료");
							window.close();
							if (window.opener && !window.opener.closed) {
								window.opener.location.reload();
							}
						}
					},
					error : function(xhr, status, error) {
						// Ajax 요청이 실패했을 때 실행되는 콜백 함수입니다.
						console.error('오류가 발생했습니다:', error);
					}
				});
			});
		});
	</script>
</body>
</html>
```

![_config.yml]({{ site.baseurl }}/img/SpringDB/customermodify.png)

#### AdminController.java 에 추가

```java
	// 회원정보 수정
	@GetMapping("userManagementEdit")
	public void userManagementEditGET(String userid, Model model) throws Exception {
		System.out.println("userManagementEdit 접속");
		UserVO user = adminService.getUser(userid);
		model.addAttribute("shopUser", user);
	}
```

#### AdminMapper.java 에 추가

```java
	public void adminUserUpdate(UserVO uVo); // 유저 수정
```

#### AdminMapper.xml 에 추가

```xml
	<!-- 유저 업데이트 -->
	<update id="adminUserUpdate">
		update shopuser set name = #{name}, email = #{email},
		point = #{point}, grade=#{grade},
		address1 = #{address1}, address2 =
		#{address2},
		address3 = #{address3}, phone = #{phone}, gender =
		#{gender}
		where userid = #{userid}
	</update>
```

#### Adminservice.java 에 추가

```java
	public void adminUserUpdate(UserVO uVo) throws Exception; // 유저 수정
```

#### AdminserivceImpl.java 에 추가

```java
	@Override
	public void adminUserUpdate(UserVO uVo) throws Exception {
		mapper.adminUserUpdate(uVo);
	}
```

#### userEditComplete.jsp 추가

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
</head>
<body>
<script type="text/javascript">
window.opener.parent.location.href = "redirect:/admin/userManagementPage";
window.close();
</script>
</body>
</html>
```

#### AdminController.java 에 추가

```java
    @Autowired
	UserService userService;

    // 회원 수정 완료
	@PostMapping("userEditComplete.do")
	@ResponseBody
	public String userEditCompletePOST(UserVO vo) throws Exception {
		System.out.println("userEditComplete.do 접속");
		System.out.println("uservo: " + vo);
		adminService.adminUserUpdate(vo);
		System.out.println("userupdate: " + userService.userUpdate(vo));
		String response = "success";
		return response;
	}
```

#### 5. 회원 삭제 기능

#### AdminMapper.java 에 추가

```java
	public void deleteUser(String userid); // 유저 삭제
```

#### AdminMapper.xml 에 추가

```xml
	<!-- 유저 삭제 -->
	<delete id="deleteUser">
		delete shopuser where userid = #{userid}
	</delete>
```

#### AdminService.java 에 추가

```java
	public void deleteUser(String userid) throws Exception; // 유저 삭제
```

#### AdminServiceImpl.java 에 추가

```java
@Override
	public void deleteUser(String userid) throws Exception {
		mapper.deleteUser(userid);
	}
```

#### AdminController.java 에 추가

```java
	// 회원 삭제
	@GetMapping("userDelete")
	public String userDeleteGET(String shopUserid) throws Exception {
		System.out.println("userDelete 접속");
		adminService.deleteUser(shopUserid);
		return "redirect:/admin/userManagementPage";
	}
```



## [프로젝트 주소](https://github.com/GreenteaPIE/TeamProjectDBSpringVer)
