---
layout: post
title: 12 - DB Spring 게시판 등록
date: 2023-05-22
excerpt: "팀 프로젝트 DB Spring Version"
tags: [project, java, jsp, Oracle, css, HTML, BootStrap, API, JQuery, JavaScript, Spring, FrameWork]
feature: /img/SpringDB/logo.png
comments: true


---


> **사용한 플랫폼 : Spring, Oracle**



### 1.  게시판 등록 기능

먼저 게시판 등록 기능을 구현 하기 위해 등록 페이지로 이동시킬 버튼을 만들어 준다.

#### list.jsp 추가

```jsp
		<div>
			<c:if test="${not empty user.userid}">
				<a href="/board/enroll" class="btn btn-primary pull-right" style="color: white;">게시판 등록</a>
			</c:if>
			<c:if test="${empty user.userid}">
				<a href="#" onclick="alert('로그인 후에 이용이 가능합니다.'); return false;" class="btn btn-primary pull-right" style="color: white;">게시판 등록</a>
			</c:if>
		</div>
```

로그인이 되어있지 않은 사용자는 게시판을 등록할 수 없게 한다.

![_config.yml]({{ site.baseurl }}/img/SpringDB/boardupload.png)

#### com.db.model에 BoardVO 추가

```java
package com.db.model;

import java.sql.Timestamp;

import lombok.Data;

@Data
public class BoardVO {

	private String userid, title, content;
	private int readcount, num;
	private Timestamp writedate;
	private int reply_count;
	private String category;

}
```

#### BoardMapper.java 추가

게시판 등록 쿼리를 요청하는 enroll() 메서드 코드를 작성한다.

```java
package com.db.mapper;

import com.db.model.BoardVO;

public interface BoardMapper {
	//게시판 등록
		public void enroll(BoardVO board);
}
```

#### BoardMapper.xml 추가

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.db.mapper.BoardMapper">

</mapper>

```

xml 파일을 mapper로서의 기능을 하도록 만들기 위해 위와 같이 작성한다. 

가장 중요한 것은 namespace 속성 값을 앞에서 생성하고 작성한 Mapper 인터페이스의 경로를 포함하는 인터페이스의 이름으로 작성해야한다.

namespace 속성 값이 중요한 이유는 MyBatis가 Mapper 인터페이스와 XML을 인터페이스 이름과 namespace 속성 값을 가지고 판단하기 때문이다.

Mapper 인터페이스에서 선언한 enroll 메서드 호출될 시에 실행될 SQL문을 아래와 같이 작성한다. <insert> 태그에 id 속성 값을 메서드 이름과 동일하게 작성한다.

값이 들어가야 할 구문에는 Mapper 인터페이스에서 선언한 enroll() 메서드의 파라미터 BoardVO 클래스의 멤버 변수명을 #{}을 붙여 작성한다.

```XML
    <!-- 게시판 등록 -->
	<insert id="enroll">
		insert into free_board(num, title, content, userid,
		category) values (free_seq.nextval,
		#{title},#{content},#{userid},#{category})

	</insert>
```

#### BoardService.java 추가

```java
package com.db.service;

import com.db.model.BoardVO;

public interface BoardService {
	
	//게시판 등록
	public void enroll(BoardVO board);
}
```

#### BoardServiceImpl.java 추가

```java
package com.db.service;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import com.db.mapper.BoardMapper;
import com.db.model.BoardVO;

@Service
public class BoardServiceImpl implements BoardService {

	@Autowired
	private BoardMapper mapper;

	@Override
	public void enroll(BoardVO board) {
		mapper.enroll(board);

	}

}
```

스프링에서 해당 클래스가 Service 역할을 한다는 것을 인식 할 수 있다록 @Service 어노테이션을 추가한다.

#### BoardConroller.java 추가

```java
package com.db.controller;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;



@Controller
@RequestMapping("/board/*")
public class BoardController {

	private static final Logger log = LoggerFactory.getLogger(BoardController.class);


}
```

클래스 선언부에 2개의 어노테이션을 추가한다.<br>@Controller  어노테이션의 경우 해당 클래스를 스프링의 빈으로 인식하도록 하기 위함이고, @RequestMapping("/board/*")의 경우 '/board'로 시작하는 모든 처리를 BoardController.java 가 하도록 지정하는 역할을 한다. 

게시판 등록 페이지로 이동 할 수 있는 맵핑 메서드를 작성한다.

```java
	@GetMapping("/enroll")
	public void boardEnrollGET() {
		log.info("게시판 등록 페이지 진입");

	}
```

#### enroll.jsp 추가

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
    	<script src="//cdn.ckeditor.com/4.21.0/standard/ckeditor.js"></script>
</head>
<jsp:include page="../header.jsp"></jsp:include>
<body>
	<hr>
	<div class="container">
		<form action="/board/enroll" name="frm" method="post" enctype="multipart/form-data">
			<div class="row">
				<table class="table" style="text-align: center; border: 1px solid #dddddd">
					<thead>
						<tr>
							<th colspan="2" style="background-color: #eeeeee; text-align: center;">게시글 등록</th>
						</tr>
					</thead>
					<tbody>
						<tr>
							<td colspan="1"><select name="category" id="categorySelect">
									<option value="F">자유게시판</option>
									<option value="Q">질문게시판</option>
									<c:if test="${user.userid eq 'admin'}">
										<option value="S">공지사항</option>
									</c:if>
							</select></td>
							<td colspan="1">작성자  <input type="hidden" class="form-control" name="userid" value="${user.userid }">${user.userid }
							</td>
						</tr>
						<tr>
							<td colspan="2"><input type="text" class="form-control" placeholder="글 제목" name="title" maxlength="50"></td>
						</tr>
						<tr>
							<td colspan="2"><textarea id="content" name="content" class="form-control" placeholder="글 내용"></textarea></td>
						</tr>
					</tbody>
				</table>
			</div>
			<div style="margin-left: 30px;">
				<input type="button" class="btn btn-primary pull-right" value="등록" onclick="if(boardCheck()) go_fbw();">&nbsp;&nbsp;<input type="button" class="btn btn-primary pull-right" value="목록" onclick="history.back(-1)">
			</div>
		</form>
	</div>
	<hr>
    	<script type="text/javascript">
		// 글쓰기 editor 및 사진 업로드 기능
		CKEDITOR.replace('content', {
			filebrowserUploadUrl : '/board/imageUpload.do',
			height : 500
		// Set the desired height value here
		});
	</script>
</body>
<jsp:include page="../footer.jsp"></jsp:include>
</html>
```

![_config.yml]({{ site.baseurl }}/img/SpringDB/uploadpage.png)

ckeditor 스크립트는 글 편집기로 홈페이지에서 가장 많이 사용되고 있다고 한다.

#### BoardController.java 에 추가

이미지 업로드를 위한 imageUpload 메서드를 만들어 준다.

```java
	// 이미지 업로드
	@RequestMapping(value = "/imageUpload.do", method = RequestMethod.POST)
	public void imageUpload(HttpServletRequest request, HttpServletResponse response,
			MultipartHttpServletRequest multiFile, @RequestParam MultipartFile upload) throws Exception {
		// 랜덤 문자 생성
		UUID uid = UUID.randomUUID();

		OutputStream out = null;
		PrintWriter printWriter = null;

		// 인코딩
		response.setCharacterEncoding("utf-8");
		response.setContentType("text/html;charset=utf-8");
		try {
			// 파일 이름 가져오기
			String fileName = upload.getOriginalFilename();
			byte[] bytes = upload.getBytes();

			// 이미지 경로 생성
			String path = "C:\\upload\\Pictures\\Saved Pictures" + "ckImage/"; // 이미지 경로 설정(폴더 자동 생성)
			String ckUploadPath = path + uid + "_" + fileName;
			File folder = new File(path);
			System.out.println("path:" + path); // 이미지 저장경로 console에 확인
			// 해당 디렉토리 확인
			if (!folder.exists()) {
				try {
					folder.mkdirs(); // 폴더 생성
				} catch (Exception e) {
					e.getStackTrace();
				}
			}

			out = new FileOutputStream(new File(ckUploadPath));
			out.write(bytes);
			out.flush(); // outputStram에 저장된 데이터를 전송하고 초기화

			String callback = request.getParameter("CKEditorFuncNum");
			printWriter = response.getWriter();  // 서버에 요청
			String fileUrl = "/board/ckImgSubmit.do?uid=" + uid + "&fileName=" + fileName; // 작성화면

			// 업로드시 메시지 출력
			printWriter.println("{\"filename\" : \"" + fileName + "\", \"uploaded\" : 1, \"url\":\"" + fileUrl + "\"}");
			printWriter.flush();

		} catch (IOException e) {
			e.printStackTrace();
		} finally {
			try {
				if (out != null) {
					out.close();
				}
				if (printWriter != null) {
					printWriter.close();
				}
			} catch (IOException e) {
				e.printStackTrace();
			}
		}
		return;
	}
```

이미지 파일을 받고, UUID를 사용하여 고유한 랜덤 문자열을 생성 후, 이미지 파일 경로 및 저장 위치를 설정한다. 폴더가 존재하지 않으면 폴더를 생성하고, 받은 이미지를 파일로 저장 후 경로를 포함한 메시지를 반환한다.

> UUID란? 범용 고유 식별자를 의미하고, 중복이 되지않는 유일한 값을 구성하고자 할때 주로 사용한다.

```java
	// 서버로 전송된 이미지 뿌려주기
	@RequestMapping(value = "/board/ckImgSubmit.do")
	public void ckSubmit(@RequestParam(value = "uid") String uid, @RequestParam(value = "fileName") String fileName,
			HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

		// 서버에 저장된 이미지 경로
		String path = "C:\\upload\\Pictures\\Saved Pictures" + "ckImage/"; // 저장된 이미지 경로
		System.out.println("path:" + path);
		String sDirPath = path + uid + "_" + fileName;

		File imgFile = new File(sDirPath);

		// 사진 이미지 찾지 못하는 경우 예외처리로 빈 이미지 파일을 설정한다.
		if (imgFile.isFile()) {
			byte[] buf = new byte[1024];
			int readByte = 0;
			int length = 0;
			byte[] imgBuf = null;

			FileInputStream fileInputStream = null;
			ByteArrayOutputStream outputStream = null;
			ServletOutputStream out = null;

			try {
				fileInputStream = new FileInputStream(imgFile);
				outputStream = new ByteArrayOutputStream();
				out = response.getOutputStream();

				while ((readByte = fileInputStream.read(buf)) != -1) {
					outputStream.write(buf, 0, readByte);
				}

				imgBuf = outputStream.toByteArray();
				length = imgBuf.length;
				out.write(imgBuf, 0, length);
				out.flush();

			} catch (IOException e) {
				e.printStackTrace();
			} finally {
				outputStream.close();
				fileInputStream.close();
				out.close();
			}
		}
	}
```

이미지를 저장된 서버에서 파일을 찾아 불러오는 ckSubmit 메서드를 만들어준다. 업로드한 파일 경로를 찾고, 그 경로에 있는 이미지를 읽어 들인 후, ServletOutPutStream을 사용해 이미지를 보여준다.

이미지 파일을 찾지 못할 경우, 빈 이미지 파일을 설정해 예외 처리를 한다.

#### freeboard.js 추가

```js
function go_fbw()
{

var theForm = document.frm
theForm.action = "/board/enroll";
CKEDITOR.instances.content.updateElement();
theForm.submit();
}

function go_fbu()
{
var theForm = document.frm
theForm.action = "DBServlet?command=free_board_update";
theForm.submit();
}
function deleteBoard(num) {
  if (confirm("게시글을 삭제하시겠습니까?")) {
    location.href = 'DBServlet?command=freeboard_delete&num=' + num;
  }
}

function boardCheck(){
	   if (CKEDITOR.instances.content.getData().length == 0) {
		alert("내용 입력하세요.");
		return false;
	}
	if(document.frm.title.value.length ==0){
		alert("제목을 입력하세요.")
		return false;
	}
	return true;
}
```

게시글 삭제를 한번 더 확인 하고, 게시글의 제목과 내용이 비어있으면 form이 보내지지 않게 만든다.

#### enroll.jsp 에 추가

```jsp
<script type="text/javascript" src="../js/freeboard.js"></script>
```

#### BoardController.java 에 추가

```java
// 게시판 등록
	@PostMapping("/enroll")
	public String boardEnrollPOST(BoardVO board, RedirectAttributes rttr, Criteria cri) {
		log.info("BoardVO : " + board);

		bservice.enroll(board);
        
        return "redirect:/board/list";
	}
```

등록이 완료되면 게시판 리스트로 redirect 한다.

## [프로젝트 주소](https://github.com/GreenteaPIE/TeamProjectDBSpringVer)
