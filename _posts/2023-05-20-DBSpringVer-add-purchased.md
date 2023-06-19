---
layout: post
title: 10 - DB Spring 상품 결제
date: 2023-05-20
excerpt: "팀 프로젝트 DB Spring Version"
tags: [project, java, jsp, Oracle, css, HTML, BootStrap, API, JQuery, JavaScript, Spring, FrameWork]
feature: /img/SpringDB/logo.png
comments: true


---


> **사용한 플랫폼 : Spring, Oracle**



### 1.  결제 기능 추가

#### cart.jsp 에 수정

```jsp
						<c:if test="${clist.size()!=0}">
							<a href="/product/checkOut?userid=${user.userid}">
								<input type="button" class="btn btn-block btn-primary my-3 py-3" value="Proceed To Checkout">
							</a>
						</c:if>
```

#### OrderVO 추가

```java
package com.db.model;

import java.sql.Timestamp;

import lombok.Data;

@Data
public class OrderVO { // orders table + order_detail table

	private int ordernumber; // seq
	private int orderdetailnumber; // seq
	private int num; // product number
	private int quantity;
	private int price;
	private String userid;
	private String psize;
	private String result;
	private Timestamp indate;
	private int totalprice;
	private String name, email, address1,address2,address3,phone;


}
```

orders Table과 order_detail Table를 Join으로 엮어서 주문내역을 열람하고 관리한다.

#### checkOut.jsp 추가

```jsp
<%@ page language="java" contentType="text/html; charset=utf-8" pageEncoding="utf-8"%>
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<!DOCTYPE html>
<html>
<head>
</head>
<jsp:include page="../header.jsp"></jsp:include>
<body>
	<hr>
	<!-- Page Header Start -->
	<div class="container bg-secondary mb-3" style="max-width: 800px;">
		<div class="d-flex flex-column align-items-center justify-content-center" style="min-height: 200px">
			<h1 class="font-weight-semi-bold text-uppercase mb-3">Checkout</h1>
			<div class="d-inline-flex">
				<p class="m-0">
					<a href="/">Home</a>
				</p>
			</div>
		</div>
	</div>
	<!-- Page Header End -->
	<!-- Checkout Start -->
	<form action="/product/purchased" method="post" name="frm" id="frm">
		<!-- 가지고 넘어가야 할 값들 
		cart = psize, num, quantity, price, cartnum -> 0 으로 변경
		product = imgurl, pname
		coupon = cnum -> 0으로 변경
		user = userid 
		totalprice
		 -->
		<input type="hidden" name="userid" value="${user.userid }">
		<!--<input type="hidden" name="totalprice" value="">
		 pname 과 imgurl 은 num(product)를 가져가서 productvo에서 불러오기 -->
		<div class="container-fluid pt-5">
			<div class="row px-xl-5">
				<div class="col-lg-8">
					<div class="mb-4">
						<h4 class="font-weight-semi-bold mb-4">Billng Address</h4>
						<div class="row">
							<div class="col-md-6 form-group">
								<label>이름</label> <input class="form-control" type="text" name="name" placeholder="${user.name }">
							</div>
							<div class="col-md-6 form-group">
								<label>E-mail</label> <input class="form-control" type="text" name="email" placeholder="${user.email }">
							</div>
							<div class="col-md-6 form-group">
								<label>전화번호</label> <input class="form-control" type="text" name="phone" placeholder="${user.phone }">
							</div>
							<div class="col-md-6 form-group">
								<label>우편번호</label> <input class="form-control address_input_1" type="text" name="address1" placeholder="${user.address1 }">
							</div>
							<div class="col-md-12 form-group">
								<label>주소</label> <input class="form-control address_input_2" type="text" name="address2" placeholder="${user.address2 }">
							</div>
							<div class="col-md-12 form-group">
								<label>상세주소</label> <input class="form-control address_input_3" type="text" name="address3" placeholder="${user.address3 }">
							</div>
							<div class="col-md-12 text-right">
								<input type="button" onclick="execution_daum_address()" class="rbutton xsmall white btn btn-primary" value="우편번호 찾기">
							</div>
							<div class="col-md-12 form-group">
								<div class="custom-control custom-checkbox">
									<input type="checkbox" class="custom-control-input" id="useraddress"> <label class="custom-control-label" for="useraddress">주문자 정보와 동일</label>
								</div>
							</div>
							<div class="col-md-12 form-group">
								<div class="custom-control custom-checkbox">
									<input type="checkbox" class="custom-control-input" id="shipto"> <label class="custom-control-label" for="shipto" data-toggle="collapse" data-target="#shipping-address">약관 동의</label>
								</div>
							</div>
						</div>
					</div>
					<div class="collapse mb-4" id="shipping-address">
						<h4 class="font-weight-semi-bold mb-4">이용약관</h4>
						<div class="row">
							<div class="col-md-6 form-group">
								<label>결제약관</label>
								<p>제1조 (목적) 이 약관은 "간편결제서비스회원"이 ㈜DIIB(이하 “DB”이라 합니다.)이 운영하는 매체(이하 “쇼핑몰”이라 한다)에서 "대상카드"를 이용하여 전자상거래를 이용 하는 경우 롯데카드 주식회사(이하 "회사"라 합니다.)가 "간편결제서비스회원"에게 제공하는 간편결제서비스(이하 "간편결제서비스"라 합니다.)의 이용조건 및 절차에 관한 기본사항을 정함을 목적으로 합니다.</p>
								<hr>
								<p>제2조 (용어의 정의) ① "대상카드"란 "간편결제서비스"를 적용하고자 하는 카드로서, "회사"가 발급한 VISA, Master, JCB, American Express Card, Local 신용카드를 소지한 개인 개별 회원, 체크카드 이용회원을 말합니다.</p>
							</div>
						</div>
					</div>
				</div>
				<div class="col-lg-4">
					<div class="card border-secondary mb-5">
						<div class="card-header bg-secondary border-0">
							<h4 class="font-weight-semi-bold m-0">Order Total</h4>
						</div>
						<div class="card-body">
							<c:set var="subprice" value="0" />
							<h5 class="font-weight-medium mb-3">Products</h5>
							<c:forEach var="cart" items="${clist}">
								<c:forEach var="plist" items="${plist}">
									<c:if test="${cart.num == plist.num}">
										<div class="d-flex justify-content-between">
											<p>${plist.pname}</p>
											<p>
												<fmt:setLocale value="ko_KR" />
												<fmt:formatNumber type="currency" value="${cart.price * cart.quantity}" currencySymbol="₩" />
											</p>
										</div>
										<input type="hidden" name="pname" id="pname" value="${plist.pname}">
										<input type="hidden" name="psize" value="${cart.psize }">
										<input type="hidden" name="quantity" value="${cart.quantity }">
										<input type="hidden" name="num" value="${cart.num }">
										<!-- cartnum 을 가져가서 result -> 0으로 변경하기 -->
										<input type="hidden" name="price" value="${cart.price }">
										<!-- 할인된 가격은 cart에 저장되어있기 때문에 주문상세에서 불러오려면 cart에 저장된 price를 들고가야함 -->
										<input type="hidden" name="cartnum" value="${cart.cartnum }">
										<c:set var="subprice" value="${subprice + cart.price * cart.quantity}" />
									</c:if>
								</c:forEach>
							</c:forEach>
							<br>
							<hr class="mt-0">
							<div class="d-flex justify-content-between mb-3 pt-1">
								<h6 class="font-weight-medium">Subtotal</h6>
								<h6 class="font-weight-medium" id="subtotalPrice">
									<fmt:setLocale value="ko_KR" />
									<fmt:formatNumber type="currency" value="${subprice}" currencySymbol="₩" />
								</h6>
							</div>
							<div class="d-flex justify-content-between">
								<h6 class="font-weight-medium">Shipping</h6>
								<h6 class="font-weight-medium">
									<strong style="font-size: 20px">Free</strong>
								</h6>
							</div>
						</div>
						<div class="card-footer border-secondary bg-transparent">
							<div class="d-flex justify-content-between mb-2">
								<h5 class="font-weight-bold">Total</h5>
								<h5 class="font-weight-bold" id="totalDisplayPrice">
									<fmt:setLocale value="ko_KR" />
									<fmt:formatNumber type="currency" value="${subprice}" currencySymbol="₩" />
								</h5>
							</div>
							<div>
								<div class="d-flex justify-content-between mb-2">
									<h6 class="font-weight-medium">Discount Price</h6>
									<h6 class="font-weight-medium" id="DiscountPrice">₩0</h6>
								</div>
							</div>
						</div>
					</div>
					<div class="input-group mb-5">
						<div class="form-control form-control-custom">
							<select name="coupon" class="select-no-outline">
								<option value="0">쿠폰 선택 안함</option>
								<c:forEach var="coup" items="${couplist}">
									<c:if test="${coup.couponresult != 0 }">
										<option value="${coup.discountprice}" data-num="${coup.cnum}">${coup.couponname}</option>
										<c:set var="totalprice" value="${subprice - coup.discountprice}" />
									</c:if>
								</c:forEach>
							</select> <input id="total" type="hidden" name="totalprice" value="${totalprice }" step="1"> <input type="hidden" name="cnum" value="">
							<!-- cnum 쿠폰등록 번호 를 가져가서 result ->0 로 변경하기 -->
						</div>
						<div class="input-group-append">
							<button class="btn btn-primary">Select Coupon</button>
						</div>
					</div>
					<div class="card border-secondary mb-5">
						<div class="card-header bg-secondary border-0">
							<h4 class="font-weight-semi-bold m-0">Payment</h4>
						</div>
						<div class="card-body">
							<div class="form-group">
								<div class="custom-control custom-radio">
									<input type="radio" class="custom-control-input" name="payment" id="paypal"> <label class="custom-control-label" for="paypal">신용카드</label>
								</div>
							</div>
							<div class="form-group">
								<div class="custom-control custom-radio">
									<input type="radio" class="custom-control-input" name="payment" id="directcheck"> <label class="custom-control-label" for="directcheck">삼성페이</label>
								</div>
							</div>
							<div class="">
								<div class="custom-control custom-radio">
									<input type="radio" class="custom-control-input" name="payment" id="banktransfer"> <label class="custom-control-label" for="banktransfer">무통장입금</label>
								</div>
							</div>
						</div>
						<div class="card-footer border-secondary bg-transparent">
							<button class="btn btn-lg btn-block btn-primary font-weight-bold my-3 py-3 " id="checkout-button">Place Order</button>
						</div>
					</div>
				</div>
			</div>
		</div>
	</form>
	<!--Checkout End -->
	<hr>
</body>
<jsp:include page="../footer.jsp"></jsp:include>
</html>
```

![_config.yml]({{ site.baseurl }}/img/SpringDB/purchased.png)

주문자 정보와 동일을 누르면 placeholder로 입력되어 있는 값이 Textbox 안으로 입력되고 다른 배송지로 주문 할 수 있도록 주소록 API 를 추가해준다.

약관의 동의하지 않았거나 결제 방법을 선택 하지 않으면 Form이 제출되지 않게 하고, 쿠폰 선택 시 상품 가격보다 쿠폰의 할인가가 높다면 해당 쿠폰을 사용 할 수 없게한다.

결제는 아임포트 라이브러리를 사용하여 결제를 진행 하게 하고, 테스트를 위해 결제가 완료되지 않아도 Form이 제출되게 만든다.

#### Script 추가

```jsp
<script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
<script src="//t1.daumcdn.net/mapjsapi/bundle/postcode/prod/postcode.v2.js"></script>
```

다음의 주소록 api 를 추가한다.

```js
	 
	// 다음 주소 연동
		function execution_daum_address() {

			new daum.Postcode(
					{
						oncomplete : function(data) {
							// 팝업에서 검색결과 항목을 클릭했을때 실행할 코드를 작성하는 부분.

							// 각 주소의 노출 규칙에 따라 주소를 조합한다.
							// 내려오는 변수가 값이 없는 경우엔 공백('')값을 가지므로, 이를 참고하여 분기 한다.
							var addr = ''; // 주소 변수
							var extraAddr = ''; // 참고항목 변수

							//사용자가 선택한 주소 타입에 따라 해당 주소 값을 가져온다.
							if (data.userSelectedType === 'R') { // 사용자가 도로명 주소를 선택했을 경우
								addr = data.roadAddress;
							} else { // 사용자가 지번 주소를 선택했을 경우(J)
								addr = data.jibunAddress;
							}

							// 사용자가 선택한 주소가 도로명 타입일때 참고항목을 조합한다.
							if (data.userSelectedType === 'R') {
								// 법정동명이 있을 경우 추가한다. (법정리는 제외)
								// 법정동의 경우 마지막 문자가 "동/로/가"로 끝난다.
								if (data.bname !== ''
										&& /[동|로|가]$/g.test(data.bname)) {
									extraAddr += data.bname;
								}
								// 건물명이 있고, 공동주택일 경우 추가한다.
								if (data.buildingName !== ''
										&& data.apartment === 'Y') {
									extraAddr += (extraAddr !== '' ? ', '
											+ data.buildingName
											: data.buildingName);
								}
								// 표시할 참고항목이 있을 경우, 괄호까지 추가한 최종 문자열을 만든다.
								if (extraAddr !== '') {
									extraAddr = ' (' + extraAddr + ')';
								}
								// 조합된 참고항목을 해당 필드에 넣는다.
								// document.getElementById("sample6_extraAddress").value = extraAddr;
								addr += extraAddr;

							} else {
								//document.getElementById("sample6_extraAddress").value = '';
								addr += ' ';
							}

							// 우편번호와 주소 정보를 해당 필드에 넣는다.
							//document.getElementById('sample6_postcode').value = data.zonecode;
							//document.getElementById("sample6_address").value = addr;
							// 커서를 상세주소 필드로 이동한다.
							$(".address_input_1").val(data.zonecode);
							$(".address_input_2").val(addr);

							//  document.getElementById("sample6_detailAddress").focus();

							// 상세주소 입력란 disabled 속성 변경 및 커서를 상세주소 필드로 이동한다.
							$(".address_input_3").attr("readonly", false);
							$(".address_input_3").focus();
						}
					}).open();
		}
```

아래는 주문자 정보를 클릭 했을 때 유저의 정보를 입력시킨다.

```js
	 //주문자 정보 동일
	 $(document).ready(function () {
    $("#useraddress").on("change", function () {
      if ($(this).is(":checked")) {
        $("input[name='name']").val($("input[name='name']").attr("placeholder")).prop("readonly", true);
        $("input[name='email']").val($("input[name='email']").attr("placeholder")).prop("readonly", true);
        $("input[name='phone']").val($("input[name='phone']").attr("placeholder")).prop("readonly", true);
        $("input[name='address1']").val($("input[name='address1']").attr("placeholder")).prop("readonly", true);
        $("input[name='address2']").val($("input[name='address2']").attr("placeholder")).prop("readonly", true);
        $("input[name='address3']").val($("input[name='address3']").attr("placeholder")).prop("readonly", true);
      } else {
        // Restore input fields to be editable and remove the value if the checkbox is unchecked.
        $("input[type='text']").val("").prop("readonly", false);
      }
    });
  });
```

아래는 유저의 쿠폰 정보중 할인가격을 확인하여 구매할 상품의 가격보다 높다면 해당 쿠폰을 사용 할 수 없게 한다.

```js
//쿠폰
function numberWithCommas(x) {
	  return x.toString().replace(/\B(?<!\.\d*)(?=(\d{3})+(?!\d))/g, ",");
	}


$(document).ready(function () {
	  $("button.btn.btn-primary").on("click", function (e) {
		  e.preventDefault();

	    var discountPrice = parseFloat($("select[name='coupon']").val());
	    var coupNum = $("select[name='coupon'] option:selected").data("num");
	    var subPrice = parseFloat(
	      $("#subtotalPrice").text().replace(/[^0-9.]/g, "")
	    );
	    var totalPrice = parseFloat(
	      $("#totalDisplayPrice").text().replace(/[^0-9.]/g, "")
	    );

	    if (subPrice - discountPrice < 0) {
	      alert("상품 가격 보다 높은 할인쿠폰을 사용 하실 수 없습니다");
	      $("select[name='coupon']").prop("selectedIndex", 0);
	    } else {
	      var newTotalPrice = subPrice - discountPrice;

	      // Update the discount price
	      $("#DiscountPrice").html(
	        "₩" + numberWithCommas(discountPrice.toFixed(0))
	      );

	      $("#subtotalPrice").html("₩" + numberWithCommas(subPrice.toFixed(0)));
	      $("#totalDisplayPrice").html(
	        "₩" + numberWithCommas(newTotalPrice.toFixed(0))
	      );

	      // Update the hidden total input value
	      $("#total").val(newTotalPrice);

	      // Assign the coupNum value to the input field with the 'cnum' name
	      $("input[name='cnum']").val(coupNum);
	    }
	  });
	});
```

아래는 쿠폰을 선택하지 않고 결제 진행을 하려고 하면 쿠폰할인가를 적용한 최종가격을 0원으로 인식하여 결제가 진행되지 않아 select 버튼을 클릭하지 않아도 최종가격을 가지고 갈 수 있도록 해준다.

```js
 document.addEventListener('DOMContentLoaded', function() {
		    // select 엘리먼트를 가져옵니다
		    const selectElement = document.getElementsByName('coupon')[0];

		    // 버튼 엘리먼트를 가져옵니다
		    const button = document.querySelector('.input-group-append .btn');

		    // select 값에 따라 버튼을 활성화/비활성화하는 함수
		    function checkSelectedValue() {
		      if (selectElement.value === "") {
		        button.disabled = true;
		      } else {
		        button.disabled = false;
		      }
		    }

		    // 함수를 처음 호출하여 버튼의 초기 상태를 설정합니다
		    checkSelectedValue();

		    // 변경 사항을 감시하기 위해 select 엘리먼트에 이벤트 리스너를 추가합니다
		    selectElement.addEventListener('change', checkSelectedValue);
		  });
```

아래는 아임포트 결제 API 이다.

```js
<%--아임포트 라이브러리--%>
<script type="text/javascript" src="https://cdn.iamport.kr/js/iamport.payment-1.1.5.js"></script>

//결제 
	
const shiptoCheckbox = document.getElementById('shipto');
const paymentRadios = document.getElementsByName('payment');
const placeOrderBtn = document.getElementById('checkout-button');

placeOrderBtn.addEventListener('click', function(event) {
	  event.preventDefault(); // 폼 제출을 막음

	  // Select Coupon 버튼 클릭 이벤트를 강제로 실행
	  $('button.btn.btn-primary').click();

	  if (!shiptoCheckbox.checked) {
	    alert('약관에 동의해주세요.');
	  } else if (!Array.from(paymentRadios).some(radio => radio.checked)) {
	    alert('결제 방법을 선택해주세요.');
	  } else {
	    // 결제 요청 함수 호출
	    iamport();
	  }
	});
function iamport() {
    var total = parseFloat($("#total").val()); // parseFloat로 숫자로 변환
    var pname = $("#pname").val();
    // 가맹점 식별코드
    IMP.init('imp36864364');
    
    // 결제 요청 함수 호출
    IMP.request_pay({
        pg: 'html5_inicis',
        pay_method: 'card',
        merchant_uid: 'merchant_' + new Date().getTime(),
        name: pname, // 결제창에서 보여질 이름
        amount: total, // 실제 결제되는 가격
        buyer_email: 'iamport@siot.do',
        buyer_name: '구매자이름',
        buyer_tel: '010-1234-5678',
        buyer_addr: '서울 강남구 대치동',
        buyer_postcode: '123-456'
    }, function(rsp) {
        if (rsp.success) {
            // 결제 성공 처리
            var msg = '결제가 완료되었습니다.';
        
            alert(msg);
            document.getElementById('frm').submit(); // 폼 제출
        } else {
            // 결제 실패 처리
            var msg = '결제에 실패하였습니다.';
            msg += '에러내용 : ' + rsp.error_msg;
            alert(msg);
            document.getElementById('frm').submit(); // 폼 제출
        }
    });
}
```

 원래는 결제에 실패하면 결제 실패 alert 만 띄워줘야 하지만, 테스트를 위해 Form이 제출되게 만든다.

#### ProductController.java 에 추가

```java
	@Autowired
	UserService userService;

// 체크아웃 페이지
	@GetMapping("/checkOut")
	public String checkOutGET(String userid, HttpServletRequest request, HttpSession session) throws Exception {

		ArrayList<CartVO> clist = productService.getCartList(userid); // userid로 장바구니 리스트 불러오기
		request.setAttribute("clist", clist);

		ArrayList<ProductVO> plist = productService.getAllProduct(); // 모든 상품 가져오기
		request.setAttribute("plist", plist);

		ArrayList<CouponVO> couplist = userService.getMyCoupon(userid); // 쿠폰 정보 불러오기
		request.setAttribute("couplist", couplist);
		return "/product/checkOut";
	}
```

장바구니에서 최종적으로 보낸 상품의 정보와 장바구니 정보, 유저의 쿠폰 정보를 불러온다.

```java
	// 결제완료
	@PostMapping("/purchased")
	public String purchasedPOST(@Param("cnum") Integer cnum, @Param("cartnum") int cartnum,
			@Param("userid") String userid, @Param("totalprice") int totalprice, @Param("name") String name,
			@Param("email") String email, @Param("phone") String phone, @Param("address1") String address1,
			@Param("address2") String address2, @Param("address3") String address3, HttpServletRequest request)
			throws Exception {

		if (cnum != null) {
			productService.useCoupon(cnum); // 쿠폰 적용후 쿠폰 result값 변경
		}
		int orderNumber = productService.getLatestOrderNumber(userid);// orderNumber를 가져옴

		ArrayList<CartVO> cartlist = productService.getCartList(userid);
		for (CartVO cart : cartlist) {
			productService.addOrderDetail(cart, totalprice, orderNumber, name, phone, email, address1, address2,
					address3); // order_detail table에 저장
			productService.cartResultChange(cartnum, cart);
			// 주문완료한 장바구니 result -> 0 으로 변경

		}

		ArrayList<ProductVO> plist = productService.getAllProduct();
		request.setAttribute("plist", plist); // 상품정보 불러오고 저장

		ArrayList<OrderVO> olist = productService.getOrderList(orderNumber);
		request.setAttribute("olist", olist); // 마지막 주문정보를 불러오고 저장

		return "/product/orderList";
	}
```

구매 완료를 진행하는 메소드의 순서는 orders table에 저장 -> orders table의 마지막 생성 주문번호와 함께 order_detail table에 저장 -> 사용한 쿠폰의 result 를 0으로 변경 -> 주문을 완료한 장바구니의 result를 0으로 변경 -> 주문 완료 페이지 단계이다.

아래는 쿠폰을 적용하여 결제를 진행 했을 때 해당 쿠폰의 result 값을 0으로 변경하여 사용 했음을 나타나게 해준다.

#### ProductMapper.java 에 추가

```java
	// 사용한 쿠폰 result -> 0 으로 변경
	public int useCoupon(@Param("cnum") int cnum);
```

#### ProductService.java 에 추가

```java
	// 사용한 쿠폰 result -> 0 으로 변경
	public int useCoupon(int cnum) throws Exception;
```

#### ProductServiceImpl 에 추가

```java
	@Override
	public int useCoupon(int cnum) throws Exception {

		return productmapper.useCoupon(cnum);
	}
```

#### ProductMapper.xml 에 추가

```xml
	<!-- 사용한 coupon result 0으로 변경 -->
	<update id="useCoupon">
		update coupon set couponresult = 0 where cnum=#{cnum}
	</update>
```

orders 와 order_detail 두개의 테이블에 동시에 데이터 값을 넣기 위해 oders Table에 먼저 주문정보를 저장하고 마지막 주문번호를 불러와 함께 상세 주문 정보를 order_detail Table에 넣어준다.

아래는 주문 정보를 저장하는 메소드 이다.

#### ProductMapper.java 에 추가

```java
	// 결제 정보 추가(orders table)
	public void addOrders(String userid);
```

#### ProductService.java 에 추가

```java
	// 결제 정보 추가(orders table)
	public void addOrders(String userid) throws Exception;
```

#### ProductServiceImpl.java 에 추가

```java
	@Override
	public void addOrders(String userid) throws Exception {

		productmapper.addOrders(userid);
	}
```

#### ProductMapper.xml 에 추가

```xml
	<!-- 결제정보 추가(orders table) -->
	<insert id="addOrders">
		insert into
		orders(ordernumber, userid)
		values(orders_seq.nextval,
		#{userid})
	</insert>
```

아래는 저장된 마지막 주문번호를 불러오는 메소드 이다.

#### ProductMapper.java 에 추가

```java
	// orders table에 추가된 ordernumber 값 가져오기
	public int getLatestOrderNumber(String userid);
```

#### ProductService.java 에 추가

```java
	// orders table에 추가된 ordernumber 값 가져오기
	public int getLatestOrderNumber(String userid) throws Exception;
```

#### ProductServiceImpl.java 에 추가

orders Table에 값을 저장하면서 마지막 주문번호를 반환 해준다.

```java
	@Override
	public int getLatestOrderNumber(String userid) throws Exception {
		productmapper.addOrders(userid);
		// orders 테이블에 입력후 새로 생성된 주문 번호(ordernumber)를 반환함.
		return productmapper.getLatestOrderNumber(userid);
	}
```

#### ProductMapper.xml 에 추가

```xml
	<!-- orders table ordernumber값 가져오기 -->
	<select id="getLatestOrderNumber" resultType="int">
		SELECT
		MAX(orderNumber) FROM orders WHERE userid=#{userid}
	</select>
```

아래는 order_detail Table 에 상세 주문 정보를 저장하는 메소드 이다.

#### ProductMapper.java 에 추가

```java
	// order detail 테이블에 추가
	public void addOrderDetail(@Param("cart") CartVO cart, @Param("totalprice") int totalprice,
			@Param("ordernumber") int ordernumber, @Param("name") String name, @Param("phone") String phone,
			@Param("email") String email, @Param("address1") String address1, @Param("address2") String address2,
			@Param("address3") String address3);
```

#### ProductService.java 에 추가

```java
	// order detail 테이블에 추가
	public void addOrderDetail(CartVO cart, int totalprice, int ordernumber, String name, String phone, String email,
			String address1, String address2, String address3) throws Exception;
```

#### ProductServiceImpl.java 에 추가

```java
	@Override
	public void addOrderDetail(CartVO cart, int totalprice, int ordernumber, String name, String phone, String email,
			String address1, String address2, String address3) throws Exception {
		productmapper.addOrderDetail(cart, totalprice, ordernumber, name, phone, email, address1, address2, address3);
	}
```

#### ProductMapper.xml 에 추가

```xml
	<!-- order detail 추가 -->
	<insert id="addOrderDetail">
		insert into order_detail(
		orderdetailnumber,
		ordernumber,num, quantity, price,
		psize, totalprice, name, email,
		address1, address2, address3, phone
		) values (
		order_detail_seq.nextval, #{ordernumber},#{cart.num},
		#{cart.quantity}, #{cart.price}, #{cart.psize}, #{totalprice},
		#{name}, #{email},
		#{address1},
		#{address2}, #{address3}, #{phone}
		)
	</insert>
```

아래는 주문을 완료한 장바구니의 result 를 0으로 바꾸어 장바구니에 노출이 되지않게 만든다.

#### ProductMapper.java 에 추가

```java
	// 주문완료한 장바구니 result -> 0 으로 변경
	public int cartResultChange(@Param("cartnum") int cartnum, @Param("cart") CartVO cart);
```

#### ProductService.java 에 추가

```java
	// 주문완료한 장바구니 result ->0 으로 변경
	public int cartResultChange(@Param("cartnum") int cartnum, @Param("cart") CartVO cart) throws Exception;
```

#### ProductServiceImpl.java 에 추가

```java
	@Override
	public int cartResultChange(@Param("cartnum") int cartnum, @Param("cart") CartVO cart) throws Exception {

		return productmapper.cartResultChange(cartnum, cart);
	}
```

#### ProductMapper.xml 에 추가

```xml
	<!-- 주문완료한 cart result 0으로 변경 -->
	<update id="cartResultChange">

		update cart set result = 0 where
		cartnum=#{cart.cartnum}
	</update>
```

아래는 orders 와 order_detail 두개의 Table을 Join 하여 order_view 라는 가상 Table 에서 주문 정보들을 불러온다.

#### ProductMapper.java 에 추가

```java
	// order_view 에서 정보 가져오기
	public ArrayList<OrderVO> getOrderList(int orderNumber);
```

#### ProductService.java 에 추가

```java
	// order_view 에서 정보 가져오기
	public ArrayList<OrderVO> getOrderList(int orderNumber) throws Exception;
```

#### ProductServiceImpl.java 에 추가

```java
	@Override
	public ArrayList<OrderVO> getOrderList(int orderNumber) throws Exception {

		return productmapper.getOrderList(orderNumber);
	}
```

#### ProductMapper.xml 에 추가

```xml
	<!-- order_view 에서 정보가져오기 -->
	<select id="getOrderList" resultType="com.db.model.OrderVO">
		select * from order_view
		where ordernumber = #{ordernumber}
	</select>
```

#### oderList.jsp 추가

주문이 완료되면 아래 페이지로 이동되고 주문내역을 볼 수 있다.

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
			<h1 class="font-weight-semi-bold text-uppercase mb-3">Order List</h1>
			<div class="d-inline-flex">
				<p class="m-0">주문해주셔서 감사합니다.</p>
			</div>
		</div>
	</div>
	<!-- Page Header End -->
	<div class="container bg-secondary mb-3" style="max-width: 800px;">
		<table class="table table-bordered text-center mb-0">
			<thead class="bg-secondary text-dark">
				<tr>
					<th>Products</th>
					<th>Quantity</th>
					<th>Price</th>
					<th>Order Date</th>
					<th>Status</th>
				</tr>
			</thead>
			<tbody class="align-middle">
				<c:forEach var="olist" items="${olist }">
					<c:forEach var="plist" items="${plist }">
						<c:if test="${olist.num == plist.num}">
							<tr>
								<td class="align-middle">${plist.pname }</td>
								<td class="align-middle">${olist.quantity }</td>
								<td class="align-middle"><fmt:formatNumber value="${olist.price * olist.quantity}" type="currency" /></td>
								<td class="align-middle"><fmt:formatDate value="${olist.indate }" type="date" /></td>
								<td class="align-middle"><c:choose>
										<c:when test='${olist.result  =="1" }'>주문 확인 중</c:when>
									</c:choose></td>
							</tr>
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
					<th>Total Price</th>
					<td colspan="4"><fmt:formatNumber value="${totalPrice}" type="currency" /></td>
				<tr>
			</tbody>
		</table>
		<div class="card-footer border-secondary bg-transparent" align="center">
			<button class="btn btn-block btn-primary my-3 py-3" style="width: 350px;" onclick="location.href='/'">Continue shopping</button>
		</div>
	</div>
	<hr>
</body>
<jsp:include page="../footer.jsp"></jsp:include>
</html>
```

![_config.yml]({{ site.baseurl }}/img/SpringDB/pursuc.png)

## [프로젝트 주소](https://github.com/GreenteaPIE/TeamProjectDBSpringVer)
