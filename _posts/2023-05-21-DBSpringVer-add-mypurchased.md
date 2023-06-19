---
layout: post
title: 11 - DB Spring 주문내역과 주문취소
date: 2023-05-21
excerpt: "팀 프로젝트 DB Spring Version"
tags: [project, java, jsp, Oracle, css, HTML, BootStrap, API, JQuery, JavaScript, Spring, FrameWork]
feature: /img/SpringDB/logo.png
comments: true


---


> **사용한 플랫폼 : Spring, Oracle**

### 1.  나의 주문내역 기능

#### purchasedList.jsp 추가

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
</head>
<jsp:include page="../header.jsp"></jsp:include>
<body>
	<hr>
	<!-- Page Header Start -->
	<div class="container bg-secondary mb-3" style="max-width: 800px;">
		<div class="d-flex flex-column align-items-center justify-content-center" style="min-height: 300px">
			<h1 class="font-weight-semi-bold text-uppercase mb-3">Purchased List</h1>
			<div class="d-inline-flex">
				<p class="m-0">진행 중인 주문내역</p>
			</div>
		</div>
	</div>
	<!-- Page Header End -->
	<div class="container bg-secondary mb-3" style="max-width: 900px;">
		<form name="frm" action="DBServlet">
			<input type="hidden" value="user_purchased_detail" name="command">
			<table class="table table-bordered text-center mb-0">
				<thead class="bg-secondary text-dark">
					<tr>
						<th>Order Number</th>
						<!-- 					<th>Products</th> -->
						<!-- 					<th>Total Price</th> -->
						<th>Order Date</th>
						<th>Status</th>
					</tr>
				</thead>
				<tbody class="align-middle">
				
				<c:if test="${olist.size()==0}">
								<tr>
									<td class="align-middle" colspan="3">
										<h3>주문내역이 없습니다.</h3>
									</td>
								</tr>
							</c:if>
					<c:set var="prev_orderNumber" value="-1" scope="page" />
					<c:forEach var="olist" items="${olist }">
						<c:choose>
							<c:when test="${prev_orderNumber != olist.ordernumber}">
								<c:set var="prev_orderNumber" value="${olist.ordernumber}" />
								<tr>
									<td class="align-middle"><a href="#">${olist.ordernumber }</a></td>
									<td class="align-middle"><fmt:formatDate value="${olist.indate }" type="date" /></td>
									<td class="align-middle"><c:choose>
											<c:when test='${olist.result  =="1" }'>주문 확인 중</c:when>
											<c:when test='${olist.result  =="2" }'>처리 진행 중</c:when>
											<c:when test='${olist.result  =="3" }'>배송 중</c:when>
											<c:when test='${olist.result  =="4" }'>취소 요청 중</c:when>
											<c:when test='${olist.result  =="5" }'>취소 완료</c:when>
										</c:choose></td>
								</tr>
							</c:when>
						</c:choose>
					</c:forEach>
			</table>
		</form>
		<div class="card-footer border-secondary bg-transparent" align="center">
			<button class="btn btn-block btn-primary my-3 py-3" style="width: 350px;" onclick="location.href='/'">Continue shopping</button>
		</div>
	</div>
	<hr>
</body>
<jsp:include page="../footer.jsp"></jsp:include>
</html>
```

해당 페이지는 주문 처리 과정과 주문 번호, 주문 일자를 표시한다.

![_config.yml]({{ site.baseurl }}/img/SpringDB/mypur.png)

#### UserController.java 에 추가

```java
	// 나의 주문 내역
	@GetMapping("/myPurchased")
	public String myPuchasedGET(String userid, HttpServletRequest request) throws Exception {

		ArrayList<OrderVO> olist = userService.getMyPurchased(userid);
		request.setAttribute("olist", olist); // order_view table

		return "/user/purchasedList";

	}
```

#### UserMapper.java 에 추가

```java
	// 주문내역 리스트
	public ArrayList<OrderVO> getMyPurchased(String userid);
```

#### UserService.java 에 추가

```java
	// 주문내역 리스트
	public ArrayList<OrderVO> getMyPurchased(String userid);
```

#### UserServiceImpl.java 에 추가

```java
	@Override
	public ArrayList<OrderVO> getMyPurchased(String userid) {
		
		return usermapper.getMyPurchased(userid);
	}
```

#### UserMapper.xml 에 추가

```xml
	<!-- 내 주문 내역 리스트 -->
	<select id="getMyPurchased" resultType="com.db.model.OrderVO">

		select*from order_view
		where
		userid=#{userid}

	</select>
```

### 2. 주문 상세 보기

#### purchasedList.jsp 에 수정

```jsp
									<td class="align-middle">
									<a href="/user/myPurchasedDetail?ordernumber=${olist.ordernumber}">${olist.ordernumber }</a>
									</td>
```

주문번호를 누르면 해당 주문의 상세내역 페이지로 이동시킨다.

#### purchasedDetail.jsp 추가

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
</head>
<jsp:include page="../header.jsp"></jsp:include>
<body>
	<hr>
	<!-- Page Header Start -->
	<div class="container bg-secondary mb-3" style="max-width: 800px;">
		<div class="d-flex flex-column align-items-center justify-content-center" style="min-height: 300px">
			<h1 class="font-weight-semi-bold text-uppercase mb-3">Purchased List</h1>
			<div class="d-inline-flex">
				<p class="m-0">주문내역 상세 확인</p>
			</div>
		</div>
	</div>
	<input type="hidden" name="userid" value="${user.userid }">
	<!-- Page Header End -->
	<div class="container bg-secondary mb-3" style="max-width: 1000px;">
		<div class="d-flex flex-column align-items-center justify-content-center" style="min-height: 300px">
			<table class="table table-bordered text-center mb-0">
				<thead class="bg-secondary text-dark">
					<c:if test="${not empty olist}">
						<tr>
							<th>주문한 사람</th>
							<td>${olist[0].userid }</td>
						</tr>
						<tr>
							<th>배송받을 사람</th>
							<td>${olist[0].name }</td>
						</tr>
						<tr>
							<th>전화번호</th>
							<td>${olist[0].phone }</td>
						</tr>
						<tr>
							<th>배송지</th>
							<td>${olist[0].address1 }&nbsp;${olist[0].address2 }${olist[0].address3 }</td>
						</tr>
					</c:if>
				</thead>
			</table>
		</div>
	</div>
	<div class="container bg-secondary mb-3" style="max-width: 1000px;">
		<table class="table table-bordered text-center mb-0">
			<thead class="bg-secondary text-dark">
				<tr>
					<th>No.</th>
					<th>Products</th>
					<th>size</th>
					<th>quantity</th>
					<th>Price</th>
					<th>Status</th>
					<th>Order Date</th>
				</tr>
			</thead>
			<tbody class="align-middle">
				<c:forEach var="olist" items="${olist}">
					<c:forEach var="plist" items="${plist }">
						<c:if test="${olist.num eq plist.num }">
							<tr>
								<td class="align-middle">${olist.num}</td>
								<td class="align-middle"><a href='/product/productDetail?num=<c:out value="${plist.num }"/>&pname=${plist.pname}'>${plist.pname}</a></td>
								<td class="align-middle">${olist.psize}</td>
								<td class="align-middle">${olist.quantity }</td>
								<td class="align-middle"><fmt:setLocale value="ko_KR" /> <fmt:formatNumber type="currency" value="${olist.price * olist.quantity }" currencySymbol="₩" /></td>
								<td class="align-middle"><c:choose>
										<c:when test='${olist.result  =="1" }'>주문 확인 중</c:when>
										<c:when test='${olist.result  =="2" }'>처리 진행 중</c:when>
										<c:when test='${olist.result  =="3" }'>배송 중</c:when>
										<c:when test='${olist.result  =="4" }'>취소 요청 중</c:when>
										<c:when test='${olist.result  =="5" }'>취소 완료</c:when>
									</c:choose></td>
								<td class="align-middle"><fmt:formatDate value="${olist.indate}" pattern="yyyy. MM. dd. HH:mm" /></td>
							</tr>
							<c:set var="ordernumber" value="${olist.ordernumber }" />
							<c:set var="result" value="${olist.result }" />
						</c:if>
					</c:forEach>
				</c:forEach>
				<c:set var="totalPrice" value="0" />
				<c:forEach var="olist" items="${olist}">
					<c:forEach var="plist" items="${plist}">
						<c:if test="${olist.num == plist.num}">
							<c:set var="totalPrice" value="${olist.totalprice}" />
						</c:if>
					</c:forEach>
				</c:forEach>
				<tr>
					<th colspan="4">Total Price</th>
					<td colspan="3"><fmt:formatNumber value="${totalPrice}" type="currency" /></td>
				<tr>
		</table>
		<div class="card-footer border-secondary bg-transparent" align="center">
			<button class="btn btn-block btn-primary my-3 py-3" style="width: 350px;" onclick="#">Withdraw Order</button>
			<button class="btn btn-block btn-primary my-3 py-3" style="width: 350px;" onclick="location.href='/user/myPurchased?userid=${user.userid }'">Purchased List</button>
		</div>
	</div>
	<hr>
</body>
<jsp:include page="../footer.jsp"></jsp:include>
</html>
```

해당 페이지는 결제 시 작성 했던 주문지와 구매한 상품의 정보등을 표시한다.

![_config.yml]({{ site.baseurl }}/img/SpringDB/mypurdetail.png)

#### UserController.java 에 추가

```java
	@Autowired
	ProductService productService;

// 나의 주문 내역 상세 조회
	@GetMapping("/myPurchasedDetail")
	public String myPurchasedDetailGET(int ordernumber, HttpServletRequest request) throws Exception {

		ArrayList<ProductVO> plist = productService.getAllProduct(); // 모든 상품 가져오기
		request.setAttribute("plist", plist);

		ArrayList<OrderVO> olist = userService.getMyPurchasedDetail(ordernumber); //주문번호로 정보 가져오기
		request.setAttribute("olist", olist);

		return "/user/purchasedDetail";
	}

```

#### UserMapper.java 에 추가

```java
	// 주문내역 상세조회
	public ArrayList<OrderVO> getMyPurchasedDetail(int ordernumber);
```

#### UserService.java 에 추가

```java
	// 주문내역 상세조회
	public ArrayList<OrderVO> getMyPurchasedDetail(int ordernumber);
```

#### UserServiceImpl.java 에 추가

```java
	@Override
	public ArrayList<OrderVO> getMyPurchasedDetail(int ordernumber) {
		
		return usermapper.getMyPurchasedDetail(ordernumber);
	}
```

#### UserMapper.xml 에 추가

```xml
	<!-- 내 주문 내역 상세 조회 -->
	<select id="getMyPurchasedDetail"
		resultType="com.db.model.OrderVO">
		select * from order_view
		where
		orderNumber=#{orderNumber}
	</select>
```

### 3. 주문 취소 기능

#### purchasedDetail.jsp 에 수정

```jsp
			<button class="btn btn-block btn-primary my-3 py-3" style="width: 350px;" onclick="withdrawOrder('${ordernumber}', '${result}')">Withdraw Order</button>
```

버튼 클릭 시 비동기로 주문 취소 메소드를 작동 시킨다.

#### Script 추가

```js
	<script>
		function withdrawOrder(ordernumber, orderResult) {
			if (orderResult === "1") {
				if (confirm("주문을 취소하시겠습니까?")) {
					$.ajax({
						url : `/user/withdrawOrder?ordernumber=${ordernumber}`,
						type : 'GET',
						success : function(data) {
							if (data === "Success") {
								alert("주문이 성공적으로 취소되었습니다.");
								location.reload(); // 주문 상태를 업데이트하기 위해 페이지를 다시 로드합니다.
							} else {
								alert("주문 취소 중 문제가 발생했습니다.");
							}
						},
						error : function(err) {
							alert("주문 취소 요청이 실패했습니다.");
						}
					});
				}
			} else {
				alert("취소 요청할 수 없습니다.");
			}
		}
	</script>
```

#### UserController.java 에 추가

```java
	// 주문 취소 요청
	@GetMapping("/withdrawOrder")
	@ResponseBody
	public ResponseEntity<String> withdrawOrderGET(int ordernumber, HttpServletRequest request) {
		System.out.println("취소요청 주문번호" + ordernumber);
		userService.withdrawChangeResult(ordernumber);

		return ResponseEntity.ok("Success");
	}
```

#### UserMapper.java 에 추가

```java
	// 주문 취소 result -> 4 으로 변경
	public int withdrawChangeResult(@Param("ordernumber") int ordernumber);
```

#### UserService.java 에 추가

```java
	// 주문 취소 result -> 4 으로 변경
	public int withdrawChangeResult(@Param("ordernumber") int ordernumber);
```

#### UserServiceImpl.java 에 추가

```java
	@Override
	public int withdrawChangeResult(int ordernumber) {
		
		return usermapper.withdrawChangeResult(ordernumber);
	}

```

#### UserMapper.xml 에 추가

```xml
	<!-- 주문 취소한 orderdetail result 4로 변경 -->
	<update id="withdrawChangeResult">

		update order_detail set result = 4 where
		ordernumber=#{ordernumber}
	</update>
```

주문 취소 요청이 완료 되면 아래와 같이 취소 요청 중으로 바뀐다.

![_config.yml]({{ site.baseurl }}/img/SpringDB/withdraw.png)

## [프로젝트 주소](https://github.com/GreenteaPIE/TeamProjectDBSpringVer)
