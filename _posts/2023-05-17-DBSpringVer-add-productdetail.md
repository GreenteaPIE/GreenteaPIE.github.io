---
layout: post
title: 7 - DB Spring 상품 상세보기
date: 2023-05-17
excerpt: "팀 프로젝트 DB Spring Version"
tags: [project, java, jsp, Oracle, css, HTML, BootStrap, API, JQuery, JavaScript, Spring, FrameWork]
feature: /img/SpringDB/logo.png
comments: true


---


> **사용한 플랫폼 : Spring, Oracle**



### 1.  상품 정보 상세보기

#### ProductController.java 에 추가

```java
	// 상품 상세 보기
	@GetMapping("/productDetail")
	public String productDetailGET(int num, HttpServletRequest request) throws Exception {
		ProductVO pdlist = productService.productDetail(num);
		String pname = request.getParameter("pname");
		List<ProductVO> slist = productService.productSizeList(pname);

		request.setAttribute("ps", slist); // 상품 이름으로 존재하는 사이즈를 가져옴
		request.setAttribute("pdlist", pdlist); // 상품번호로 상품정보를 가져옴
		return "/product/productDetail";

	}
```

디테일 페이지에서 해당 상품에 존재하는 사이즈들을 불러와 해당 사이즈를 선택 할 수 있게 request 객체에 상품의 사이즈 리스트와 상품의 상세정보들을 담아 보낸다.

#### ProductMapper.java 에 추가

```java
	// 상품 상세
	public ProductVO productDetail(int num);

	// 상품 사이즈 리스트
	public List<ProductVO> productSizeList(String pname);
```

#### ProductService.java 에 추가

```java
	// 상품 상세
	public ProductVO productDetail(int num) throws Exception;

	// 상품 사이즈 리스트
	public List<ProductVO> productSizeList(String pname) throws Exception;
```

#### ProductServiceImpl.java 에 추가

```java
	@Override
	public ProductVO productDetail(int num) {

		return productmapper.productDetail(num);
	}

	@Override
	public List<ProductVO> productSizeList(String pname) throws Exception {

		return productmapper.productSizeList(pname);
	}
```

#### ProductMapper.xml 에 추가

```xml
	<!-- 상품 상세보기 -->
	<select id="productDetail" resultType="com.db.model.ProductVO">

		select * from product
		where num = #{num}

	</select>

	<!-- 상품 사이즈 리스트 -->
	<select id="productSizeList" resultType="com.db.model.ProductVO">

		select psize from
		product where pname = #{pname}

	</select>
```

#### productDetail.jsp 추가

```jsp
<%@ page language="java" contentType="text/html; charset=utf-8" pageEncoding="utf-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt"%>
<!DOCTYPE html>
<html>
<head>
<script src="../resources/js/detail.js"></script>
<script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
<jsp:include page="../header.jsp"></jsp:include>
</head>
<body>
	<hr>
	<!-- Page Header Start -->
	<div class="container bg-secondary mb-3" style="max-width: 800px;">
		<div class="d-flex flex-column align-items-center justify-content-center" style="min-height: 200px">
			<h1 class="font-weight-semi-bold text-uppercase mb-3">${pdlist.bname}</h1>
			<div class="d-inline-flex">
				<p class="m-0">
					<a href="/">Home</a>
				</p>
			</div>
		</div>
	</div>
	<!-- Page Header End -->
	<!-- Shop Detail Start -->
	<div class="container-fluid py-5">
		<div class="row px-xl-5">
			<div class="col-lg-5 pb-5">
				<div id="product-carousel" class="carousel slide" data-ride="carousel">
					<div class="carousel-inner border">
						<div class="carousel-item active">
							<img class="w-100 pimg" src="../resources/img/${ pdlist.imgUrl}" alt="Image">
						</div>
						<div class="carousel-item">
							<img class="w-100 pimg" src="../resources/img/${ pdlist.imgUrl}" alt="Image">
						</div>
					</div>
					<a class="carousel-control-prev" href="#product-carousel" data-slide="prev">
						<i class="fa fa-2x fa-angle-left text-dark"></i>
					</a>
					<a class="carousel-control-next" href="#product-carousel" data-slide="next">
						<i class="fa fa-2x fa-angle-right text-dark"></i>
					</a>
				</div>
			</div>
			<div class="col-lg-7 pb-5">
				<h3 class="font-weight-semi-bold">${pdlist.pname }</h3>
				<h3 class="font-weight-semi-bold mb-4">
					<c:if test="${pdlist.discountrate==0}">
						<fmt:formatNumber value="${Integer.parseInt(pdlist.price)}" pattern="₩###,###" />
					</c:if>
					<c:if test="${pdlist.discountrate!=0}">
									   ${pdlist.discountrate}% 할인
                                            <del style="text-decoration: line-through; color: gray;">
							<!-- 할인전 가격 -->
							<fmt:formatNumber value="${Integer.parseInt(pdlist.price)}" pattern="₩###,###" />
						</del>
						<br>
						<!-- 할인후 가격 -->
						<c:set var="discountedPrice" value="${Integer.parseInt(pdlist.price) * (100 - pdlist.discountrate) / 100}" />
						<fmt:formatNumber value="${Math.round(discountedPrice/100)*100}" pattern="₩###,###" />
					</c:if>
				</h3>
				<p class="mb-4">${pdlist.explain }</p>
				<div class="d-flex mb-3">
					<p class="text-dark font-weight-medium mb-0 mr-3">Sizes</p>
				</div>
				<form name="addcart" method="post" onsubmit="return validateForm();">
					<input type="hidden" name="num" value="${pdlist.num }">
					<input type="hidden" name="userid" value="${user.userid }">
					<c:if test="${pdlist.discountrate==0}">
						<input type="hidden" name="price" value="${Integer.parseInt(pdlist.price)}">
					</c:if>
					<c:if test="${pdlist.discountrate!=0}">
						<input type="hidden" name="price" value="${Math.round(discountedPrice/100)*100}">
					</c:if>
					<c:forEach var="ps" items="${ps }">
						<div class="custom-control custom-radio custom-control-inline">
							<input type="radio" class="custom-control-input" id="${ps.psize }" name="psize" value="${ps.psize }">
							<label class="custom-control-label" for="${ps.psize }">${ps.psize }</label>
						</div>
					</c:forEach>
					<div class="d-flex align-items-center mb-4 pt-2">
						<div class="input-group quantity mr-3" style="width: 130px;">
							<div class="input-group-btn">
								<button class="btn btn-primary btn-minus" onclick="minus()">
									<i class="fa fa-minus"></i>
								</button>
							</div>
							<input type="text" name="quantity" class="form-control bg-secondary text-center" value="1">
							<div class="input-group-btn">
								<button class="btn btn-primary btn-plus" onclick="plus()">
									<i class="fa fa-plus"></i>
								</button>
							</div>
						</div>
						<c:if test="${user != null}">
							<button class="btn btn-primary px-3">
								<i class="fa fa-shopping-cart mr-1"></i> Add To Cart
							</button>
						</c:if>
						<c:if test="${user == null}">
							<button class="btn btn-primary px-3" onclick="alert('로그인 후에 이용이 가능합니다.'); return false;">
								<i class="fa fa-shopping-cart mr-1"></i> Add To Cart
							</button>
						</c:if>
					</div>
				</form>
			</div>
		</div>
		<div class="row px-xl-5">
			<div class="col">
				<div class="nav nav-tabs justify-content-center border-secondary mb-4">
					<a class="nav-item nav-link active" data-toggle="tab" href="#tab-pane-1">Description</a>
					<a class="nav-item nav-link" data-toggle="tab" href="#tab-pane-2">Information</a>
				</div>
				<div class="tab-content">
					<div class="tab-pane fade show active" id="tab-pane-1">
						<h4 class="mb-3">Product Description</h4>
						<p>상품 설명</p>
					</div>
					<div class="tab-pane fade" id="tab-pane-2">
						<h4 class="mb-3">Additional Information</h4>
						<p>상품 정보</p>
						<div class="row">
							<div class="col-md-6">
								<ul class="list-group list-group-flush">
									<li class="list-group-item px-0">정보1</li>
									<li class="list-group-item px-0">정보2</li>
									<li class="list-group-item px-0">정보3</li>
									<li class="list-group-item px-0">정보4</li>
								</ul>
							</div>
							<div class="col-md-6">
								<ul class="list-group list-group-flush">
									<li class="list-group-item px-0">정보1-1</li>
									<li class="list-group-item px-0">정보2-2</li>
									<li class="list-group-item px-0">정보3-3</li>
									<li class="list-group-item px-0">정보4-4</li>
								</ul>
							</div>
						</div>
					</div>
				</div>
			</div>
		</div>
	</div>
	<!-- Shop Detail End -->
	<hr>
</body>
<jsp:include page="../footer.jsp"></jsp:include>
</html>
```

할인율이 적용된 상품은 할인율이 적용된 가격으로 출력되게 구분하여 만든다.

로그인을 안했을 때 장바구니 추가 버튼을 조작할 수 없게 만들고 수량 조절을 위해 스크립트를 추가 해준다.

#### detail.js 추가

```js
function plus() {
			// input 태그 가져오기
			var quantityInput = document.getElementsByName("quantity")[0];
			// 수량 증가
			var quantity = parseInt(quantityInput.value);
			quantityInput.value = quantity + 1;
			// form 제출 방지
			event.preventDefault();
		}
		function minus() {
			// input 태그 가져오기
			var quantityInput = document.getElementsByName("quantity")[0];
			// 수량 감소
			var quantity = parseInt(quantityInput.value);
			if (quantity > 1) {
				quantityInput.value = quantity - 1;
			}
			// form 제출 방지
			event.preventDefault();
		}
```

 위 스크립트는 -버튼을 클릭 시 0까지 1씩 감소 시키고 +버튼을 클릭 시 1씩 증가 시킨다.

### 2. 각 상품 리스트 페이지 수정

#### brandProductList.jsp 수정

```jsp
									<a href='/product/productDetail?num=<c:out value="${bplist.num }"/>&pname=${bplist.pname}'>
										<img class="img-fluid w-100" style="height: 240px" src="../resources/img/${bplist.imgUrl}" alt="">
									</a>
```

#### saleList.jsp 수정

```jsp
									<a href='/product/productDetail?num=<c:out value="${hdlist.num }"/>&pname=${hdlist.pname}'>
										<img class="img-fluid w-100" style="height: 240px" src="../resources/img/${hdlist.imgUrl}" alt="">
									</a>							
```

#### searchProduct.jsp 수정

```jsp
	<a href='/product/productDetail?num=<c:out value="${bplist.num }"/>&pname=${bplist.pname}'>
									<img class="img-fluid w-100" style="height: 280px" src="../resources/img/${bplist.imgUrl}" alt="">
								</a>
```

각 페이지에 불러온 상품 이미지를 클릭 했을 때 각 상품의 상세 페이지로 이동되게 수정한다.

## [프로젝트 주소](https://github.com/GreenteaPIE/TeamProjectDBSpringVer)
