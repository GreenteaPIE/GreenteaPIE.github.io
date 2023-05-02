---
layout: post
title: MVC2 Pattern MainForm
date: 2023-04-22
excerpt: "두 번째 팀 프로젝트 DiamondBlack"
tags: [project, java, jsp, Oracle, css, HTML, BootStrap, API, JQuery, JavaScript]
feature: /img/DiamondBlack/logo.png
comments: true

---


> **사용한 플랫폼 : Eclipse**



### 1. Index.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
</head>

<body>

	<jsp:include page="header.jsp"></jsp:include>

	<!-- Categories Start -->
	<div class="container-fluid pt-5">`
		<div class="row px-xl-5 pb-3">
			<div class="col-lg-4 col-md-6 pb-1">
				<div class="cat-item text-center" style="padding: 30px;">
					<p class="text-right"></p>
					<a href="DBServlet?command=BRAND_LIST&bname=balenciaga" class="cat-img position-relative overflow-hidden mb-3"> <img class="img-fluid" src="images/balenciagalogo.png" alt="">
					</a>
					<h5 class="font-weight-semi-bold m-0"></h5>
				</div>
			</div>
			<div class="col-lg-4 col-md-6 pb-1">
				<div class="cat-item text-center" style="padding: 30px;">
					<p class="text-right"></p>
					<a href="DBServlet?command=BRAND_LIST&bname=hermes" class="cat-img position-relative overflow-hidden mb-3"> <img class="img-fluid" src="images/hermeslogo.png" alt="">
					</a>
					<h5 class="font-weight-semi-bold m-0"></h5>
				</div>
			</div>
			<div class="col-lg-4 col-md-6 pb-1">
				<div class="cat-item text-center" style="padding: 30px;">
					<p class="text-right"></p>
					<a href="DBServlet?command=BRAND_LIST&bname=louisvuitton" class="cat-img position-relative overflow-hidden mb-3"> <img class="img-fluid" src="images/louisvuittonlogo.png" alt="">
					</a>
					<h5 class="font-weight-semi-bold m-0"></h5>
				</div>

			</div>
			<div class="col-lg-4 col-md-6 pb-1">
				<div class="cat-item text-center" style="padding: 30px;">
					<p class="text-right"></p>
					<a href="DBServlet?command=BRAND_LIST&bname=prada" class="cat-img position-relative overflow-hidden mb-3"> <img class="img-fluid" src="images/pradalogo.png" alt="">
					</a>
					<h5 class="font-weight-semi-bold m-0"></h5>
				</div>

			</div>
			<div class="col-lg-4 col-md-6 pb-1">
				<div class="cat-item text-center" style="padding: 30px;">
					<p class="text-right"></p>
					<a href="DBServlet?command=BRAND_LIST&bname=Saint Laurent" class="cat-img position-relative overflow-hidden mb-3"> <img class="img-fluid" src="images/saintlaurentlogo.png" alt="">
					</a>
					<h5 class="font-weight-semi-bold m-0"></h5>
				</div>

			</div>


		</div>
	</div>
	<!-- Categories End -->
    
	<jsp:include page="footer.jsp"></jsp:include>
</body>
</html>
```

### 2. header.jsp

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
<meta content="Free HTML Templates" name="keywords">
<meta content="Free HTML Templates" name="description">

<!-- Favicon -->
<link href="img/favicon.ico" rel="icon">

<!-- Google Web Fonts -->
<link rel="preconnect" href="https://fonts.gstatic.com">
<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@100;200;300;400;500;600;700;800;900&display=swap" rel="stylesheet">

<!-- Font Awesome -->

<link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.10.0/css/all.min.css" rel="stylesheet">

<!-- Libraries Stylesheet -->
<link href="css/owl.carousel.min.css" rel="stylesheet">

<!-- Customized Bootstrap Stylesheet -->
<link href="css/style.css" rel="stylesheet">
</head>

<body>
	<!-- Topbar Start -->
	<div class="container-fluid">

		<div class="row align-items-center py-3 px-xl-5">
			<div class="col-lg-3 d-none d-lg-block">
				<a href="index.jsp" class="text-decoration-none"> <img src="images/logo.png" alt="DB로고" height="50px" class="img-fluid">

				</a>
			</div>
			<div class="col-lg-6 col-6 text-left">
				<form name="sp" method="post" action="DBServlet">
					<input type="hidden" name="command" value="search_product">
					<div class="input-group">
						<input type="text" class="form-control" placeholder="제품을 검색하세요" name="pname">
						<div class="input-group-append">
							<button type="submit" class="input-group-text bg-transparent text-primary">
								<i class="fa fa-search"></i>
							</button>
						</div>
					</div>
				</form>

			</div>
			<div class="col-lg-3 col-6 text-right">


				<c:if test="${not empty loginUser}">

					<a href="DBServlet?command=user_cart&loginUser=${loginUser.userid }" class="btn border"> <i class="fas fa-shopping-cart text-primary"></i> <span class="badge"></span>
					</a>
				</c:if>
				<c:if test="${empty loginUser}">

					<a href="#" class="btn border" onclick="alert('로그인 후에 이용이 가능합니다.'); return false;"> <i class="fas fa-shopping-cart text-primary"></i> <span class="badge"></span>
					</a>
				</c:if>



			</div>
		</div>
	</div>
	<!-- Topbar End -->


	<!-- Navbar Start -->
	<div class="container-fluid">
		<div class="row border-top px-xl-5">
			<div class="col-lg-3 d-none d-lg-block">
				<a class="btn shadow-none d-flex align-items-center justify-content-between bg-primary text-white w-100" data-toggle="collapse" href="#navbar-vertical" style="height: 65px; margin-top: -1px; padding: 0 30px;">
					<h6 class="m-0" style="color: white;">Categories</h6> <i class="fa fa-angle-down text-white"></i>
				</a>
				<nav class="collapse position-absolute navbar navbar-vertical navbar-light align-items-start p-0 border border-top-0 border-bottom-0 bg-light" id="navbar-vertical" style="width: calc(100% - 30px); z-index: 1;">
					<div class="navbar-nav w-100 overflow-hidden" style="height: 410px">
						<div class="nav-item dropdown">
							<a href="#" class="nav-link" data-toggle="dropdown">BALENCIAGA <i class="fa fa-angle-down float-right mt-1"></i></a>
							<div class="dropdown-menu position-absolute bg-secondary border-0 rounded-0 w-100 m-0">
								<a href="DBServlet?command=BRAND_TOP_LIST&bname=balenciaga" class="dropdown-item">TOP</a><a href="DBServlet?command=BRAND_Bottom_LIST&bname=balenciaga" class="dropdown-item">BOTTOM</a> <a href="DBServlet?command=BRAND_Bcollection_LIST&bname=balenciaga" class="dropdown-item">boutique collection</a>
							</div>
						</div>
						<div class="nav-item dropdown">
							<a href="#" class="nav-link" data-toggle="dropdown">HERMES <i class="fa fa-angle-down float-right mt-1"></i></a>
							<div class="dropdown-menu position-absolute bg-secondary border-0 rounded-0 w-100 m-0">
								<a href="DBServlet?command=BRAND_TOP_LIST&bname=hermes" class="dropdown-item">TOP</a> <a href="DBServlet?command=BRAND_Bottom_LIST&bname=hermes" class="dropdown-item">BOTTOM</a> <a href="DBServlet?command=BRAND_Bcollection_LIST&bname=hermes" class="dropdown-item">boutique collection</a>
							</div>
						</div>
						<div class="nav-item dropdown">
							<a href="#" class="nav-link" data-toggle="dropdown">LOUIS VUITTON <i class="fa fa-angle-down float-right mt-1"></i></a>
							<div class="dropdown-menu position-absolute bg-secondary border-0 rounded-0 w-100 m-0">
								<a href="DBServlet?command=BRAND_TOP_LIST&bname=louisvuitton" class="dropdown-item">TOP</a> <a href="DBServlet?command=BRAND_Bottom_LIST&bname=louisvuitton" class="dropdown-item">BOTTOM</a> <a href="DBServlet?command=BRAND_Bcollection_LIST&bname=louisvuitton" class="dropdown-item">boutique collection</a>
							</div>
						</div>

						<div class="nav-item dropdown">
							<a href="#" class="nav-link" data-toggle="dropdown">PRADA <i class="fa fa-angle-down float-right mt-1"></i></a>
							<div class="dropdown-menu position-absolute bg-secondary border-0 rounded-0 w-100 m-0">
								<a href="DBServlet?command=BRAND_TOP_LIST&bname=prada" class="dropdown-item">TOP</a> <a href="DBServlet?command=BRAND_Bottom_LIST&bname=prada" class="dropdown-item">BOTTOM</a> <a href="DBServlet?command=BRAND_Bcollection_LIST&bname=prada" class="dropdown-item">boutique collection</a>
							</div>
						</div>

						<div class="nav-item dropdown">
							<a href="#" class="nav-link" data-toggle="dropdown">SAINT LAURENT <i class="fa fa-angle-down float-right mt-1"></i></a>
							<div class="dropdown-menu position-absolute bg-secondary border-0 rounded-0 w-100 m-0">
								<a href="DBServlet?command=BRAND_TOP_LIST&bname=Saint Laurent" class="dropdown-item">TOP</a> <a href="DBServlet?command=BRAND_Bottom_LIST&bname=Saint Laurent" class="dropdown-item">BOTTOM</a> <a href="DBServlet?command=BRAND_Bcollection_LIST&bname=Saint Laurent" class="dropdown-item">boutique collection</a>
							</div>
						</div>

					</div>
				</nav>
			</div>

			<div class="col-lg-9">
				<nav class="navbar navbar-expand-lg bg-light navbar-light py-3 py-lg-0 px-0">
					<a href="index.jsp" class="text-decoration-none d-block d-lg-none"> <img src="images/logo.png" alt="DB로고" height="50px" class="img-fluid">
					</a>
					<button type="button" class="navbar-toggler" data-toggle="collapse" data-target="#navbarCollapse">
						<span class="navbar-toggler-icon"></span>
					</button>
					<div class="collapse navbar-collapse justify-content-between" id="navbarCollapse">
						<div class="navbar-nav mr-auto py-0">
							<a href="index.jsp" class="nav-item nav-link">Home</a>
							<!--<a href="shop.jsp" class="nav-item nav-link active">미입력</a> <a href="detail.jsp" class="nav-item nav-link">미입력</a>-->
							
							<div class="nav-item dropdown">
								<a href="#" class="nav-link dropdown-toggle" data-toggle="dropdown">게시판</a>
								<div class="dropdown-menu rounded-0 m-0">
									<a href="DBServlet?command=free_board_list" class="dropdown-item">자유게시판</a> <a href="DBServlet?command=qna_board_list" class="dropdown-item">Q&A게시판</a>
								</div>
							</div>
							<a href="DBServlet?command=auction_view" class="nav-item nav-link">Auction</a>
							<a href="DBServlet?command=Hot_Deal" class="nav-item nav-link">HotDeal</a>
							<a href="contact.jsp" class="nav-item nav-link">Contact</a>
							

						</div>
						<div class="navbar-nav mr-auto py-0" align="right">
							<c:if test="${!empty loginUser }">
								
									&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
									${loginUser.grade}등급회원&nbsp;&nbsp;&nbsp;${loginUser.userid}님
								
							</c:if>
						</div>

						<div class="navbar-nav ml-auto py-0">

							<c:if test="${empty loginUser }">

								<a href="DBServlet?command=shopuser_loin" class="nav-item nav-link">로그인</a>

								<a href="DBServlet?command=user_join_form" class="nav-item nav-link">회원가입</a>
							</c:if>
							<c:if test="${loginUser.grade == 1}">

								<div class="nav-item dropdown">
									<a href="#" class="nav-link dropdown-toggle" data-toggle="dropdown">관리자</a>
									<div class="dropdown-menu rounded-0 m-0">
										<a href="DBServlet?command=product_management" class="dropdown-item">상품 관리</a> <a href="DBServlet?command=user_management" class="dropdown-item">회원 관리</a> <a href="DBServlet?command=board_management" class="dropdown-item">게시판 관리</a> <a href="DBServlet?command=auction" class="dropdown-item">옥 션</a> <a href="DBServlet?command=sales_management" class="dropdown-item">매출 관리</a>
									</div>
								</div>


							</c:if>

							<c:if test="${!empty loginUser }">
								<c:if test="${loginUser.grade != 1 }">
									<a href="DBServlet?command=my_page&userid=${loginUser.userid }" class="nav-item nav-link">MY PAGE</a>

								</c:if>
								<a href="DBServlet?command=shopuser_logout" class="nav-item nav-link">로그아웃</a>
							</c:if>
							<input type="hidden" name="command" value="member_logout">
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

### 3. footer.jsp

```jsp
<%@page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<footer class="footer-black">

<style>
.foop p {
  font-size: 12px;
}
</style>
	<!-- Footer Start -->
	<div class="container-fluid bg-secondary text-dark mt-5 pt-5">
		<div class="row px-xl-5 pt-5">
			<div class="col-lg-4 col-md-12 mb-5 pr-3 pr-xl-5 foop">

				<img src="images/logo.png" alt="DB로고" height="50px" class="img-fluid">

				<p></p>
				<p class="mb-2 ">
					<i class="fa fa-map-marker-alt text-primary mr-3"></i>(00000) 서울특별시 강남구 대치동 777 - 77
				</p>
				<p class="mb-2 ">
					<i class="fa fa-envelope text-primary mr-3"></i>jongmin@hunnam.com
				</p>
				<p class="mb-0 ">
					<i class="fa fa-phone-alt text-primary mr-3"></i>+02 345 6789
				</p>
			</div>
			<div class="col-lg-8 col-md-12 foop">
				<div class="row">
					<h4>COPYRIGHT (C) Diamond ll Black Co., Ltd ALL RIGHTS RESERVED.</h4><br>
				<P>DB 내 매거진, 스트리트스냅, 스토어 등 DB 자체 생성 콘텐츠는 DB 및 DB 계약업체에 저작권이 있습니다.</P><br>
				<P>이러한 콘텐츠는 출처를 밝히고(DB 표기 및 www.DB.com 링크 포함 필수) 비상업적인 용도에서만 활용하실 수 있습니다. </P><br>
				<P>커뮤니티 및 중고장터, 댓글 등 DB 회원이 올린 이미지가 저작권에 위배될 경우 신고 하시면 검토 후 삭제하겠습니다. </P><br>
				
				<div>	<h4>100% AUTHENTIC</h4>
				<P>DB에서 판매되는 모든 브랜드 제품은 정식제조, 정식수입원을 통해 유통되는 100% 정품임을 보증합니다.</P><br>
				
				</div>
				</div>
			</div>
		</div>
		<div class="row border-top border-light mx-xl-5 py-4">
			<div class="col-md-6 px-xl-0">
				<p class="mb-md-0 text-center text-md-left text-dark">
					&copy; <a class="text-dark font-weight-semi-bold" href="#">(주)DIIB | 대표이사:임종민 | 사업자등록번호:000-00-00000</a>
				</p>
			</div>
			<div class="col-md-6 px-xl-0 text-center text-md-right">
				<img class="img-fluid" src="images/payments.png" alt="">
			</div>
		</div>
	</div>
	<!-- Footer End -->

</footer>

</body>

<!-- Back to Top -->
<a href="#" class="btn btn-primary back-to-top"><i class="fa fa-angle-double-up"></i></a>


<!-- JavaScript Libraries -->
<script src="https://code.jquery.com/jquery-3.4.1.min.js"></script>
<script src="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/js/bootstrap.bundle.min.js"></script>
<script src="script/easing.min.js"></script>


<!-- Template Javascript -->
<script src="js/main.js"></script>


</html>
```



## [프로젝트 주소](https://github.com/GreenteaPIE/TeamProjectDBverOne)
