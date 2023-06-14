---
layout: post
title: 9 - DB Spring 쿠폰
date: 2023-05-19
excerpt: "팀 프로젝트 DB Spring Version"
tags: [project, java, jsp, Oracle, css, HTML, BootStrap, API, JQuery, JavaScript, Spring, FrameWork]
feature: /img/SpringDB/logo.png
comments: true


---


> **사용한 플랫폼 : Spring, Oracle**



### 1. 쿠폰 발급 기능

#### event.jsp 추가

쿠폰같은 경우는 임의로 제작한 쿠폰 이미지에 각각 쿠폰 이름과 할인금액을 정해서 coupon table에 userid와 함께 넣고 결제 시에 적용 할 수 있도록 만들 예정이다. 

쿠폰을 추가할 경우엔 쿠폰을 임의로 만들어 html에 붙이는 방식으로 추가한다.

```jsp
<%@ page language="java" contentType="text/html; charset=utf-8" pageEncoding="utf-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<!DOCTYPE html>
<html>
<head>
<jsp:include page="header.jsp"></jsp:include>
<style>
.title {
	text-align: center;
	font-family: "Helvetica Neue", Arial, sans-serif;
	font-weight: bold;
	font-size: 28px;
	letter-spacing: 1px;
	text-transform: uppercase;
	color: #333;
}
</style>
</head>
<body>
	<hr>
	<!-- Page Header Start -->
	<div class="container bg-secondary mb-3" style="max-width: 800px;">
		<div class="d-flex flex-column align-items-center justify-content-center" style="min-height: 200px">
			<h1 class="font-weight-semi-bold text-uppercase mb-3">Event</h1>
			<div class="d-inline-flex">
				<p class="m-0">
					<a href="/">Home</a>
				</p>
			</div>
		</div>
	</div>
	<!-- Page Header End -->
	<div class="container">
		<!-- 신규가입 환영 쿠폰 영역 -->
		<div class="row">
			<div class="col-md-12">
				<h2 class="title">신규가입 환영 쿠폰</h2>
			</div>
		</div>
		<!-- 빈 공간 -->
		<div class="row mt-4">
			<div class="col-md-12"></div>
		</div>
		<c:if test="${user != null}">
			<div class="row">
				<div class="col-md-6">
					<a href="javascript:void(0);" id="WelcomeCoupon-link">
						<img class="img-fluid w-100" style="height: 240px; width: 450px" src="../resources/img/welcomecoupon.png" alt="">
					</a>
				</div>
			</div>
		</c:if>
		<c:if test="${user == null}">
			<div class="row">
				<div class="col-md-6">
					<a href="#" onclick="alert('로그인 후에 이용이 가능합니다.'); return false;">
						<img class="img-fluid w-100" style="height: 240px; width: 450px" src="../resources/img/welcomecoupon.png" alt="">
					</a>
				</div>
			</div>
		</c:if>
		<!-- 빈 공간 -->
		<div class="row mt-4">
			<div class="col-md-12"></div>
		</div>
		<!-- 이번 달 할인 쿠폰 영역 -->
		<div class="row mt-4">
			<div class="col-md-12">
				<h2 class="title">이달의 할인 쿠폰</h2>
			</div>
		</div>
		<!-- 빈 공간 -->
		<div class="row mt-4">
			<div class="col-md-12"></div>
		</div>
		<c:if test="${user != null}">
			<div class="row">
				<div class="col-md-6">
					<div style="padding: 10px;">
						<a href="javascript:void(0);" id="BronzeCoupon-link">
							<img class="img-fluid w-100" style="height: 240px; width: 450px" src="../resources/img/bronzecoupon.png" alt="">
						</a>
					</div>
				</div>
				<div class="col-md-6">
					<div style="padding: 10px;">
						<a href="javascript:void(0);" id="GoldCoupon-link">
							<img class="img-fluid w-100" style="height: 240px; width: 450px" src="../resources/img/goldcoupon.png" alt="">
						</a>
					</div>
				</div>
				<div class="col-md-6">
					<div style="padding: 10px;">
						<a href="javascript:void(0);" id="SilverCoupon-link">
							<img class="img-fluid w-100" style="height: 240px; width: 450px" src="../resources/img/silvercoupon.png" alt="">
						</a>
					</div>
				</div>
				<div class="col-md-6">
					<div style="padding: 10px;">
						<a href="javascript:void(0);" id="DiaCoupon-link">
							<img class="img-fluid w-100" style="height: 240px; width: 450px" src="../resources/img/diacoupon.png" alt="">
						</a>
					</div>
				</div>
			</div>
		</c:if>
		<c:if test="${user == null}">
			<div class="row">
				<div class="col-md-6">
					<div style="padding: 10px;">
						<a href="#" onclick="alert('로그인 후에 이용이 가능합니다.'); return false;">
							<img class="img-fluid w-100" style="height: 240px; width: 450px" src="../resources/img/bronzecoupon.png" alt="">
						</a>
					</div>
				</div>
				<div class="col-md-6">
					<div style="padding: 10px;">
						<a href="#" onclick="alert('로그인 후에 이용이 가능합니다.'); return false;">
							<img class="img-fluid w-100" style="height: 240px; width: 450px" src="../resources/img/goldcoupon.png" alt="">
						</a>
					</div>
				</div>
				<div class="col-md-6">
					<div style="padding: 10px;">
						<a href="#" onclick="alert('로그인 후에 이용이 가능합니다.'); return false;">
							<img class="img-fluid w-100" style="height: 240px; width: 450px" src="../resources/img/silvercoupon.png" alt="">
						</a>
					</div>
				</div>
				<div class="col-md-6">
					<div style="padding: 10px;">
						<a href="#" onclick="alert('로그인 후에 이용이 가능합니다.'); return false;">
							<img class="img-fluid w-100" style="height: 240px; width: 450px" src="../resources/img/diacoupon.png" alt="">
						</a>
					</div>
				</div>
			</div>
		</c:if>
	</div>
	<hr>
</body>
<jsp:include page="footer.jsp"></jsp:include>
</html>

```

로그인을 하지 않았을 경우엔 쿠폰을 발급 받지 못하게 만들어 준다.

![_config.yml]({{ site.baseurl }}/img/SpringDB/coupon.png)

#### CouponVO 추가

```java
package com.db.model;

import lombok.Data;

@Data
public class CouponVO {
	private String userid, couponname, imgurl;
	private int discountprice, cnum, couponresult;
}
```

쿠폰을 지급할때 userid와 함께 Table에 저장하여 중복 지급을 막고, 유저가 해당 쿠폰을 사용했는지에 대한 유무를 couponresult로 확인한다.

#### event.jsp 에 스크립트 추가

```js
<script>
	    function issueCoupon(couponName, discountPrice, imgUrl, requiredGrade) {
	        if (${user.grade} < requiredGrade) {
	            alert('등급이 낮아 쿠폰을 받으실 수 없습니다.');
	        } else {
	            var xhttp = new XMLHttpRequest();
	            xhttp.onreadystatechange = function() {
	                if (this.readyState === 4 && this.status === 200) {
	                    var result = parseInt(this.responseText);
	                    switch(result) {
	                        case 1:
	                            alert('쿠폰 지급 완료');
	                            var couponLink = document.getElementById(couponName + '-link');
	                            couponLink.style.pointerEvents = 'none';
	                            couponLink.style.opacity = 0.5;
	                            break;
	                        case 2:
	                            alert('이미 지급된 쿠폰입니다');
	                            var couponLink = document.getElementById(couponName + '-link');
	                            couponLink.style.pointerEvents = 'none';
	                            couponLink.style.opacity = 0.5;
	                            break;
	                        default:
	                            alert('오류가 발생했습니다. 관리자에게 문의하세요');
	                            break;
	                    }
	                }
	            };
	            xhttp.open('POST', '/user/addCoupon?userid=' + encodeURIComponent('${user.userid}') + '&couponname=' + couponName + '&discountprice=' + discountPrice + '&imgurl=' + imgUrl, true);
	            xhttp.send();
	        }
	    }

	    document.getElementById('GoldCoupon-link').addEventListener('click', function() {
	        issueCoupon('GoldCoupon', 500000, 'goldcoupon.png', 3);
	    });

	    document.getElementById('SilverCoupon-link').addEventListener('click', function() {
	        issueCoupon('SilverCoupon', 100000, 'silvercoupon.png', 2);
	    });

	    document.getElementById('DiaCoupon-link').addEventListener('click', function() {
	        issueCoupon('DiaCoupon', 1000000, 'diacoupon.png', 4);
	    });

	    document.getElementById('BronzeCoupon-link').addEventListener('click', function() {
	        issueCoupon('BronzeCoupon', 5000, 'bronzecoupon.png', 0);
	    });

	    document.getElementById('WelcomeCoupon-link').addEventListener('click', function() {
	        issueCoupon('WelcomeCoupon', 10000, 'welcomecoupon.png', 0);
	    });
	</script>
```

회원등급이 충족되지 않았을 경우엔 쿠폰을 지급되지 않도록 만들고, 이미 지급된 쿠폰의 경우 중복 지급되지 않게 만든다.

#### UserController.java 에 추가

```java
	// 쿠폰 받기
	@PostMapping("/addCoupon")
	@ResponseBody
	public String addWcouponPOST(@RequestParam String userid, @RequestParam String couponname, String imgurl,
			@RequestParam int discountprice, @ModelAttribute CouponVO coupon, HttpServletRequest request)
			throws Exception {

		int result = userService.addCoupon(coupon);
		return result + "";

	}
```

#### UserMapper.java 에 추가

```java
	// 쿠폰 받기
	public int addCoupon(CouponVO coupon);

	// 쿠폰 보유 확인
	public CouponVO checkCoupon(CouponVO coupon);
```

checkCoupon 은 addCoupon 메소드가 작동할때 먼저 해당 쿠폰을 보유하고 있는지 검사를 거친다.

#### UserService.java 에  추가

```java
	// 쿠폰 받기
	public int addCoupon(CouponVO coupon) throws Exception;

	// 쿠폰 보유 확인
	public CouponVO checkCoupon(CouponVO coupon);
```

#### UserServiceImpl.java 에 추가

```java
	@Override
	public int addCoupon(CouponVO coupon){
		
		CouponVO checkCoupon = usermapper.checkCoupon(coupon);//쿠폰 보유 확인
		
		if(checkCoupon != null) {
			return 2;
		}
		
		try {
		return usermapper.addCoupon(coupon);
		}catch (Exception e) {
			return 0;
		}
	}

	@Override
	public CouponVO checkCoupon(CouponVO coupon) {
		
		return usermapper.checkCoupon(coupon);
	}
```

checkCoupon 메소드를 작동하고 검색한 값이 존재 한다면 2를 반환하여 "이미 지급된 쿠폰 입니다." 메시지를 띄우고, 그렇지 않다면 addCoupon메서드를 작동한다.

#### header.jsp 수정

```jsp
<a href="/event" class="nav-item nav-link">Event</a>
```

#### HomeController.java 에 추가

```java
	// Event 페이지
	@GetMapping("/event")
	public void eventGET() {
		logger.info("event 페이지 진입");
	}

```

### 2. 나의 보유 쿠폰

#### myCoupon.jsp 추가

```jsp
<%@ page language="java" contentType="text/html; charset=utf-8" pageEncoding="utf-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<!DOCTYPE html>
<html>
<head>
<jsp:include page="../header.jsp"></jsp:include>
</head>
<body>
	<hr>
	<!-- Page Header Start -->
	<div class="container bg-secondary mb-3" style="max-width: 800px;">
		<div class="d-flex flex-column align-items-center justify-content-center" style="min-height: 200px">
			<h1 class="font-weight-semi-bold text-uppercase mb-3">MY COUPON</h1>
			<div class="d-inline-flex">
				<p class="m-0">
					<a href="/">Home</a>
				</p>
			</div>
		</div>
	</div>
	<div id="my-container" class="container-fluid pt-5">
		<div class="col-lg-9 col-md-12">
			<h3 style="text-align: center;">사용한 쿠폰</h3>
			<div class="row pb-3">
				<c:set var="usedCouponFound" value="false" />
				<c:forEach var="couplist" items="${couplist }">
					<c:if test="${couplist.couponresult == 0 }">
						<c:set var="usedCouponFound" value="true" />
						<div class="col-lg-4 col-md-6 col-sm-12 pb-1">
							<div class="card product-item border-0 mb-4">
								<div class="card-header position-relative overflow-hidden bg-transparent border p-0">
									<img class="img-fluid w-100" style="height: 140px;" src="../resources/img/${couplist.imgurl}" alt="">
								</div>
							</div>
						</div>
					</c:if>
				</c:forEach>
				<c:if test="${!usedCouponFound}">
					<div class="col-12">
						<p style="text-align: center;">사용한 쿠폰 없음</p>
					</div>
				</c:if>
			</div>
		</div>
	</div>
	<!-- Page Header End -->
	<div id="my-container" class="container-fluid pt-5">
		<div class="col-lg-9 col-md-12">
			<h3 style="text-align: center;">미사용 쿠폰</h3>
			<div class="row pb-3">
				<c:set var="unusedCouponFound" value="false" />
				<c:forEach var="couplist" items="${couplist }">
					<c:if test="${couplist.couponresult != 0 }">
						<c:set var="unusedCouponFound" value="true" />
						<div class="col-lg-4 col-md-6 col-sm-12 pb-1">
							<div class="card product-item border-0 mb-4">
								<div class="card-header position-relative overflow-hidden bg-transparent border p-0">
									<img class="img-fluid w-100" style="height: 140px;" src="../resources/img/${couplist.imgurl}" alt="">
								</div>
							</div>
						</div>
					</c:if>
				</c:forEach>
				<c:if test="${!unusedCouponFound}">
					<div class="col-12">
						<p style="text-align: center;">미사용 쿠폰 없음</p>
					</div>
				</c:if>
			</div>
		</div>
	</div>
	<hr>
</body>
<jsp:include page="../footer.jsp"></jsp:include>
</html>

```

해당 페이지는 어떤 쿠폰을 보유하고 있는지만 확인하는 페이지이기 때문에 미사용 쿠폰과 사용된 쿠폰 영역을 나누어준다.

상품 구매 시 쿠폰을 사용하게 되면 해당 쿠폰의 couponresult값을 0으로 바꾸어 사용한 쿠폰에 보이게 만든다.

#### UserController.java 에 추가

```java
	// 보유 쿠폰 가져오기
	@GetMapping("/myCoupon")
	public String myCouponGET(String userid, HttpServletRequest request) {

		ArrayList<CouponVO> couplist = userService.getMyCoupon(userid);
		request.setAttribute("couplist", couplist);
		return "/user/myCoupon";

	}
```

#### UserMapper.java 에 추가

```java
	// 보유 쿠폰 확인
	public ArrayList<CouponVO> getMyCoupon(String userid);
```

#### UserService.java 에 추가

```java
	// 보유 쿠폰 확인
	public ArrayList<CouponVO> getMyCoupon(String userid);
```

#### UserServiceImpl.java 에 추가

```java
	@Override
	public ArrayList<CouponVO> getMyCoupon(String userid) {
	
		return usermapper.getMyCoupon(userid);
	}
```

#### header.jsp 수정

```jsp
<a href="/user/myCoupon?userid=${user.userid }" class="dropdown-item">보유 쿠폰</a>
```

## [프로젝트 주소](https://github.com/GreenteaPIE/TeamProjectDBSpringVer)
