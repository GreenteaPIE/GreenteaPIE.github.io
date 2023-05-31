---
layout: post
title: DB Spring -2 MainPage 제작
date: 2023-05-15
excerpt: "팀 프로젝트 DB Spring Version"
tags: [project, java, jsp, Oracle, css, HTML, BootStrap, API, JQuery, JavaScript, Spring, FrameWork]
feature: /img/SpringDB/logo.png
comments: true



---


> **사용한 플랫폼 : Spring, Oracle**



### 1.  header.jsp

각 기능들을 사용하고 필요한 Page들에 include 해줄 header.jsp 를 만들어준다.

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<%@ taglib prefix="fn" uri="http://java.sun.com/jsp/jstl/functions"%>
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>DIIB</title>
<meta content="width=device-width, initial-scale=1.0" name="viewport">
<!-- Google Web Fonts -->
<link rel="preconnect" href="https://fonts.gstatic.com">
<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@100;200;300;400;500;600;700;800;900&display=swap" rel="stylesheet">
<!-- Font Awesome -->
<link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.10.0/css/all.min.css" rel="stylesheet">
<!-- Libraries Stylesheet -->
<link href="../resources/css/owl.carousel.min.css" rel="stylesheet">
<!-- Customized Bootstrap Stylesheet -->
<link href="../resources/css/style.css" rel="stylesheet">
<script src="https://code.jquery.com/jquery-3.4.1.js" integrity="sha256-WpOohJOqMqqyKL9FccASB9O0KwACQJpFTUBLTYOVvVU=" crossorigin="anonymous">
	
</script>
<style>
body {
  cursor: url("../resources/img/cursor.png"), auto;
}
* {
  cursor: url("../resources/img/cursor.png"), auto;
}
</style>

</head>
<body>
	<!-- Topbar Start -->
	<div class="container-fluid">
		<div class="row align-items-center py-3 px-xl-5">
			<div class="col-lg-3 d-none d-lg-block">
				<a href="/" class="text-decoration-none">
					<img src="../resources/img/logo.png" alt="DB로고" height="50px" class="img-fluid">
				</a>
			</div>
			<div class="col-lg-6 col-6 text-left">
				<form method="get" action="#">
					<div class="input-group">
						<input type="text" class="form-control" placeholder="상품명을 입력 해주세요." name="pname">
						<div class="input-group-append">
							<button type="submit" class="input-group-text bg-transparent text-primary sp_btn">
								<i class="fa fa-search"></i>
							</button>
						</div>
					</div>
				</form>
			</div>
			<div class="col-lg-3 col-6 text-right">
					<div id="cart-badge">
						<a href="#" class="btn border">
							<i class="fas fa-shopping-cart text-primary"></i>
							<span class="badge"></span>
						</a>
					</div>
			</div>
		</div>
	</div>
	<!-- Topbar End -->
	<!-- Navbar Start -->
	<div class="container-fluid">
		<div class="row border-top px-xl-5">
			<div class="col-lg-3 d-none d-lg-block">
				<a class="btn shadow-none d-flex align-items-center justify-content-between bg-primary text-white w-100" data-toggle="collapse" href="#navbar-vertical" style="height: 65px; margin-top: -1px; padding: 0 30px;">
					<h6 class="m-0" style="color: white;">Categories</h6>
					<i class="fa fa-angle-down text-white"></i>
				</a>
				<nav class="collapse position-absolute navbar navbar-vertical navbar-light align-items-start p-0 border border-top-0 border-bottom-0 bg-light" id="navbar-vertical" style="width: calc(100% - 30px); z-index: 1;">
					<div class="navbar-nav w-100 overflow-hidden" style="height: 410px">
					</div>
				</nav>
			</div>
			<div class="col-lg-9">
				<nav class="navbar navbar-expand-lg bg-light navbar-light py-3 py-lg-0 px-0">
					<a href="/" class="text-decoration-none d-block d-lg-none">
						<img src="../resources/img/logo.png" alt="DB로고" height="50px" class="img-fluid">
					</a>
					<button type="button" class="navbar-toggler" data-toggle="collapse" data-target="#navbarCollapse">
						<span class="navbar-toggler-icon"></span>
					</button>
					<div class="collapse navbar-collapse justify-content-between" id="navbarCollapse">
						<div class="navbar-nav mr-auto py-0">
							<a href="/" class="nav-item nav-link">Home</a>
							<!--<a href="shop.jsp" class="nav-item nav-link active">미입력</a> <a href="detail.jsp" class="nav-item nav-link">미입력</a>-->
							<div class="nav-item dropdown">
								<a href="#" class="nav-link dropdown-toggle" data-toggle="dropdown">게시판</a>
								<div class="dropdown-menu rounded-0 m-0">
									<a href="#" class="dropdown-item">자유 게시판</a>
									<a href="#" class="dropdown-item">질문 게시판</a>
									<a href=#" class="dropdown-item">공지사항</a>
								</div>
							</div>
							<a href="#" class="nav-item nav-link">Q&A</a>
							<a href="#" class="nav-item nav-link">Auction</a>
							<a href="#" class="nav-item nav-link">Sale</a>
							<a href="#" class="nav-item nav-link">Event</a>
							<a href="#" class="nav-item nav-link">Contact</a>
						</div>
						<div class="navbar-nav ml-auto py-0">
								<a href="/user/login" class="nav-item nav-link">로그인</a>
								<a href="/user/join" class="nav-item nav-link">회원가입</a>
						</div>
					</div>
				</nav>
			</div>
		</div>
	</div>
	<!-- Navbar End -->
</body>
</html>
```

header에는 전체적인 css 를 관리하는 bootstrap과 jquery 등을 포함하고 있다.

![_config.yml]({{ site.baseurl }}/img/SpringDB/header.png)

### 2.  footer.jsp

마찬가지로 필요한 page들에 include할 footer.jsp 를 만들어준다.

```jsp
<%@page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<footer class="footer-black">
	<style>
.foop p {
	font-size: 12px;
}
</style>
	<!-- Footer Start -->
	<div class="container-fluid bg-secondary text-dark">
		<div class="row px-xl-5 pt-4">
			<div class="col-lg-4 col-md-12 mb-5 pr-3 pr-xl-5 foop">
				<img src="../resources/img/logo.png" alt="DB로고" height="50px" class="img-fluid">
				<p></p>
				<p class="mb-2 ">
					<i class="fa fa-map-marker-alt text-primary mr-3"></i>(00000) 서울특별시 강남구 대치동 777 - 77
				</p>
				<p class="mb-2 ">
					<i class="fa fa-envelope text-primary mr-3"></i>DB@Diamondblack.com
				</p>
				<p class="mb-0 ">
					<i class="fa fa-phone-alt text-primary mr-3"></i>+02 345 6789
				</p>
			</div>
			<div class="col-lg-8 col-md-12 foop">
				<div class="row">
					<h4>COPYRIGHT (C) Diamond ll Black Co., Ltd ALL RIGHTS RESERVED.</h4>
					<br>
					<P>DB 내 매거진, 스트리트스냅, 스토어 등 DB 자체 생성 콘텐츠는 DB 및 DB 계약업체에 저작권이 있습니다.</P>
					<br>
					<P>이러한 콘텐츠는 출처를 밝히고(DB 표기 및 www.DB.com 링크 포함 필수) 비상업적인 용도에서만 활용하실 수 있습니다.</P>
					<br>
					<P>커뮤니티 및 중고장터, 댓글 등 DB 회원이 올린 이미지가 저작권에 위배될 경우 신고 하시면 검토 후 삭제하겠습니다.</P>
					<br>
					<div>
						<h4>100% AUTHENTIC</h4>
						<P>DB에서 판매되는 모든 브랜드 제품은 정식제조, 정식수입원을 통해 유통되는 100% 정품임을 보증합니다.</P>
						<br>
					</div>
				</div>
			</div>
		</div>
		<div class="row border-top border-light mx-xl-5 py-4">
			<div class="col-md-6 px-xl-0">
				<p class="mb-md-0 text-center text-md-left text-dark">
					&copy;
					<a class="text-dark font-weight-semi-bold" href="#">(주)DIIB | 대표이사:이규동 | 사업자등록번호:000-00-00000</a>
				</p>
			</div>
			<div class="col-md-6 px-xl-0 text-center text-md-right">
				<img class="img-fluid" src="../resources/img/payments.png" alt="">
			</div>
		</div>
	</div>
	<!-- Footer End -->
</footer>
</body>
<!-- Back to Top -->
<a href="#" class="btn btn-primary back-to-top">
	<i class="fa fa-angle-double-up"></i>
</a>
<!-- JavaScript Libraries -->
<script src="https://code.jquery.com/jquery-3.4.1.min.js"></script>
<script src="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/js/bootstrap.bundle.min.js"></script>
<script src="../resources/js/owl.carousel.min.js"></script>
<script src="../resources/js/easing.min.js"></script>
<!-- Template Javascript -->
<script src="../resources/js/main.js"></script>
</html>
```

![_config.yml]({{ site.baseurl }}/img/SpringDB/footer.png)

### 3.  home.jsp

위에 만든 header와 footer 를 include 해주고 브랜드 리스트를 가져올 home.jsp를 만들어준다.

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<!DOCTYPE html>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<html>
<head>
<jsp:include page="header.jsp"></jsp:include>
</head>
<body>
	<hr>
	
	<hr>
	<!-- Featured Start -->
    <div class="container-fluid pt-5">
        <div class="row px-xl-5 pb-3">
            <div class="col-lg-3 col-md-6 col-sm-12 pb-1">
                <div class="d-flex align-items-center border mb-4" style="padding: 30px;">
                    <h1 class="fa fa-check text-primary m-0 mr-3"></h1>
                    <h5 class="font-weight-semi-bold m-0">Warranty 100%</h5>
                </div>
            </div>
            <div class="col-lg-3 col-md-6 col-sm-12 pb-1">
                <div class="d-flex align-items-center border mb-4" style="padding: 30px;">
                    <h1 class="fa fa-shipping-fast text-primary m-0 mr-2"></h1>
                    <h5 class="font-weight-semi-bold m-0">Free Shipping</h5>
                </div>
            </div>
            <div class="col-lg-3 col-md-6 col-sm-12 pb-1">
                <div class="d-flex align-items-center border mb-4" style="padding: 30px;">
                    <h1 class="fas fa-exchange-alt text-primary m-0 mr-3"></h1>
                    <h5 class="font-weight-semi-bold m-0">14-Day Return</h5>
                </div>
            </div>
            <div class="col-lg-3 col-md-6 col-sm-12 pb-1">
                <div class="d-flex align-items-center border mb-4" style="padding: 30px;">
                    <h1 class="fa fa-phone-volume text-primary m-0 mr-3"></h1>
                    <h5 class="font-weight-semi-bold m-0">Support</h5>
                </div>
            </div>
        </div>
    </div>
    <!-- Featured End -->
	
	<hr>
</body>
<jsp:include page="footer.jsp"></jsp:include>
</html>
```

![_config.yml]({{ site.baseurl }}/img/SpringDB/home.png)

## [프로젝트 주소](https://github.com/GreenteaPIE/TeamProjectDBSpringVer)
