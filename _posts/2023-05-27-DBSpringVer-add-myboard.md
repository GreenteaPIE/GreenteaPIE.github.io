---
layout: post
title: 17 - DB Spring 나의 작성 글
date: 2023-05-27
excerpt: "팀 프로젝트 DB Spring Version"
tags: [project, java, jsp, Oracle, css, HTML, BootStrap, API, JQuery, JavaScript, Spring, FrameWork]
feature: /img/SpringDB/logo.png
comments: true


---


> **사용한 플랫폼 : Spring, Oracle**



### 1.  나의 작성 글 확인 기능

#### UserController.java 에 추가

```java
		@Autowired
	private BoardService bservice;

	// 내가 쓴글 확인
	@GetMapping("/myWriting")
	public void myWritingGET(HttpServletRequest request, Model model, Criteria cri,
			@RequestParam(required = false) String category) {

		cri.setCategory(category); // category 값을 저장합니다.

		model.addAttribute("list", bservice.getListPaging(cri));

		int total = bservice.getTotal(cri);

		PageMakerDTO pageMake = new PageMakerDTO(cri, total);

		model.addAttribute("pageMaker", pageMake);
	}
```

#### header.jsp 수정

```jsp
											<a href="/user/myWriting?pageNum=1&amount=10&type=M&keyword=${user.userid }" class="dropdown-item">나의 작성 글</a>

```

게시판 목록을 가지고 오기 위해 만들었던 getListPaging 메서드를 사용하여 키워드 값으로 세션에 있는 userid를 가지고 게시물을 불러온다.

![_config.yml]({{ site.baseurl }}/img/SpringDB/mywrite.png)

## [프로젝트 주소](https://github.com/GreenteaPIE/TeamProjectDBSpringVer)
