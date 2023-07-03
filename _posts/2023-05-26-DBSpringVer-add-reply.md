---
layout: post
title: 16 - DB Spring 게시판 댓글
date: 2023-05-26
excerpt: "팀 프로젝트 DB Spring Version"
tags: [project, java, jsp, Oracle, css, HTML, BootStrap, API, JQuery, JavaScript, Spring, FrameWork]
feature: /img/SpringDB/logo.png
comments: true


---


> **사용한 플랫폼 : Spring, Oracle**



### 1.  게시판 댓글 등록 기능

#### com.db.model 에 FBoardReplyVO.java 추가

```java
package com.db.model;

import java.sql.Timestamp;

import lombok.Data;

@Data
public class FBoardReplyVO {

	private int rno;
	private int num;
	private String writer;
	private String content;
	private Timestamp regDate;

}

```

#### ReplyMapper.java 추가

```java
package com.db.mapper;

import com.db.model.FBoardReplyVO;

public interface ReplyMapper {

	// 댓글 작성
	public void replyWriter(FBoardReplyVO vo)throws Exception;
}

```

#### ReplyMapper.xml 추가

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.db.mapper.ReplyMapper">

	<!-- 댓글 작성 -->
	<insert id="replyWriter"
		parameterType="com.db.model.FBoardReplyVO">
		INSERT INTO fboard_reply(rno, num, writer, content, regDate)
		VALUES(freply_seq.NEXTVAL, #{num}, #{writer}, #{content},
		SYSTIMESTAMP)
	</insert>

</mapper>
```

#### ReplyService.java 추가

```java
package com.db.service;

import com.db.model.FBoardReplyVO;

public interface ReplyService {

	// 댓글 작성
	public void replyWriter(FBoardReplyVO vo) throws Exception;
}

```

#### ReplyServiceImpl.java 추가

```java
package com.db.service;

import org.springframework.beans.factory.annotation.Autowired;

import com.db.mapper.ReplyMapper;
import com.db.model.FBoardReplyVO;

public class ReplyServiceImpl implements ReplyService{
	
	@Autowired
	private ReplyMapper mapper;
	
	@Override
	public void replyWriter(FBoardReplyVO vo) throws Exception {
		mapper.replyWriter(vo);
	}
}
```

#### ReplyController.java 추가

```java
package com.db.controller;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestMapping;

import com.db.model.FBoardReplyVO;
import com.db.service.ReplyService;

@Controller
@RequestMapping("/reply/*")
public class ReplyController {
	@Autowired
	private ReplyService service;
	
	// 댓글 작성
	@PostMapping("/write")
	public String postWirte(FBoardReplyVO vo) throws Exception {

		service.replyWriter(vo);

		return "redirect:/board/get?num=" + vo.getNum();
	}


}
```

### 2. 댓글 조회 기능

#### ReplyMapper.java 에 추가

```java
	// 댓글 조회
	public List<FBoardReplyVO> replyList(int num)throws Exception;
```

#### ReplyMapper.xml 에 추가

```xml
	<!-- 댓글 조회 -->
	<select id="replyList" parameterType="int"
		resultType="com.db.model.FBoardReplyVO">
		select
		rno, num, writer, content, regDate
		from fboard_reply
		where num = #{num}
	</select>
```

게시판 번호로 DB에서 작성된 댓글들을 불러온다.

#### ReplyService.java 에 추가

```java
	// 댓글 조회
	public List<FBoardReplyVO> replyList(int num) throws Exception;
```

#### ReplyServiceImpl.java 에 추가

```java
	@Override
	public List<FBoardReplyVO> replyList(int bno) throws Exception {
		return mapper.replyList(bno);
	}
```

#### BoardController.java 에 수정

```java
	@Inject
	private ReplyService replyService; 

    // 게시판 조회
	@GetMapping("/get")
	public void boardGetPageGET(@RequestParam("num") int num, Model model, Criteria cri) throws Exception {
		bservice.updateReadCount(num); //조회수 업데이트
		log.info("게시글번호:" + num);
		model.addAttribute("pageInfo", bservice.getPage(num));
		model.addAttribute("cri", cri);

		// 댓글 조회
		List<FBoardReplyVO> reply = null;
		reply = replyService.replyList(num);
		model.addAttribute("reply", reply);

	}
```

게시판을 조회 했을때 reply에 댓글 정보를 저장한다.

#### BoardMapper.java 에 추가

게시판 리스트와 조회 페이지에 해당 게시판의 댓글 수를 불러오기 위한 메서드를 만들어 준다.

```java
	// 댓글 수
	public int getReplyCount(int num);
```

#### BoardMapper.xml 에 추가

```xml
	<!-- 댓글 수 -->
	<select id="getReplyCount" parameterType="int" resultType="int">
		SELECT COUNT(*) FROM fboard_reply WHERE num = #{num}
	</select>
```

#### BoardService.java 에 추가

```java
		//댓글 수 
		public int getReplyCount(int num);
```

#### BoardServiceImpl.java 에 추가

```java
	@Override
	public int getReplyCount(int num) {
		return mapper.getReplyCount(num);
	}
```

아래는 getListPaging 쿼리가 실행 될 때 getReplyCount 메서드를 호출하여 게시판에 해당 하는 댓글의 개수를 가져오게 수정한다.

```java
	@Override
	public List<BoardVO> getListPaging(Criteria cri) {
	    // 1. 게시물 목록을 가져옴
	    List<BoardVO> list = mapper.getListPaging(cri);

	    // 2. 각 게시물에 대해 댓글 개수를 가져와서 replyCount를 설정함
	    for (BoardVO boardVO : list) {
	        int replyCount = mapper.getReplyCount(boardVO.getNum()); // 댓글 개수를 가져오는 새로운 메소드 호출
	        boardVO.setReply_count(replyCount); 
	    }

	    return list;
	}
```

#### BoardController.java 에 수정

```java
	// 게시판 조회
	@GetMapping("/get")
	public void boardGetPageGET(@RequestParam("num") int num, Model model, Criteria cri) throws Exception {
		bservice.updateReadCount(num); //조회수 업데이트
		log.info("게시글번호:" + num);
		model.addAttribute("pageInfo", bservice.getPage(num));
		model.addAttribute("cri", cri);
		model.addAttribute("reply_Count", bservice.getReplyCount(num));

		// 댓글 조회
		List<FBoardReplyVO> reply = null;
		reply = replyService.replyList(num);
		model.addAttribute("reply", reply);

	}
```

게시판 조회 페이지도 마찬가지로 댓글 개수를 불러올 수 있게 getReplyCount 메서드를 추가 해준다.

#### list.jsp 수정

```jsp
							<td style="text-align: left;"><a class="move" href='<c:out value="${list.num}"/>'>
									<c:out value="${list.title}" />
									(
									<c:out value="${list.reply_count}" />
									)
								</a></td>
```

#### get.jsp 수정

```jsp
		<ol>
			<c:forEach items="${reply}" var="reply">
				<li>
					<div>
						<div class="d-flex justify-content-between">
							<p>${reply.writer}</p>
							<p>
								<fmt:formatDate value="${reply.regDate}" pattern="yyyy. MM. dd. HH:mm" />
							</p>
						</div>
						<p>${reply.content }</p>
						<c:if test="${reply.writer eq user.userid or user.userid eq 'admin'}">
							<p class="text-right">
								<a href="#" onclick="openModifyWindow('${pageInfo.num}', '${reply.rno}')" class="text-decoration-none">
									<button type="button" class="btn btn-sm btn-primary text-right">수정</button>
								</a>
								<a href="/reply/delete?num=${pageInfo.num}&rno=${reply.rno}" class="text-decoration-none">
									<button type="button" class="btn btn-sm btn-primary text-right">삭제</button>
								</a>
							</p>
						</c:if>
						<hr />
					</div>
				</li>
			</c:forEach>
		</ol>
		<div>
			<form method="post" action="/reply/write" class="mt-3">
				<div class="row mb-3">
					<div class="col">
						<label for="writer" class="form-label">댓글 작성자</label>
						<input type="hidden" class="form-control" name="writer" id="writer" value="${user.userid}">
						<span>${user.userid}</span>
						<c:if test="${empty user.userid}">
							<b>로그인 필요</b>
						</c:if>
					</div>
				</div>
				<div class="row mb-3">
					<div class="col">
						<label for="content" class="form-label">댓글 내용</label>
						<textarea id="content" name="content" class="form-control" rows="5"></textarea>
					</div>
				</div>
				<div class="row mb-3">
					<div class="col">
						<c:if test="${not empty user.userid}">
							<input type="hidden" name="num" value="${pageInfo.num}">
							<button type="submit" class="btn btn-primary">댓글 작성</button>
						</c:if>
						<c:if test="${empty user.userid}">
							<input type="hidden" name="num" value="${pageInfo.num}">
							<button onclick="alert('로그인 후에 이용이 가능합니다.'); return false;" class="btn btn-primary">댓글 작성</button>
						</c:if>
					</div>
				</div>
			</form>
		</div>

<script>
		function openModifyWindow(num, rno) {
			let url = "/reply/modify?num=" + num + "&rno=" + rno;
			window.open(url, "modifyWindow", "width=400, height=300");
		}
</script>
```

![_config.yml]({{ site.baseurl }}/img/SpringDB/breply.png)

### 3. 댓글 수정 기능

자신이 작성한 하나의 단일 댓글을 수정 하기 위해서는 해당 댓글의 번호를 찾는 replySelect 메서드와 댓글을 수정하는 modify 메서드를 만들어준다.

#### ReplyMapper.java 에 추가

```java
	// 단일 댓글 조회
	public FBoardReplyVO replySelect(FBoardReplyVO vo) throws Exception;
	
	// 댓글 수정
	public void replyModify(FBoardReplyVO vo)throws Exception;
```

#### ReplyMapper.xml 에 추가

```xml
	<!-- 단일 댓글 조회 -->
	<select id="replySelect"
		parameterType="com.db.model.FBoardReplyVO"
		resultType="com.db.model.FBoardReplyVO">
		select
		rno, num, writer, content, regDate
		from fboard_reply
		where num = #{num}
		and rno = #{rno}
	</select>

	<!-- 댓글 수정 -->
	<update id="replyModify"
		parameterType="com.db.model.FBoardReplyVO">
		update fboard_reply set
		content = #{content}
		where rno = #{rno}
		and num = #{num}
	</update>
```

#### ReplyService.java 에 추가

```java
	// 단일 댓글 조회
	public FBoardReplyVO replySelect(FBoardReplyVO vo) throws Exception;
	
	// 댓글 수정
	public void replyModify(FBoardReplyVO vo) throws Exception;
```

#### ReplyServiceImpl.java 에 추가

```java
	@Override
	public FBoardReplyVO replySelect(FBoardReplyVO vo) throws Exception {
		
		return mapper.replySelect(vo);
	}
	
	@Override
	public void replyModify(FBoardReplyVO vo) throws Exception {
		mapper.replyModify(vo);
	}
```

#### ReplyController.java 에 추가

```java
	// 댓글 단일 조회(수정페이지)
	@GetMapping("/modify")
	public void getMofidy(@RequestParam("num") int num, @RequestParam("rno") int rno, Model model) throws Exception {

		FBoardReplyVO vo = new FBoardReplyVO();
		vo.setNum(num);
		vo.setRno(rno);

		FBoardReplyVO reply = service.replySelect(vo); //수정 버튼을 클릭한 댓글을 불러옴

		model.addAttribute("reply", reply);
	}
```

#### modify.jsp 추가

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>DIIB</title>
<!-- Libraries Stylesheet -->
<link href="../css/owl.carousel.min.css" rel="stylesheet">
<!-- Customized Bootstrap Stylesheet -->
<link href="../resources/css/style.css" rel="stylesheet">
<script src="https://code.jquery.com/jquery-3.4.1.js" integrity="sha256-WpOohJOqMqqyKL9FccASB9O0KwACQJpFTUBLTYOVvVU=" crossorigin="anonymous">
	
</script>
</head>
</head>
<body>
	<form method="post" action="/reply/modify">
		<div class="container">
			<div class="form-group">
				<label for="writer">댓글 작성자</label>
				<input type="text" class="form-control" name="writer" id="writer" readonly="readonly" value="${reply.writer}">
			</div>
			<div class="form-group">
				<label for="content">댓글 내용</label>
				<textarea class="form-control" rows="5" name="content" id="content">${reply.content}</textarea>
			</div>
			<div class="form-group">
				<input type="hidden" name="num" value="${reply.num}">
				<input type="hidden" name="rno" value="${reply.rno}">
			</div>
			<button type="submit" class="btn btn-primary">댓글 수정</button>
		</div>
	</form>
	<script>
		function closeWindowAndRefreshParent(ev) {
			ev.preventDefault();
			fetch('/reply/modify', {
				method : 'POST',
				headers : {
					'Content-Type' : 'application/x-www-form-urlencoded'
				},
				body : new URLSearchParams(new FormData(ev.target))
			}).then(function(response) {
				if (response.ok) {
					window.opener.location.reload();
					window.close();
				} else {
					alert('댓글 수정 실패');
				}
			});
		}

		document.querySelector("form").addEventListener("submit",
				closeWindowAndRefreshParent);
	</script>
</body>
</html>
```

![_config.yml]({{ site.baseurl }}/img/SpringDB/replymodi.png)

#### ReplyController.java 에 추가

```java
	// 댓글 수정
	@PostMapping("/modify")
	public String postModify(FBoardReplyVO vo) throws Exception {

		service.replyModify(vo);

		return "redirect:/board/get?num=" + vo.getNum();
	}
```

### 4. 댓글 삭제 기능

#### ReplyMapper.java 에 추가

```java
	// 댓글 삭제
	public void replyDelete(FBoardReplyVO vo)throws Exception;
```

#### ReplyMapper.xml 에 추가

```xml
	<!-- 댓글 삭제 -->
	<delete id="replyDelete"
		parameterType="com.db.model.FBoardReplyVO">
		delete fboard_reply
		where rno = #{rno}
		and num = ${num}
	</delete>
```

#### ReplyService.java 에 추가

```java
	// 댓글 삭제
	public void replyDelete(FBoardReplyVO vo) throws Exception;
```

#### ReplyServiceImpl.java 에 추가

```java
	@Override
	public void replyDelete(FBoardReplyVO vo) throws Exception {
		mapper.replyDelete(vo);
	}
```

#### ReplyController.java 에 추가

```java
	// 댓글 삭제
	@GetMapping("/delete")
	public String getDelete(FBoardReplyVO vo) throws Exception {

		service.replyDelete(vo);

		return "redirect:/board/get?num=" + vo.getNum();
	}
```

## [프로젝트 주소](https://github.com/GreenteaPIE/TeamProjectDBSpringVer)
