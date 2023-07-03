---
layout: post
title: 13 - DB Spring 게시판 리스트(페이징)
date: 2023-05-23
excerpt: "팀 프로젝트 DB Spring Version"
tags: [project, java, jsp, Oracle, css, HTML, BootStrap, API, JQuery, JavaScript, Spring, FrameWork]
feature: /img/SpringDB/logo.png
comments: true


---


> **사용한 플랫폼 : Spring, Oracle**



### 1.  게시판 리스트 기능

#### BoardMapper.java 에 추가

```jsp
	//게시판 목록
	public List<BoardVO> getList();
```

리스트 기능을 수행하는 List 메서드를 작성한다. 해당 메서드의 반환 타입은 List<BoardVO>로 작성한다.

SELECT 결과 하나의 행에 대한 정보만 얻을 경우에는 BoardVO 클래스 타입으로 반환 받으면 되나, 리스트 페이지의 경우 대부분 2개 이상의 행에 있는 정보를 반환 받아야 하기 때문에 List 탑으로 반환받는다.

#### BoardMapper.xml 에 추가

```xml
	<!-- 게시판 조회 -->
	<select id="getList" resultType="com.db.model.BoardVO">
		SELECT * FROM free_board
	</select>
```

#### BoardService.java 에 추가

```java
		//게시판 목록
		public List<BoardVO> getList();
```

#### BoardServiceImpl.java 에 추가

인터페이스에서 선언한 메서드를 오버라이딩 하여 구현한다.

```java
	@Override
	public List<BoardVO> getList() {
		return mapper.getList();
	}
```

#### BoardController.java 에 추가

```java
// 게시판 리스트 페이지
    @GetMapping("/list")
    // => @RequestMapping(value="list", method=RequestMethod.GET)
    public void boardListGET(Model model) {
  
        model.addAttribute("list", bservice.getList());
        
    }
```

#### list.jsp 추가

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
</head>
<body>
	<hr>
	<div class="container bg-secondary mb-3" style="max-width: 800px;">
		<div class="d-flex flex-column align-items-center justify-content-center" style="min-height: 200px">
			<c:choose>
				<c:when test="${category == 'F'}">
					<h1 class="font-weight-semi-bold text-uppercase mb-3">자유 게시판</h1>
				</c:when>
				<c:when test="${category == 'Q'}">
					<h1 class="font-weight-semi-bold text-uppercase mb-3">질문 게시판</h1>
				</c:when>
				<c:when test="${category == 'S'}">
					<h1 class="font-weight-semi-bold text-uppercase mb-3">공지사항</h1>
				</c:when>
			</c:choose>
			<div class="d-inline-flex">
				<p class="m-0">
					<a href="/">Home</a>
				</p>
			</div>
		</div>
	</div>
	<div class="container">
		<div style="height: 550px">
			<div class="row">
				<table class="table" style="text-align: center; border: 1px solid #dddddd">
					<thead>
						<tr>
							<th style="background-color: #eeeeee; text-align: center;">번호</th>
							<th style="background-color: #eeeeee; text-align: center;">분류</th>
							<th style="background-color: #eeeeee; text-align: center; width: 40%;">제목</th>
							<th style="background-color: #eeeeee; text-align: center;">작성자</th>
							<th style="background-color: #eeeeee; text-align: center;">작성일</th>
							<th style="background-color: #eeeeee; text-align: center;">조회수</th>
						</tr>
					</thead>
					<c:forEach items="${list}" var="list">
						<tr class="record">
							<td><c:out value="${list.num}" /></td>
							<td><c:choose>
									<c:when test="${list.category == 'F'}">[자&nbsp;&nbsp;&nbsp;유]</c:when>
									<c:when test="${list.category == 'Q'}">[질&nbsp;&nbsp;&nbsp;문]</c:when>
									<c:when test="${list.category == 'S'}">[공지사항]</c:when>
								</c:choose></td>
							<td style="text-align: left;"><a class="move" href='<c:out value="${list.num}"/>'>
									<c:out value="${list.title}" />
									(
									댓글 갯수 영역
									)
								</a></td>
							<td><c:out value="${list.userid}" /></td>
							<td><fmt:formatDate pattern="yyyy/MM/dd" value="${list.writedate}" /></td>
							<td><c:out value="${list.readcount}" /></td>
						</tr>
					</c:forEach>
				</table>
			</div>
		</div>
		<div>
			<c:if test="${not empty user.userid}">
				<a href="/board/enroll" class="btn btn-primary pull-right" style="color: white;">게시판 등록</a>
			</c:if>
			<c:if test="${empty user.userid}">
				<a href="#" onclick="alert('로그인 후에 이용이 가능합니다.'); return false;" class="btn btn-primary pull-right" style="color: white;">게시판 등록</a>
			</c:if>
		</div>
	</div>
	<hr>
	<script>
		$(document).ready(function() {

			let result = '<c:out value="${result}"/>';

			checkAlert(result);

			function checkAlert(result) {

				if (result === '') {
					return;
				}

				if (result === "enrol success") {

			}

		});

	</script>
</body>
<jsp:include page="../footer.jsp"></jsp:include>
</html>

```

![_config.yml]({{ site.baseurl }}/img/SpringDB/boardlist.png)

### 페이징 기능

페이징 기법은 수많은 자료 데이터(행, 레코드)를 일정 크기로 나누어, 나누어진 하나하나의 집단에 페이지 번호를 부여하는 방식이다. 

정해진 갯수와 원하는 영역의 데이터를 불러오고 출력한다면 '가독성'과 '자원 낭비' 의 문제점이 보완될 것이다.

#### Rownum

Rownum은 모든 SQL에 그대로 삽입해서 사용할 수 있는 가상의 컬럼이다. 해당 컬럼의 값은 SQL이 실행되는 과정에서 발생하는 행의 일련번호다. 쉽게 말하면 Select를 통해 출력되는 결과 테이블에 행(row) 번호를 매기는 기능이다.

이러한 행 번호를 기준으로 '게시판 개수'와 '페이지 시작 번호' 데이터를 활용하여 페이징 처리를 할 수 있다.

#### com.db.model 에 Criteria.java 추가

페이징 쿼리를 동적 제어하기 위해 필요한 데이터 'pageNum' 과 'amount'를 같이 파라미터로 전달하기 위한 용도로 Criteria 클래스를 작성한다. 

각각의 데이터를 분리하여 파라미터로 전달해도 되지만 연관성 있는 데이터를 같이 관리함으로써 관리하기 편하고 재사용성의 장점이 있다.

```java
package com.db.model;

import java.util.Arrays;

public class Criteria {

	private int pageNum;

	private int amount;

	public Criteria() {
		this(1, 10);
	}

	public Criteria(int pageNum, int amount) {

		this.pageNum = pageNum;
		this.amount = amount;
	}

	public int getPageNum() {
		return pageNum;
	}

	public void setPageNum(int pageNum) {
		this.pageNum = pageNum;
	}

	public int getAmount() {
		return amount;
	}

	public void setAmount(int amount) {
		this.amount = amount;
	}

	@Override
	public String toString() {
		return "Criteria [pageNum=" + pageNum + ", amount=" + amount + "]";
	}

}

```

'현재 페이지' 와 '게시물 개수' 변수(pageNum, amount)를 선언

파라미터 없이 Criteria 클래스를 호출했을때 기본적으로 pageNum은 1, amount는 10을 가지도록 생성자를 작성

파라미터와 함께 Criteria를 호출하게 되면 파라미터의 값이 각각 pageNum과 amout 값에 저장되도록 생성자를 작성

#### BoardMapper.java 에 추가

```java
	//게시판 목록(페이징)
	public List<BoardVO>getListPaging(Criteria cri);
```

pageNum, amount의 정보를 DB에 전달하기 위해 Criteria 클래스를 파라미터로 부여한다.

#### BoardMapper.xml 에 추가

```xml
	<!-- 게시물 목록(페이징) -->
	<select id="getListPaging" resultType="com.db.model.BoardVO"> 
	
 	<![CDATA[ -->
		
		select num, title, content, userid, writedate, readcount, category from( 
		
	        select /*+INDEX_ASC(free_board idx_free_board_num_desc) */ rownum  as rn, num, title, content, userid, readcount, writedate, category
		          
 		        from free_board where rownum <= #{pageNum} * #{amount})  

			        
 		where rn > (#{pageNum} -1) * #{amount} 
	
 	]]>
```

#### BoardService.java 에 추가

```java
		//게시판 목록(페이징)
		public List<BoardVO>getListPaging(Criteria cri);
```

#### BoardServiceImpl.java 에 추가

```java
	@Override
	public List<BoardVO> getListPaging(Criteria cri) {

	    List<BoardVO> list = mapper.getListPaging(cri); //게시물 목록을 불러옴

	    return list;
	}
```

#### BoardController.java 에 수정

```java
	// 게시판 목록 페이지 접속(페이징)
	@GetMapping("/list")
	public void boardListGET(Model model, Criteria cri) {

		log.info("boardListGET");

		model.addAttribute("list", bservice.getListPaging(cri));

	}
```

기존 메서드에서 Criteria 클래스를 파라미터로 추가하고, getListPaging 메서드를 사용한다.

#### com.db.mapper 에 PageMakerDTO 추가

페이지 이동 언터페이스를 구현을 목표로 3가지 정보가 필요로 한다.

화면에 표시되는 페이지 번호 / 현재 보이는 페이지의 '이전 페이지', '다음 페이지' / 현재 페이지 표시

위의 3가지 정보를 서버에서 Model에 담아 뷰로 전송 후 뷰가 이를 활용해 화면에 표시하도록 한다.

위의 3가지 정보를 한 번에 각 단계끼리 교환 될 수 있도록 클래스에 정의하여 사용한다.

```java
package com.db.mapper;

import com.db.model.Criteria;

public class PageMakerDTO {

	// 시작
	private int startPage;

	// 끝
	private int endPage;
	// 이전 or 다음 페이지 존재유무
	private boolean prev, next;
	// 전체 게시물 수
	private int total;
	// 페이지와 페이지 게시물 표시수 정보
	private Criteria cri;

	// 생성자
	public PageMakerDTO(Criteria cri, int total) {

		this.cri = cri;
		this.total = total;
		// 마지막
		this.endPage = (int) (Math.ceil(cri.getPageNum() / 10.0)) * 10;

		// 시작
		this.startPage = this.endPage - 9;

		// 전체 마지막
		int realEnd = (int) (Math.ceil(total * 1.0 / cri.getAmount()));

		// 전체 마지막페이지 값이 마지막페이지(10) 보다 작은경우 보이는 페이지 값으로 조정
		if (realEnd < this.endPage) {
			this.endPage = realEnd;
		}
		// 시작페이지 값이 1보다 큰 경우 true
		this.prev = this.startPage > 1;
		// 마지막페이지 값이 1보다 큰 경우 true
		this.next = this.endPage < realEnd;

	}

	public int getStartPage() {
		return startPage;
	}

	public void setStartPage(int startPage) {
		this.startPage = startPage;
	}

	public int getEndPage() {
		return endPage;
	}

	public void setEndPage(int endPage) {
		this.endPage = endPage;
	}

	public boolean isPrev() {
		return prev;
	}

	public void setPrev(boolean prev) {
		this.prev = prev;
	}

	public boolean isNext() {
		return next;
	}

	public void setNext(boolean next) {
		this.next = next;
	}

	public int getTotal() {
		return total;
	}

	public void setTotal(int total) {
		this.total = total;
	}

	public Criteria getCri() {
		return cri;
	}

	public void setCri(Criteria cri) {
		this.cri = cri;
	}

	@Override
	public String toString() {
		return "PageMakerDTO [startPage=" + startPage + ", endPage=" + endPage + ", prev=" + prev + ", next=" + next
				+ ", total=" + total + ", cri=" + cri + "]";
	}

}

```

#### BoardMapper.java 에 추가

PageMakerDTO를 활용하기 위해서는 Criteria 클래스의 데이터와 게시물 전체 개수 데이터가 필요하다.

```java
	// 게시판 총 갯수
	public int getTotal(Criteria cri);	
```

#### BoardMapper.xml 에 추가

```xml
	<!-- 게시물 총 갯수 -->
	<select id="getTotal" resultType="int">

		select count(*) from free_board

	</select>
```

#### BoardService.java 에 추가

```java
		// 게시판 총 갯수
		public int getTotal(Criteria cri);	
```

#### BoardServiceImpl.java 에 추가

```java
	@Override
	public int getTotal(Criteria cri) {
		
		return mapper.getTotal(cri);
	}
```

#### BoardController.java 에 수정

```java
	// 게시판 목록 페이지 접속(페이징)
	@GetMapping("/list")
	public void boardListGET(HttpServletRequest request, Model model, Criteria cri) {

		log.info("boardListGET");

		model.addAttribute("list", bservice.getListPaging(cri));

		int total = bservice.getTotal(cri);
		
		request.setAttribute("category", category); // 받은 category 로 어떤 게시판인지 표시해줌
		
		PageMakerDTO pageMake = new PageMakerDTO(cri, total);

		model.addAttribute("pageMaker", pageMake);
	}
```

PageMakerDTO 클래스의 데이터를 뷰로 보내기 위해 addAttribute 메서드를 활용해 "pageMaker" 속성에 저장한다.

#### list.jsp 에 수정

```jsp
		<!-- 페이징 -->
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
			</form>
		</div>
	</div>
<script>
		let moveForm = $("#moveForm");

		$(".move").on(
				"click",
				function(e) {
					e.preventDefault();

					moveForm.append("<input type='hidden' name='num' value='"
							+ $(this).attr("href") + "'>");
					moveForm.attr("action", "/board/get");
					moveForm.submit();
				});

		$("#pageInfo a").on("click", function(e) {
			e.preventDefault();
			moveForm.find("input[name='pageNum']").val($(this).attr("href"));
			moveForm.attr("action", "/board/list");
			moveForm.submit();

		});
</script>
```

![_config.yml]({{ site.baseurl }}/img/SpringDB/bpage.png)

## [프로젝트 주소](https://github.com/GreenteaPIE/TeamProjectDBSpringVer)
