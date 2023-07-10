---
layout: post
title: 21 - DB Spring 어드민 매출/주문 관리
date: 2023-05-31
excerpt: "팀 프로젝트 DB Spring Version"
tags: [project, java, jsp, Oracle, css, HTML, BootStrap, API, JQuery, JavaScript, Spring, FrameWork]
feature: /img/SpringDB/logo.png
comments: true



---


> **사용한 플랫폼 : Spring, Oracle**



### 1.  매출/주문 관리 페이지

모든 주문 내역을 불러오는 페이지를 만든다.

해당 페이지 order Table에 result 1(신규주문), 2(주문처리), 3(배송 중), 4(취소요청) 항목을 구분하여 보여준다.

#### AdminMapper.java 에 추가

```java
	public ArrayList<OrderVO> getNewOrder(); // 신규주문 가져오기

	public ArrayList<OrderVO> getProcessOrder(); // 처리주문 가져오기

	public ArrayList<OrderVO> getWithdrawOrder(); // 취소주문 가져오기

	public ArrayList<OrderVO> getSalesOrder(); // 판매된주문 가져오기
```

#### AdminMapper.xml 에 추가

```xml
	
	<!-- 신규 주문 가져오기 -->
	<select id="getNewOrder" resultType="com.db.model.OrderVO">

		select*from order_view where
		result = 1
	</select>

	<!-- 처리 주문 가져오기 -->
	<select id="getProcessOrder" resultType="com.db.model.OrderVO">

		select*from order_view
		where result = 2
	</select>

	<!-- 취소 주문 가져오기 -->
	<select id="getWithdrawOrder" resultType="com.db.model.OrderVO">

		select*from order_view
		where result = 4
	</select>

	<!-- 판매된 주문 가져오기 -->
	<select id="getSalesOrder" resultType="com.db.model.OrderVO">

		select*from order_view
		where result = 3
	</select>
```

#### AdminService.java 에 추가

```java
	public ArrayList<OrderVO> getNewOrder() throws Exception; // 신규주문 가져오기

	public ArrayList<OrderVO> getProcessOrder() throws Exception; // 처리주문 가져오기

	public ArrayList<OrderVO> getWithdrawOrder() throws Exception; // 취소주문 가져오기

	public ArrayList<OrderVO> getSalesOrder() throws Exception; // 판매된주문 가져오기
```

#### AdminServiceImpl.java 에 추가

```java
	@Override
	public ArrayList<OrderVO> getNewOrder() throws Exception {
	
		return mapper.getNewOrder();
	}

	@Override
	public ArrayList<OrderVO> getProcessOrder() throws Exception {
		
		return mapper.getProcessOrder();
	}

	@Override
	public ArrayList<OrderVO> getWithdrawOrder() throws Exception {
		
		return mapper.getWithdrawOrder();
	}
	
	@Override
	public ArrayList<OrderVO> getSalesOrder() throws Exception {
	
		return mapper.getSalesOrder();
	}
```

#### AdminController.java 에 추가

```java
	// 매출 & 주문 관리
	@GetMapping("/sales_OrderManagement")
	public String salesOrderPost(HttpServletRequest request) throws Exception {

		ArrayList<OrderVO> newOrderlist = adminService.getNewOrder();// 새로운 주문 확인 result 1 --> 처리진행중 result 2로 변경
		request.setAttribute("newOrderlist", newOrderlist);

		ArrayList<OrderVO> processOrderlist = adminService.getProcessOrder();// 주문 처리 확인 result 2 --> 배송중 result 3으로 변경
		request.setAttribute("processOrderlist", processOrderlist);

		ArrayList<OrderVO> withdrawOrderlist = adminService.getWithdrawOrder();// 취소 주문 확인 result 4 --> 취소 완료 resulut 5로
																				// 변경
		request.setAttribute("withdrawOrderlist", withdrawOrderlist);

		ArrayList<OrderVO> salesOrderlist = adminService.getSalesOrder(); // 판매된 주문 확인
		request.setAttribute("salesOrderlist", salesOrderlist);

		
		return "/admin/salesManagementPage";

	}
```

#### salesManagementPage.jsp 추가

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<style>
.right {
	float: right;
}

.left {
	float: left;
}

.container {
	overflow: hidden;
}
</style>
<jsp:include page="../header.jsp"></jsp:include>
</head>
<body>
	<hr>
	<div class="container bg-secondary mb-3" style="max-width: 800px;">
		<div class="d-flex flex-column align-items-center justify-content-center" style="min-height: 200px">
			<h1 class="font-weight-semi-bold text-uppercase mb-3">매출&주문 관리</h1>
			<div class="d-inline-flex">
				<p class="m-0">
					<a href="/">Home</a>
				</p>
			</div>
		</div>
	</div>
	<div class="container">
		<div class="left" style="width: 50%">
			<div style="border: solid #0D0D0D; margin-bottom: 30px; box-shadow: 3px 3px 6px rgba(0, 0, 0, 0.3);">
				<div class="container bg-secondary mb-3" style="max-width: 800px;">
					<div class="d-flex flex-column align-items-center justify-content-center" style="min-height: 300px">
						<h1 class="font-weight-semi-bold text-uppercase mb-3">Sales List</h1>
						<div class="d-inline-flex">
							<p class="m-0">매출 관리</p>
						</div>
					</div>
				</div>
				<!-- Page Header End -->
				<div class="container bg-secondary mb-3" style="max-width: 900px;">
					<input type="hidden" value="user_purchased_detail" name="command">
					<table class="table table-bordered text-center mb-0">
						<thead class="bg-secondary text-dark">
							<tr>
								<th>Order Number</th>
								<th>Orderer</th>
								<th>Order Date</th>
								<th>Total Price</th>
							</tr>
						</thead>
						<tbody class="align-middle">
							<c:if test="${salesOrderlist.size()==0}">
								<tr>
									<td class="align-middle" colspan="4">
										<h3>판매 완료된 주문이 없습니다.</h3>
									</td>
								</tr>
							</c:if>
							<c:set var="prev_orderNumber" value="-1" scope="page" />
							<c:set var="totalSum" value="0" />
							<c:forEach var="orderlist" items="${salesOrderlist }">
								<c:choose>
									<c:when test="${prev_orderNumber != orderlist.ordernumber}">
										<c:set var="prev_orderNumber" value="${orderlist.ordernumber}" />
										<c:set var="totalSum" value="${totalSum + orderlist.totalprice}" />
										<tr>
											<td class="align-middle"><a href="/admin/salesOrder?ordernumber=${orderlist.ordernumber}">${orderlist.ordernumber }</a></td>
											<td class="align-middle">${orderlist.userid }</td>
											<td class="align-middle"><fmt:formatDate value="${orderlist.indate }" type="date" /></td>
											<td class="align-middle"><fmt:formatNumber type="currency" value="${orderlist.totalprice}" currencySymbol="₩" /></td>
										</tr>
									</c:when>
								</c:choose>
							</c:forEach>
						</tbody>
					</table><br>
					<table class="table table-bordered text-center mb-0">
						<tfoot class="bg-secondary text-dark">
							<tr>
								<th colspan="3">Total</th>
								<td colspan="1"><fmt:formatNumber type="currency" value="${totalSum}" currencySymbol="₩" /></td>
							</tr>
						</tfoot>
					</table>
				</div>
			</div>
		</div>
		<div class="right" style="width: 45%">
			<div style="border: solid #0D0D0D; margin-bottom: 30px; box-shadow: 3px 3px 6px rgba(0, 0, 0, 0.3);">
				<div class="container bg-secondary mb-3" style="max-width: 800px;">
					<div class="d-flex flex-column align-items-center justify-content-center" style="min-height: 300px">
						<h1 class="font-weight-semi-bold text-uppercase mb-3">New Order List</h1>
						<div class="d-inline-flex">
							<p class="m-0">신규 주문</p>
						</div>
					</div>
				</div>
				<!-- Page Header End -->
				<div class="container bg-secondary mb-3" style="max-width: 900px;">
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
							<c:if test="${newOrderlist.size()==0}">
								<tr>
									<td class="align-middle" colspan="3">
										<h3>신규 주문이 없습니다.</h3>
									</td>
								</tr>
							</c:if>
							<c:set var="prev_orderNumber" value="-1" scope="page" />
							<c:forEach var="orderlist" items="${newOrderlist }">
								<c:choose>
									<c:when test="${prev_orderNumber != orderlist.ordernumber}">
										<c:set var="prev_orderNumber" value="${orderlist.ordernumber}" />
										<tr>
											<td class="align-middle"><a href="/admin/orderProcess?ordernumber=${orderlist.ordernumber}">${orderlist.ordernumber }</a></td>
											<td class="align-middle"><fmt:formatDate value="${orderlist.indate }" type="date" /></td>
											<td class="align-middle"><c:choose>
													<c:when test='${orderlist.result  =="1" }'>주문 확인 중</c:when>
													<c:when test='${orderlist.result  =="2" }'>처리 진행 중</c:when>
													<c:when test='${orderlist.result  =="3" }'>배송 중</c:when>
													<c:when test='${orderlist.result  =="4" }'>취소 요청 중</c:when>
													<c:when test='${orderlist.result  =="5" }'>취소 완료</c:when>
												</c:choose></td>
										</tr>
									</c:when>
								</c:choose>
							</c:forEach>
					</table>
				</div>
			</div>
			<div style="border: solid #0D0D0D; margin-bottom: 30px; box-shadow: 3px 3px 6px rgba(0, 0, 0, 0.3);">
				<div class="container bg-secondary mb-3" style="max-width: 800px;">
					<div class="d-flex flex-column align-items-center justify-content-center" style="min-height: 300px">
						<h1 class="font-weight-semi-bold text-uppercase mb-3">Process Order List</h1>
						<div class="d-inline-flex">
							<p class="m-0">처리 진행중인 주문</p>
						</div>
					</div>
				</div>
				<!-- Page Header End -->
				<div class="container bg-secondary mb-3" style="max-width: 900px;">
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
							<c:if test="${processOrderlist.size()==0}">
								<tr>
									<td class="align-middle" colspan="3">
										<h3>배송 요청할 주문이 없습니다.</h3>
									</td>
								</tr>
							</c:if>
							<c:set var="prev_orderNumber" value="-1" scope="page" />
							<c:forEach var="orderlist" items="${processOrderlist }">
								<c:choose>
									<c:when test="${prev_orderNumber != orderlist.ordernumber}">
										<c:set var="prev_orderNumber" value="${orderlist.ordernumber}" />
										<tr>
											<td class="align-middle"><a href="/admin/shipmentProcess?ordernumber=${orderlist.ordernumber}">${orderlist.ordernumber }</a></td>
											<td class="align-middle"><fmt:formatDate value="${orderlist.indate }" type="date" /></td>
											<td class="align-middle"><c:choose>
													<c:when test='${orderlist.result  =="1" }'>주문 확인 중</c:when>
													<c:when test='${orderlist.result  =="2" }'>처리 진행 중</c:when>
													<c:when test='${orderlist.result  =="3" }'>배송 중</c:when>
													<c:when test='${orderlist.result  =="4" }'>취소 요청 중</c:when>
													<c:when test='${orderlist.result  =="5" }'>취소 완료</c:when>
												</c:choose></td>
										</tr>
									</c:when>
								</c:choose>
							</c:forEach>
					</table>
				</div>
			</div>
			<div style="border: solid #0D0D0D; margin-bottom: 30px; box-shadow: 3px 3px 6px rgba(0, 0, 0, 0.3);">
				<div class="container bg-secondary mb-3" style="max-width: 800px;">
					<div class="d-flex flex-column align-items-center justify-content-center" style="min-height: 300px">
						<h1 class="font-weight-semi-bold text-uppercase mb-3">WithDraw Order List</h1>
						<div class="d-inline-flex">
							<p class="m-0">취소 요청 주문</p>
						</div>
					</div>
				</div>
				<!-- Page Header End -->
				<div class="container bg-secondary mb-3" style="max-width: 900px;">
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
							<c:if test="${withdrawOrderlist.size()==0}">
								<tr>
									<td class="align-middle" colspan="3">
										<h3>취소 요청된 주문이 없습니다.</h3>
									</td>
								</tr>
							</c:if>
							<c:set var="prev_orderNumber" value="-1" scope="page" />
							<c:forEach var="orderlist" items="${withdrawOrderlist }">
								<c:choose>
									<c:when test="${prev_orderNumber != orderlist.ordernumber}">
										<c:set var="prev_orderNumber" value="${orderlist.ordernumber}" />
										<tr>
											<td class="align-middle"><a href="/admin/withDrawOrderCheck?ordernumber=${orderlist.ordernumber}">${orderlist.ordernumber }</a></td>
											<td class="align-middle"><fmt:formatDate value="${orderlist.indate }" type="date" /></td>
											<td class="align-middle"><c:choose>
													<c:when test='${orderlist.result  =="1" }'>주문 확인 중</c:when>
													<c:when test='${orderlist.result  =="2" }'>처리 진행 중</c:when>
													<c:when test='${orderlist.result  =="3" }'>배송 중</c:when>
													<c:when test='${orderlist.result  =="4" }'>취소 요청 중</c:when>
													<c:when test='${orderlist.result  =="5" }'>취소 완료</c:when>
												</c:choose></td>
										</tr>
									</c:when>
								</c:choose>
							</c:forEach>
					</table>
				</div>
			</div>
		</div>
	</div>
	<hr>
</body>
<jsp:include page="../footer.jsp"></jsp:include>
</html>

```

![_config.yml]({{ site.baseurl }}/img/SpringDB/ordermanagement.png)

### 2. 신규 주문 확인 -> 처리 기능

#### AdminController.java 에 추가

신규 주문 확인 페이지로 이동하는 메서드를 만들어 준다.

```java
	// 신규 주문 조회
	@GetMapping("/orderProcess")
	public String orderProcessGET(int ordernumber, HttpServletRequest request) throws Exception {

		ArrayList<ProductVO> plist = productService.getAllProduct(); // 모든 상품 가져오기
		request.setAttribute("plist", plist);

		ArrayList<OrderVO> olist = userService.getMyPurchasedDetail(ordernumber); // 주문번호로 정보 가져오기
		request.setAttribute("olist", olist);

		return "/admin/orderCheckProcess";
	}
```

#### orderCheckProcess.jsp 추가

```java
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
			<h1 class="font-weight-semi-bold text-uppercase mb-3">OrderCheck Process</h1>
			<div class="d-inline-flex">
				<p class="m-0">신규 주문 처리</p>
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
								<td class="align-middle">${plist.pname}</td>
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
			<button class="btn btn-block btn-primary my-3 py-3" style="width: 350px;" onclick="checkOrder('${ordernumber}', '${result}')">Check Order</button>
			<button class="btn btn-block btn-primary my-3 py-3" style="width: 350px;" onclick="location.href='/admin/sales_OrderManagement'">Sales&Order List</button>
		</div>
	</div>
	<hr>
	<script>
		function checkOrder(ordernumber, orderResult) {
			if (orderResult === "1") {
				if (confirm("주문처리 하시겠습니까?")) {
					$.ajax({
						url : `/admin/checkOrder?ordernumber=${ordernumber}`,
						type : 'POST',
						success : function(data) {
							if (data === "Success") {
								alert("주문이 처리 되었습니다.");
								location.reload(); // 주문 상태를 업데이트하기 위해 페이지를 다시 로드합니다.
							} else {
								alert("주문 처리 중 문제가 발생했습니다.");
							}
						},
						error : function(err) {
							alert("주문 배송처리 요청이 실패했습니다.");
						}
					});
				}
			} else {
				alert("처리 요청할 수 없습니다.");
			}
		}
	</script>
</body>
<jsp:include page="../footer.jsp"></jsp:include>
</html>
```

![_config.yml]({{ site.baseurl }}/img/SpringDB/ordercheck.png)

#### AdminMapper.java 에 추가

신규 주문을 주문 확인 처리 동작하는 메서드를 만들어 준다.

```java
	public int checkOrderChangeResult(@Param("ordernumber") int ordernumber); // 주문 확인처리 result -> 2 로 변경

```

#### AdminMapper.xml 에 추가

```xml
	<!-- 확인한 주문 orderdetail result 2로 변경 -->
	<update id="checkOrderChangeResult">

		update order_detail set result = 2 where
		ordernumber=#{ordernumber}
	</update>
```

#### AdminService.java 에 추가

```java
	public int checkOrderChangeResult(@Param("ordernumber") int ordernumber); // 주문 확인처리 result -> 2 로 변경

```

#### AdminServiceImpl.java 에 추가

```java
	@Override
	public int checkOrderChangeResult(int ordernumber) {
	
		return mapper.checkOrderChangeResult(ordernumber);
	}
```

#### AdminController.java 에 추가

```java
	// 주문 처리
	@PostMapping("/checkOrder")
	@ResponseBody
	public ResponseEntity<String> checkOrderGET(int ordernumber, HttpServletRequest request) {

		adminService.checkOrderChangeResult(ordernumber);

		return ResponseEntity.ok("Success");
	}

```

### 3. 주문 진행 확인 -> 배송 처리 기능

#### AdminController.java 에 추가

주문 배송 처리 페이지로 이동하는 메서드를 만들어 준다.

```java
	// 배송 처리 조회
	@GetMapping("/shipmentProcess")
	public String shipmentProcessGET(int ordernumber, HttpServletRequest request) throws Exception {

		ArrayList<ProductVO> plist = productService.getAllProduct(); // 모든 상품 가져오기
		request.setAttribute("plist", plist);

		ArrayList<OrderVO> olist = userService.getMyPurchasedDetail(ordernumber); // 주문번호로 정보 가져오기
		request.setAttribute("olist", olist);

		return "/admin/shipmentProcess";
	}
```

#### shipmentProcess.jsp 추가

```java
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
			<h1 class="font-weight-semi-bold text-uppercase mb-3">ShipMent Process</h1>
			<div class="d-inline-flex">
				<p class="m-0">주문 배송 처리</p>
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
							<c:set var="userid" value="${olist[0].userid }"></c:set>
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
								<td class="align-middle">${plist.pname}</td>
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
<button class="btn btn-block btn-primary my-3 py-3" style="width: 350px;" onclick="shipmentOrder('${ordernumber}', '${result}', '${userid}', '${totalPrice}')">Shipment Order</button>
			<button class="btn btn-block btn-primary my-3 py-3" style="width: 350px;" onclick="location.href='/admin/sales_OrderManagement'">Sales&Order List</button>
		</div>
	</div>
	<hr>
	<script>
	function shipmentOrder(ordernumber, orderResult, userid, totalPrice){
			if (orderResult === "2") {
				if (confirm("주문을 배송처리 하시겠습니까?")) {
					$.ajax({
						url : `/admin/shipmentOrder?ordernumber=${ordernumber}&userid=${userid}&totalprice=${totalPrice}`,
						type : 'POST',
						success : function(data) {
							if (data === "Success") {
								alert("주문이 배송처리 되었습니다.");
								location.reload(); // 주문 상태를 업데이트하기 위해 페이지를 다시 로드합니다.
							} else {
								alert("주문 배송처리 중 문제가 발생했습니다.");
							}
						},
						error : function(err) {
							alert("주문 배송처리 요청이 실패했습니다.");
						}
					});
				}
			} else {
				alert("처리 요청할 수 없습니다.");
			}
		}
	</script>
</body>
<jsp:include page="../footer.jsp"></jsp:include>
</html>
```

![_config.yml]({{ site.baseurl }}/img/SpringDB/shipment.png)

#### AdminMapper.java 에 추가

신규 주문을 주문 확인 처리 동작하는 메서드를 만들어 준다.

```java
	public int shipmentChangeResult(@Param("ordernumber") int ordernumber); // 주문 발송처리 result -> 3 으로 변경

```

#### AdminMapper.xml 에 추가

```xml
	<!-- 발송처리한 주문 orderdetail result 3으로 변경 -->
	<update id="shipmentChangeResult">

		update order_detail set result = 3 where
		ordernumber=#{ordernumber}
	</update>
```

#### AdminService.java 에 추가

```java
	public int shipmentChangeResult(@Param("ordernumber") int ordernumber); // 주문 발송처리 result -> 3 으로 변경

```

#### AdminServiceImpl.java 에 추가

```java
	@Override
	public int checkOrderChangeResult(int ordernumber) {
	
		return mapper.checkOrderChangeResult(ordernumber);
	}
```

#### ProductMapper.java 에 추가

배송 요청은 주문 완료 단계로 구분해 주문자에게 주문금액의 임의로 설정한 만큼의 비율로 포인트를 지급하는 메서드를 만들어서 추가해 준다.

```java
	// 주문완료 후 포인트 지급
	void increaseUserPoint(Map<String, Object> params);
```

#### ProductMapper.xml 에 추가

```java
	<!-- 주문완료 후 포인트 지급 -->
	<update id="increaseUserPoint" parameterType="map">
		UPDATE shopuser
		SET
		point = point + (#{totalprice} * 0.01)
		WHERE userid = #{userid}
	</update>
```

#### ProductService.java 에 추가

```java
	// 주문완료 후 포인트 지급
	public void increaseUserPoint(String userid, int totalprice) throws Exception;
```

#### ProductServiceImpl.java 에 추가

```java
	@Override
	public void increaseUserPoint(String userid, int totalprice) throws Exception {
		Map<String, Object> params = new HashMap<>();
		params.put("userid", userid);
		params.put("totalprice", totalprice);

		productmapper.increaseUserPoint(params);
	}
```

#### AdminController.java 에 추가

```java
	// 주문 처리
	@PostMapping("/checkOrder")
	@ResponseBody
	public ResponseEntity<String> checkOrderGET(int ordernumber, HttpServletRequest request) {

		adminService.checkOrderChangeResult(ordernumber);

		return ResponseEntity.ok("Success");
	}

```

#### AdminController.java 에 추가

### 4. 취소 요청 -> 취소 처리 기능

회원으로부터 취소 요청이 온 주문에 대해서 취소 처리를 해주는 메서드를 만들어 준다.

```java
	// 취소 주문 확인
	@GetMapping("/withDrawOrderCheck")
	public String withDrawOrderCheckGET(int ordernumber, HttpServletRequest request) throws Exception {

		ArrayList<ProductVO> plist = productService.getAllProduct(); // 모든 상품 가져오기
		request.setAttribute("plist", plist);

		ArrayList<OrderVO> olist = userService.getMyPurchasedDetail(ordernumber); // 주문번호로 정보 가져오기
		request.setAttribute("olist", olist);

		return "/admin/withDrawOrderCheckProcess";
	}
```

#### withDrawOrderCheckProcess.jsp 추가

```java
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
			<h1 class="font-weight-semi-bold text-uppercase mb-3">WithDraw Order Process</h1>
			<div class="d-inline-flex">
				<p class="m-0">취소요청 주문 처리</p>
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
								<td class="align-middle">${plist.pname}</td>
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
			<button class="btn btn-block btn-primary my-3 py-3" style="width: 350px;" onclick="withdrawOrder('${ordernumber}', '${result}')">WithDraw Order</button>
			<button class="btn btn-block btn-primary my-3 py-3" style="width: 350px;" onclick="location.href='/admin/sales_OrderManagement'">Sales&Order List</button>
		</div>
	</div>
	<hr>
	<script>
		function withdrawOrder(ordernumber, orderResult) {
			if (orderResult === "4") {
				if (confirm("주문을 취소 처리 하시겠습니까?")) {
					$.ajax({
						url : `/admin/withdrawOrder?ordernumber=${ordernumber}`,
						type : 'POST',
						success : function(data) {
							if (data === "Success") {
								alert("취소 처리 되었습니다.");
								location.reload(); // 주문 상태를 업데이트하기 위해 페이지를 다시 로드합니다.
							} else {
								alert("취소 처리 중 문제가 발생했습니다.");
							}
						},
						error : function(err) {
							alert("주문 배송처리 요청이 실패했습니다.");
						}
					});
				}
			} else {
				alert("취소 요청할 수 없습니다.");
			}
		}
	</script>
</body>
<jsp:include page="../footer.jsp"></jsp:include>
</html>
```

![_config.yml]({{ site.baseurl }}/img/SpringDB/withdrawmanage.png)

#### AdminMapper.java 에 추가

```java
	public int withdrawOrderChangeResult(@Param("ordernumber") int ordernumber); // 주문 취소처리 result -> 5 으로 변경
```

#### AdminMapper.xml 에 추가

```xml
	<!-- 취소처리한 주문 orderdetail result 5으로 변경 -->
	<update id="withdrawOrderChangeResult">

		update order_detail set result = 5 where
		ordernumber=#{ordernumber}
	</update>
```

#### AdminService.java 에 추가

```java
	public int withdrawOrderChangeResult(@Param("ordernumber") int ordernumber); // 주문 취소처리 result -> 5 으로 변경
```

#### AdminServiceImpl.java 에 추가

```java
	@Override
	public int withdrawOrderChangeResult(int ordernumber) {
		
		return mapper.withdrawOrderChangeResult(ordernumber);
	}
```

#### AdminController.java 에 추가

```java
	// 취소처리 확인
	@PostMapping("/withdrawOrder")
	@ResponseBody
	public ResponseEntity<String> withdrawOrderGET(int ordernumber, HttpServletRequest request) {

		adminService.withdrawOrderChangeResult(ordernumber);

		return ResponseEntity.ok("Success");
	}
```

### 5. 판매 완료 상세 보기

#### AdminController.java 에 추가

```java
	// 판매 완료 주문 확인
	@GetMapping("/salesOrder")
	public String salesOrderGET(int ordernumber, HttpServletRequest request) throws Exception {

		ArrayList<ProductVO> plist = productService.getAllProduct(); // 모든 상품 가져오기
		request.setAttribute("plist", plist);

		ArrayList<OrderVO> olist = userService.getMyPurchasedDetail(ordernumber); // 주문번호로 정보 가져오기
		request.setAttribute("olist", olist);

		return "/admin/salesOrder";
	}
```

#### salesOrder.jsp 추가

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
			<h1 class="font-weight-semi-bold text-uppercase mb-3">Sales Order</h1>
			<div class="d-inline-flex">
				<p class="m-0">판매 완료 주문</p>
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
								<td class="align-middle">${plist.pname}</td>
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
			<button class="btn btn-block btn-primary my-3 py-3" style="width: 350px;" onclick="location.href='/admin/sales_OrderManagement'">Sales&Order List</button>
		</div>
	</div>
	<hr>
	
</body>
<jsp:include page="../footer.jsp"></jsp:include>
</html>
```

![_config.yml]({{ site.baseurl }}/img/SpringDB/salesorder.png)

## [프로젝트 주소](https://github.com/GreenteaPIE/TeamProjectDBSpringVer)
