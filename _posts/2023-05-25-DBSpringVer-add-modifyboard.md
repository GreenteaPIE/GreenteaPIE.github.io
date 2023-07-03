---
layout: post
title: 15 - DB Spring 게시판 조회, 수정, 삭제
date: 2023-05-25
excerpt: "팀 프로젝트 DB Spring Version"
tags: [project, java, jsp, Oracle, css, HTML, BootStrap, API, JQuery, JavaScript, Spring, FrameWork]
feature: /img/SpringDB/logo.png
comments: true


---


> **사용한 플랫폼 : Spring, Oracle**



### 1.  게시판 조회 기능

#### BoardMapper.java 에 추가

```java
	// 게시판 조회
	public BoardVO getPage(int num);
```

하나의 게시판 정보를 얻기위해선 해당 게시판의 게시판 번호를 알아야 하기 때문에 게시판 정보 데이터를 전달받을 수 있도록 int형 변수를 파라미터로 추가한다.

#### BoardMapper.xml 에 추가

```xml
    <!-- 게시판 조회 -->
	<select id="getPage" resultType="com.db.model.BoardVO">

		select * from free_board where
		num = #{num}

	</select>
```

#### BoardService.java 에 추가

하나의 게시판 정보만 반환받아야 하기 때문에 반환 타입을 BoardVO로 한다.

```java
		//게시판 조회
		public BoardVO getPage(int num);
```

#### BoardServiceImpl.java 에 추가

```java
	@Override
	public BoardVO getPage(int num) {
		
		return mapper.getPage(num);
	}

```

인터페이스에서 선언한 메서드를 구현한다. <br>구현부에는 BoardMapper.java 인터페이스의 '게시판 조회' 메서드를 호출한다.

#### BoardController.java 에 추가

```java
	// 게시판 조회
	@GetMapping("/get")
	public void boardGetPageGET(@RequestParam("num") int num, Model model, Criteria cri) throws Exception {
		log.info("게시글번호:" + num);
		model.addAttribute("pageInfo", bservice.getPage(num));
		model.addAttribute("cri", cri);

	}
```

#### BoardMapper.java 에 추가

게시판을 조회할때마다 해당 게시판의 조회수를 올려주는 메서드를 추가한다.

```java
	// 게시판 조회수 업데이트
	public void updateReadCount(int num);
```

#### BoardMapper.xml 에 추가

```xml
<!-- 조회수 업데이트 -->
	<update id="updateReadCount" parameterType="int">

		update free_board set
		readcount = readcount+1 where num= #{num}

	</update>
```

#### BoardService.java 에 추가

```java
		// 게시판 조회수 업데이트
		public void updateReadCount(int num);
```

#### BoardServiceImpl.java 에 추가

```java
	@Override
	public void updateReadCount(int num) {
		
		mapper.updateReadCount(num);
	}
```

#### BoardController.java 에 수정

```java
	// 게시판 조회
	@GetMapping("/get")
	public void boardGetPageGET(@RequestParam("num") int num, Model model, Criteria cri) throws Exception {
		bservice.updateReadCount(num);  //조회수 업데이트
		log.info("게시글번호:" + num);
		model.addAttribute("pageInfo", bservice.getPage(num));
		model.addAttribute("cri", cri);
	}
```

#### get.jsp 추가

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<script src="https://code.jquery.com/jquery-3.4.1.js" integrity="sha256-WpOohJOqMqqyKL9FccASB9O0KwACQJpFTUBLTYOVvVU=" crossorigin="anonymous"></script>
<script type="text/javascript" src="../js/freeboard.js"></script>
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.16.0/umd/popper.min.js"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/4.5.2/js/bootstrap.min.js"></script>
<jsp:include page="../header.jsp"></jsp:include>
</head>
<body>
	<hr>
	<div class="container">
		<div class="row">
			<table class="table" style="text-align: center; border: 1px solid #dddddd">
				<thead>
					<tr>
						<c:if test="${pageInfo.category eq 'F'}">
							<th colspan="4" style="background-color: #eeeeee; text-align: left;">
								<h4 style="font-family: sans-serif; font-weight: 600; margin: 0; padding: 0;">
									<span style="color: #333; margin-left: 30px;">자유 게시판</span>
								</h4>
							</th>
						</c:if>
						<c:if test="${pageInfo.category eq 'Q'}">
							<th colspan="4" style="background-color: #eeeeee; text-align: left;">
								<h4 style="font-family: sans-serif; font-weight: 600; margin: 0; padding: 0;">
									<span style="color: #333; margin-left: 30px;">질문 게시판</span>
								</h4>
							</th>
						</c:if>
						<c:if test="${pageInfo.category eq 'S'}">
							<th colspan="4" style="background-color: #eeeeee; text-align: left;">
								<h4 style="font-family: sans-serif; font-weight: 600; margin: 0; padding: 0;">
									<span style="color: #333; margin-left: 30px;">공지사항</span>
								</h4>
							</th>
						</c:if>
					</tr>
				</thead>
				<tbody>
					<tr>
						<td style="text-align: left;" colspan="3"><span style="margin-left: 30px; font-size: 20px; font: bold;">${pageInfo.title}</span></td>
					</tr>
					<tr>
						<td style="text-align: left;"><span style="margin-left: 30px;">
								<c:out value="${pageInfo.userid}" />
								&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;
								<fmt:formatDate pattern="yyyy. MM. dd. HH:mm" value="${pageInfo.writedate}" />
							</span></td>
						<td colspan="1" style="text-align: right;"><span style="margin-right: 30px;">조회 ${pageInfo.readcount} &nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp; 댓글갯수 영역</span></td>
					</tr>
					<tr style="height: 500px;">
						<td colspan="4" style="text-align: left;"><span style="margin-left: 30px;">${pageInfo.content}</span>
					</tr>
				</tbody>
			</table>
		</div>
		<hr />

         <!-- 댓글 영역 -->

	</div>
	<div class="text-center mt-3">
		<c:if test="${pageInfo.userid eq user.userid or user.userid eq 'admin'}">
			<button class="btn btn-primary" id="modify_btn">게시글 수정</button>
		</c:if>
		<button class="btn btn-primary" id="list_btn">게시글 리스트</button>
		<c:if test="${not empty user.userid}">
			<a href="/board/enroll" class="btn btn-primary pull-right" style="color: white;">게시판 등록</a>
		</c:if>
		<c:if test="${empty user.userid}">
			<button class="btn btn-primary" onclick="alert('로그인 후에 이용이 가능합니다.');">게시글 등록</button>
		</c:if>
	</div>
	<form id="infoForm" action="/board/modify" method="get">
		<input type="hidden" id="num" name="num" value='<c:out value="${pageInfo.num}"/>'>
		<input type="hidden" name="pageNum" value='<c:out value="${cri.pageNum}"/>'>
		<input type="hidden" name="amount" value='<c:out value="${cri.amount}"/>'>
		<input type="hidden" name="type" value="${cri.type}">
		<input type="hidden" name="keyword" value="${cri.keyword}">
		<input type="hidden" name="category" value="${pageInfo.category }">
	</form>
	<script>
		let form = $("#infoForm");

		$("#list_btn").on("click", function(e) {
			form.find("#num").remove();
			form.attr("action", "/board/list");
			form.submit();

		});
		$("#modify_btn").on("click", function(e) {
			form.attr("action", "/board/modify");
			form.submit();

		});
	</script>
	<hr>
</body>
<jsp:include page="../footer.jsp"></jsp:include>
</html>

```

![_config.yml]({{ site.baseurl }}/img/SpringDB/bget.png)

게시판을 작성한 작성자 이거나 어드민일 경우 해당 게시판의 수정 페이지로 이동 시키는 버튼을 작동할 수 있게 만든다.

### 2. 게시판 수정 기능

수정을 수행하는 메서드의 경우는 반환 타입이 필요 없다. 하지만 int형으로 설정할 경우 수정 성공 시 1을 반환, 실패할 경우는 0을 반환한다.

#### BoardMapper.java 에 추가

```java
	// 게시판 수정
	public int modify(BoardVO board);
```

#### BoardMapper.xml 에 추가

```xml
	<!-- 게시판 수정 -->
	<update id="modify">
		UPDATE free_board SET title = #{title}, content = #{content} WHERE num
		= #{num}
	</update>
```

#### BoardService.java 에 추가

```java
		//게시판 수정
		public int modify(BoardVO board);
```

해당 메서드에도 반환 타입은 int로 한다. Controller에서 활용하지 않더라도 선택지를 넓혀주는 의미로 설정하였다.

#### BoardServiceImpl.java 에 추가

```java
	@Override
	public int modify(BoardVO board) {
		
		return mapper.modify(board);
	}
```

#### BoardController.java 에 추가

```java
	// 수정 페이지 이동
	@GetMapping("/modify")
	public void boardModifyGET(int num, Model model, Criteria cri) {
		model.addAttribute("pageInfo", bservice.getPage(num));

		model.addAttribute("cri", cri);
	}
```

#### modify.jsp 추가

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<script src="https://code.jquery.com/jquery-3.4.1.js" integrity="sha256-WpOohJOqMqqyKL9FccASB9O0KwACQJpFTUBLTYOVvVU=" crossorigin="anonymous"></script>
<jsp:include page="../header.jsp"></jsp:include>
<script src="//cdn.ckeditor.com/4.21.0/standard/ckeditor.js"></script>
</head>
<body>
	<hr>
	<div class="container">
		<form id="modifyForm" action="/board/modify" name="frm" method="post">
			<input type="hidden" name="num" value='<c:out value="${pageInfo.num}"/>'>
			<div class="row">
				<table class="table" style="text-align: center; border: 1px solid #dddddd">
					<thead>
						<tr>
							<th colspan="2" style="background-color: #eeeeee; text-align: center;">게시글 수정</th>
						</tr>
					</thead>
					<tbody>
						<tr>
							<td colspan="1">작성자  <input type="hidden" class="form-control" name="userid" value="${pageInfo.userid }">${pageInfo.userid }
							</td>
						</tr>
						<tr>
							<td colspan="2"><input type="text" class="form-control" placeholder="${pageInfo.title}" name="title" maxlength="50"></td>
						</tr>
						<tr>
							<td>내용 <textarea class="form-control" id="content" name="content" style="height: 350px;"><c:out value="${pageInfo.content}" /></textarea></td>
						</tr>
					</tbody>
				</table>
			</div>
			<div style="display: flex; justify-content: space-between;">
				<div style="margin-left: 30px;">
					<input id="modify_btn" type="button" class="btn btn-primary" value="등록">
					&nbsp;&nbsp; &nbsp;&nbsp;
					<input id="list_btn" type="button" class="btn btn-primary" value="목록">
				</div>
				<div>
					<input id="delete_btn" class="btn btn-primary" value="삭 제">
				</div>
			</div>
		</form>
	</div>
	<form id="infoForm" action="/board/modify" method="get">
		<input type="hidden" id="num" name="num" value='<c:out value="${pageInfo.num}"/>'>
		<input type="hidden" name="pageNum" value='<c:out value="${cri.pageNum}"/>'>
		<input type="hidden" name="amount" value='<c:out value="${cri.amount}"/>'>
		<input type="hidden" name="type" value="${cri.type}">
		<input type="hidden" name="keyword" value="${cri.keyword}">
	</form>
	<script>
		let form = $("#infoForm"); // 페이지 이동 form(리스트 페이지 이동, 조회 페이지 이동)
		let mForm = $("#modifyForm"); // 페이지 데이터 수정 form

		/* 목록 페이지 이동 버튼 */
		$("#list_btn").on("click", function(e) {
			form.find("#num").remove();
			form.attr("action", "/board/list");
			form.submit();
		});

		/* 수정 하기 버튼 */
		$("#modify_btn").on("click", function(e) {
			mForm.submit();
		});

		/* 삭제 버튼 */
		$("#delete_btn").on("click", function(e) {
			// 확인 메시지 추가
			if (confirm("게시글을 삭제 하시겠습니까?")) {
				form.attr("action", "/board/delete");
				form.attr("method", "post");
				form.submit();
			}
		});

		// 글쓰기 editor 및 사진 업로드 기능
		CKEDITOR.replace('content', {
			filebrowserUploadUrl : '/board/imageUpload.do',
			height : 500
		// Set the desired height value here
		});
	</script>
	<hr>
</body>
<jsp:include page="../footer.jsp"></jsp:include>
</html>

```

![_config.yml]({{ site.baseurl }}/img/SpringDB/boardmodify.png)

#### BoardController.java 에 추가

```java
	// 게시판 수정
	@PostMapping("/modify")
	public String boardModifyPOST(BoardVO board, RedirectAttributes rttr, Criteria cri) {
		bservice.modify(board);

		rttr.addFlashAttribute("result", "modify success");

		return "redirect:/board/list?pageNum=1&amount=10&keyword=&type=&category=" + cri.getCategory();
	}
```

### 3. 게시판 삭제 기능

삭제 기능도 수정과 동일하게 int형으로 작성한다. 삭제 쿼리가 성공하면 1을 반환, 실패하면 0을 반환한다.

#### BoardMapper.java 에 추가

```java
	// 게시판 삭제
	public int delete(int num);
```

#### BoardMapper.xml 에 추가

```xml
	<delete id="delete">
		<!-- 게시판 삭제 -->
		delete from free_board where num = #{num}

	</delete>
```

#### BoardService.java 에 추가

```java
		//게시판 삭제
		public int delete(int num);
```

#### BoardServiceImpl.java 에 추가

```java
	@Override
	public int delete(int num) {
		return mapper.delete(num);
	}
```

#### BoardController.java 에 추가

```java
	// 게시판 삭제
	@PostMapping("/delete")
	public String boardDeletePOST(int num, RedirectAttributes rttr, Criteria cri) {
		bservice.delete(num);

		rttr.addFlashAttribute("result", "delete success");

		return "redirect:/board/list?pageNum=1&amount=10&keyword=&type=&category=" + cri.getCategory();
	}
```

#### list.jsp 에 수정

```js
			function checkAlert(result) {

				if (result === '') {
					return;
				}

				if (result === "enroll success") {
					alert("등록이 완료되었습니다.");
				}
				if (result === "modify success") {
					alert("수정이 완료되었습니다.");
				}
				if (result === "delete success") {
					alert("삭제가 완료되었습니다.");
				}
			}
```

## [프로젝트 주소](https://github.com/GreenteaPIE/TeamProjectDBSpringVer)
