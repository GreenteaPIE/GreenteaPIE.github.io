---
layout: post
title: 14 - DB Spring 게시판 검색
date: 2023-05-24
excerpt: "팀 프로젝트 DB Spring Version"
tags: [project, java, jsp, Oracle, css, HTML, BootStrap, API, JQuery, JavaScript, Spring, FrameWork]
feature: /img/SpringDB/logo.png
comments: true


---


> **사용한 플랫폼 : Spring, Oracle**



### 1.  게시판 검색 기능

사용자가 검색하고자 하는 키워드를 서버에 전송하고 서버는 전달받은 키워드 데이터를 DB 까지 전송하여 DB에서 해당 키워드를 조건문으로 하는 쿼리를 실행 후 반환된 결과를 다시 서버를 거쳐 뷰에 전송되어 출력되게 된다.

핵심은 쿼리의 경우 전달받은 키워드를 통해서 조건을 부여하여 필터링된 게시물 결과를 출력해야 한다는 점과 뷰단에서 사용자가 원하는 키워드를 입력하고 검색 할 수 있는 인터페이스를 제공해야 한다는 점이다.

#### Criteria.java 에 수정

```java
package com.db.model;

import java.util.Arrays;

public class Criteria {

	private int pageNum;

	private int amount;

	private String keyword;

	private String type;

	private String[] typeArr;

	private String category;

	private String[] categoryArr;

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

	public String getKeyword() {
		return keyword;
	}

	public void setKeyword(String keyword) {
		this.keyword = keyword;
	}

	public String getType() {
		return type;
	}

	public void setType(String type) {
		this.type = type;
		this.typeArr = type.split("");
	}

	public String[] getTypeArr() {
		return typeArr;
	}

	public void setTypeArr(String[] typeArr) {
		this.typeArr = typeArr;
	}

	public String getCategory() {
		return category;
	}

	public void setCategory(String category) {
		this.category = category;
	}

	public String[] getCategoryArr() {
		return categoryArr;
	}

	public void setCategoryArr(String[] categoryArr) {
		this.categoryArr = categoryArr;
	}

	@Override
	public String toString() {
		return "Criteria [pageNum=" + pageNum + ", amount=" + amount + ", keyword=" + keyword + ", type=" + type
				+ ", typeArr=" + Arrays.toString(typeArr) + ", category=" + category + ", categoryArr="
				+ Arrays.toString(categoryArr) + "]";
	}

}

```

#### BoardMapper.xml 에 추가

```jsp
<!-- 검색 조건문 -->
	<sql id="criteria">
		<trim prefix="AND (" suffix=")" prefixOverrides="OR">
			<foreach collection="typeArr" item="type">
				<trim prefix="OR">
					<choose>
						<when test="type == 'T'.toString()">
							title like '%'||#{keyword}||'%'
						</when>
						<when test="type == 'C'.toString()">
							content like '%'||#{keyword}||'%'
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
		<if
			test="category != null and (category == 'F'.toString() or category == 'S'.toString() or category == 'Q'.toString())">
			AND category = #{category}
		</if>

	</sql>

```

검색 조건에 따라 데이터베이스 쿼리를 동적으로 구성한다.

1. 검색 조건

   `foreach`를 사용하여 `typeArr` 컬렉션의 각 요소를 검색 조건으로 사용할 수 있다. 'T', 'C', 'W', 'M' 등의 값에 따라 다음 조건들이 처리된다

   - 'T'로 설정된 경우, 제목(title)에서 키워드를 검색한다.
   - 'C'로 설정된 경우, 내용(content)에서 키워드를 검색한다.
   - 'W'로 설정된 경우, 작성자 아이디(userid)에서 키워드를 검색한다.
   - 'M'으로 설정된 경우, 작성자 아이디(userid)가 정확한 검색 키워드와 일치하는지 확인한다.

2. 카테고리 검색 처리

   - 검색할 때 추가적인 카테고리 필터를 사용하여 검색 결과를 다룬다. `if` 테스트는 'category' 값이 존재하고, 'F', 'S', 'Q' 중 하나인 경우에만 적용된다. 이 조건이 만족되면, 쿼리에 `AND category = #{category}`가 추가되어 카테고리로만 결과를 필터링한다.

```xml
		<!-- 게시물 목록(페이징) -->
	<select id="getListPaging" resultType="com.db.model.BoardVO"> 
	
 	<![CDATA[ -->
		
		select num, title, content, userid, writedate, readcount, category from( 
		
	        select /*+INDEX_ASC(free_board idx_free_board_num_desc) */ rownum  as rn, num, title, content, userid, readcount, writedate, category
		          
 		        from free_board where rownum <= #{pageNum} * #{amount}  
 	]]>
		<if test="keyword != null">
			<include refid="criteria"></include>
		</if> 
	
 	<![CDATA[ 
			        
 			    ) 
			        
 		where rn > (#{pageNum} -1) * #{amount} 
	
 	]]>
 	</select>
 	
 		<!-- 게시물 총 갯수 -->
	<select id="getTotal" resultType="int">

		select count(*) from free_board

		<if test="keyword != null">

			where num > 0
			<include refid="criteria"></include>

		</if>

	</select>
	
```

<if> 태그를 통해 keyword데이터가 넘어올 시엔 실행이 되는 where 조건문을 작성한다.

#### list.jsp 에 수정

```jsp
		<div class="search_wrap">
			<div class="search_area">
				<select name="type">
					<option value="" <c:out value="${pageMaker.cri.type == null?'selected':'' }"/>>--</option>
					<option value="T" <c:out value="${pageMaker.cri.type eq 'T'?'selected':'' }"/>>제목</option>
					<option value="C" <c:out value="${pageMaker.cri.type eq 'C'?'selected':'' }"/>>내용</option>
					<option value="W" <c:out value="${pageMaker.cri.type eq 'W'?'selected':'' }"/>>작성자</option>
					<option value="TC" <c:out value="${pageMaker.cri.type eq 'TC'?'selected':'' }"/>>제목 + 내용</option>
					<option value="TW" <c:out value="${pageMaker.cri.type eq 'TW'?'selected':'' }"/>>제목 + 작성자</option>
					<option value="TCW" <c:out value="${pageMaker.cri.type eq 'TCW'?'selected':'' }"/>>제목 + 내용 + 작성자</option>
				</select>
				<input type="text" name="keyword" value="${pageMaker.cri.keyword}">
				<button class="btn btn-primary pull-right">Search</button>
			</div>
		</div>

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
				<input type="hidden" name="keyword" value="${pageMaker.cri.keyword}">
				<input type="hidden" name="type" value="${pageMaker.cri.type}">
				<input type="hidden" name="category" value="${pageMaker.cri.category}">
			</form>
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

![_config.yml]({{ site.baseurl }}/img/SpringDB/bsearch.png)

#### header.jsp 에 수정

```jsp
								<div class="dropdown-menu rounded-0 m-0">
									<a href="/board/list?pageNum=1&amount=10&category=F&keyword=&type=" class="dropdown-item">자유 게시판</a>
									<a href="/board/list?pageNum=1&amount=10&category=Q&keyword=&type=" class="dropdown-item">질문 게시판</a>
									<a href="/board/list?pageNum=1&amount=10&category=S&keyword=&type=" class="dropdown-item">공지사항</a>
								</div>
```

![_config.yml]({{ site.baseurl }}/img/SpringDB/boardcate.png)

## [프로젝트 주소](https://github.com/GreenteaPIE/TeamProjectDBSpringVer)
