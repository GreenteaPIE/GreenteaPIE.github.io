---
layout: post
title: 6 - DB Spring 상품 리스트와 상품 검색
date: 2023-05-16
excerpt: "팀 프로젝트 DB Spring Version"
tags: [project, java, jsp, Oracle, css, HTML, BootStrap, API, JQuery, JavaScript, Spring, FrameWork]
feature: /img/SpringDB/logo.png
comments: true



---


> **사용한 플랫폼 : Spring, Oracle**

### 1. 브랜드 리스트 불러오기

#### com.db.model 에 BrandVO 추가

```java
package com.db.model;

import lombok.Data;

@Data
public class BrandVO {
	private String bname;
	private String imgurl;

}
```

Getter, Setter, ToString 은 @Data lombok 어노테이션으로 대체

#### HomeController.java 수정

```java
@Autowired
	ProductService productService;

@GetMapping("/")
	public String home(Locale locale, Model model, HttpServletRequest request, HttpSession session) {
		logger.info("Welcome home! The client locale is {}.", locale);

		Date date = new Date();
		DateFormat dateFormat = DateFormat.getDateTimeInstance(DateFormat.LONG, DateFormat.LONG, locale);

		String formattedDate = dateFormat.format(date);

		model.addAttribute("serverTime", formattedDate);

		try {
			ArrayList<BrandVO> bvo = productService.brandList();
			session.setAttribute("blist", bvo); // 브랜드 리스트를 세션에 불러옴

		} catch (Exception e) {
			e.printStackTrace();
		}

		return "home";
	}
```

접속 시 브랜드 리스트를 바로 불러와야 하기 때문에 HomeController에 리스트를 호출하는 메서드를 작성한다.

#### ProductMapper.java 생성 후 추가

```java
package com.db.mapper;

import java.util.ArrayList;

import com.db.model.BrandVO;

public interface ProductMapper {
	// 메인페이지, 헤더 브랜드 리스트
		public ArrayList<BrandVO> brandList();
}
```

#### ProductService.java 생성 후 추가

```java
package com.db.service;

import java.util.ArrayList;

import com.db.model.BrandVO;

public interface ProductService {
	// 메인 브랜드 리스트
	public ArrayList<BrandVO> brandList() throws Exception;
}
```

#### ProductServiceImpl.java 생성 후 추가

```java
package com.db.service;

import java.util.ArrayList;

import org.springframework.beans.factory.annotation.Autowired;

import com.db.mapper.ProductMapper;
import com.db.model.BrandVO;

public class ProductServiceImpl implements ProductService{
	@Autowired
	ProductMapper productmapper;

	@Override
	public ArrayList<BrandVO> brandList() throws Exception {

		return productmapper.brandList();
	}
}
```

#### ProductMapper.xml 생성 후 추가

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.db.mapper.ProductMapper">

	<!-- 메인 브랜드 리스트 -->
	<select id="brandList" resultType="com.db.model.BrandVO">

		select * from brand

	</select>
</mapper>
```

#### home.jsp에 추가

```jsp
<!-- Categories Start -->
	<div class="container-fluid pt-5 ">
		<div class="row px-xl-5 pb-3">
			<c:forEach var="blist" items="${blist }">
				<div class="col-lg-4 col-md-6 pb-1">
					<div class="cat-item text-center" style="padding: 30px;">
						<p class="text-right"></p>
						<a href="#" class="cat-img position-relative overflow-hidden mb-3">
							<img class="img-fluid" src="resources/img/${blist.imgurl}" alt="">
						</a>
						<h5 class="font-weight-semi-bold m-0"></h5>
					</div>
				</div>
			</c:forEach>
		</div>
	</div>
	<!-- Categories End -->
```

![_config.yml]({{ site.baseurl }}/img/SpringDB/brandlist.png)

임의로 작성해둔 브랜드들을 Brand 테이블에 insert 한 뒤 메인페이지에서 불러온 브랜드 리스트를 확인 할 수 있다.

![_config.yml]({{ site.baseurl }}/img/SpringDB/mainpage.png)

#### header.jsp에 추가

```jsp
<c:forEach var="blist" items="${blist }">
							<div class="nav-item dropdown">
								<a href="#" class="nav-link" data-toggle="dropdown">${blist.bname }
									<i class="fa fa-angle-down float-right mt-1"></i>
								</a>
								<div class="dropdown-menu position-absolute bg-secondary border-0 rounded-0 w-100 m-0">
									<a href="#" class="dropdown-item">TOP</a>
									<a href="#" class="dropdown-item">BOTTOM</a>
									<a href="#" class="dropdown-item">boutique collection</a>
								</div>
							</div>
						</c:forEach>
```

![_config.yml]({{ site.baseurl }}/img/SpringDB/headerbrand.png)

### 2. 브랜드 상품 리스트 불러오기

#### com.db.model 에 ProductVO 추가

```java
package com.db.model;

import java.sql.Timestamp;

import lombok.Data;

@Data
public class ProductVO {
	private int num;
	private int pGender;
	private String bname;
	private int kind;
	private String pname;
	private String imgUrl;
	private String psize;
	private int balance;
	private int price;
	private int purchasedNum;
	private String explain;
	private Timestamp writedate;
	private int readcount;
	private int discountrate;

}
```

#### ProductController.java 생성 후 추가

```java
package com.db.controller;

import java.util.ArrayList;

import javax.servlet.http.HttpServletRequest;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;

import com.db.model.ProductVO;
import com.db.service.ProductService;

@Controller
@RequestMapping(value = "/product")
public class ProductController {
	private static final Logger logger = LoggerFactory.getLogger(UserController.class);

	@Autowired
	ProductService productService;
	
	// 브랜드 상품 리스트 페이지 이동
		@GetMapping("brandProductList")
		public void brandProductListGET(String bname, HttpServletRequest request) {
			logger.info("브랜드 상품 리스트 페이지 진입");
			request.setAttribute("bname", bname);

			try {
				ArrayList<ProductVO> bplist = productService.brandProductList(bname);
				request.setAttribute("bplist", bplist); // 브랜드명으로 상품을 불러옴
			} catch (Exception e) {

				e.printStackTrace();
			}

		}
}
```

브랜드의 상품들의 정보를 bplist 에 담아 request 객체에 추가 해주고, 선택한 브랜드의 명칭을 brandProductList.jsp 에 출력하게 하기 위해 bname도 함께 request 객체에 추가 해준다. 

#### home.jsp 에 수정

```jsp
<a href='/product/brandProductList?bname=<c:out value="${blist.bname }"/>' class="cat-img position-relative overflow-hidden mb-3">
							<img class="img-fluid" src="resources/img/${blist.imgurl}" alt="">
						</a>
```

home.jsp 에서 불러온 브랜드 로고를 클릭하면 브랜드명(bname을) 가지고 호출하게 한다.

#### ProductMapper.java 에 추가

```java
// 브랜드 상품 리스트
	public ArrayList<ProductVO> brandProductList(String bname);
```

#### ProductService.java 에 추가

```java
// 브랜드 상품 리스트
	public ArrayList<ProductVO> brandProductList(String bname) throws Exception;
```

#### ProductServiceImpl.java 에 추가

```java
@Override
	public ArrayList<ProductVO> brandProductList(String bname) throws Exception {

		return productmapper.brandProductList(bname);
	}
```

#### ProductMapper.xml 에 추가

```xml
<!-- 브랜드 상품 리스트 -->
	<select id="brandProductList"
		resultType="com.db.model.ProductVO">

		SELECT num, pGender, bName, kind, pName, imgUrl, pSize,
		balance,
		price, purchasedNum, explain, writedate, readcount,
		discountrate FROM
		(SELECT num,
		pGender, bName, kind, pName, imgUrl,
		pSize, balance,
		price,
		purchasedNum, explain, writedate, readcount,
		discountrate, ROW_NUMBER() OVER
		(PARTITION BY pName ORDER BY num) RN
		FROM PRODUCT WHERE bname LIKE
		#{bname} )WHERE RN = 1

	</select>
```

임의로 Product Table에 임포트 시킨 상품들을 size 별로 복수 추가를 했기 때문에 단순히 브랜드명(bname)을 가지고 불러온다면 중복된 상품들도 함께 불러와지기 때문에 ROW_NUMBER() 함수를 사용해 각 행에 독립적인 숫자를 할당하고, PARTITION BY 으로 중복되는 pname을 기준으로 숫자를 할당시킨다. <br>ORDER BY num 으로 숫자를 오름차순 정렬 시킨 후 WHERE RN =1을 사용해 각 파티션에서 할당된 숫자가 1인 행만 선택하고 반환 하게 하여 중복되지 않게 모든 상품을 불러올 수 있다.

#### 상품 리스트 불러올 brandProductList.jsp 추가

```jsp
<%@ page language="java" contentType="text/html; charset=utf-8" pageEncoding="utf-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt"%>
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
			<h1 class="font-weight-semi-bold text-uppercase mb-3">${bname }</h1>
			<div class="d-inline-flex">
				<p class="m-0">
					<a href="/">Home</a>
				</p>
			</div>
		</div>
		<c:if test="${bplist.size()==0 }">
			<h3 style="text-align: center;">상품이 존재하지 않습니다.</h3>
		</c:if>
	</div>
	<!-- Page Header End -->
	<!-- Shop Start -->
	<div id="my-container" class="container-fluid pt-5">
		<!-- Shop Product Start -->
		<div class="col-lg-9 col-md-12">
			<div class="row pb-3">
				<c:forEach var="bplist" items="${bplist }">
					<c:if test="${bplist.discountrate == 0 }">
						<div class="col-lg-4 col-md-6 col-sm-12 pb-1">
							<div class="card product-item border-0 mb-4">
								<div class="card-header product-img position-relative overflow-hidden bg-transparent border p-0">
									<a href="#">
										<img class="img-fluid w-100" style="height: 240px" src="../resources/img/${bplist.imgUrl}" alt="">
									</a>
								</div>
								<div class="card-body border-left border-right text-center p-0 pt-4 pb-3">
									<h6 class="mb-3">${bplist.pname}</h6>
									<div class="d-flex justify-content-center">
										<%-- 통화 단위 지정 --%>
										<h6>
											<fmt:formatNumber value="${Integer.parseInt(bplist.price)}" pattern="₩###,###" />
										</h6>
									</div>
								</div>
							</div>
						</div>
					</c:if>
				</c:forEach>
			</div>
		</div>
		<!-- Shop Product End -->
	</div>
	<!-- Shop End -->
	<hr>
</body>
<jsp:include page="../footer.jsp"></jsp:include>
</html>
```

불러올 상품의 브랜드 명을 불러온 상품이 없을 땐 "상품이 존재 하지 않습니다."를 페이지에 띄우게 만든다.

상품 할인율을 1이상 설정한 상품은 Sale 페이지에만 불러오게 할 예정이기 때문에 discountrate 가 0인 상품만 불러오게 만든다.

### 3. 카테고리 별 상품 리스트 불러오기

#### header.jsp 수정

```jsp
<div class="dropdown-menu position-absolute bg-secondary border-0 rounded-0 w-100 m-0">
									<a href='/product/categoriesList?bname=<c:out value="${blist.bname }"/>&kind=1' class="dropdown-item">TOP</a>
									<a href='/product/categoriesList?bname=<c:out value="${blist.bname }"/>&kind=2' class="dropdown-item">BOTTOM</a>
									<a href='/product/categoriesList?bname=<c:out value="${blist.bname }"/>&kind=3' class="dropdown-item">boutique collection</a>
								</div>
```

브랜드명(bname) 과 카테고리(kind) 를 가지고 /product/categoriesList 를 호출한다.

#### ProductController.java 에 추가

```java
	// 브랜드 카테고리 상품 리스트
	@GetMapping("/categoriesList")
	public String categoriesListGET(String bname, int kind, HttpServletRequest request) throws Exception {
		
		request.setAttribute("bname", bname); 
		
		ArrayList<ProductVO> bplist = productService.categoriesList(bname, kind);
		request.setAttribute("bplist", bplist);
		
		return "/product/brandProductList";
	}
```

#### ProductMapper.java 에 추가

```java
	// 브랜드 카테고리 리스트
	public ArrayList<ProductVO> categoriesList(@Param("bname") String bname, @Param("kind") int kind);
```

#### ProductService.java 에 추가

```java
// 브랜드 카테고리 리스트
	public ArrayList<ProductVO> categoriesList(String bname, int kind) throws Exception;
```

#### ProductServiceImpl.java 에 추가

```java
	@Override
	public ArrayList<ProductVO> categoriesList(String bname, int kind) throws Exception {
		
		return productmapper.categoriesList(bname, kind);
	}
```

#### ProductMapper.xml

```xml
<!-- 브랜드 상품 카테고리 리스트 -->
	<select id="categoriesList" resultType="com.db.model.ProductVO">
		SELECT num, pgender, bname, kind, pname, imgUrl, psize,
		balance, price, purchasedNum, explain, writedate, readcount,
		discountrate
		FROM (
		SELECT num, pgender, bname, kind, pname, imgUrl,
		psize, balance,
		price,
		purchasedNum, explain, writedate, readcount,
		discountrate,
		ROW_NUMBER() OVER
		(PARTITION BY pName ORDER BY num) RN
		FROM PRODUCT
		WHERE bname LIKE
		#{bname} AND kind = #{kind}
		)
		WHERE RN = 1
	</select>
```

위 sql문도 동일하게 만들지만 kind를 추가하여 카테고리 별로 분류하게 만든다.

### 4. Sale 상품 리스트 불러오기

#### header.jsp 수정

```jsp
<a href="/product/saleList" class="nav-item nav-link">Sale</a>
```

#### ProductController.java 에 추가

```java
	// 세일 상품 리스트
	@GetMapping("/saleList")
	public String hotDealListGET(HttpServletRequest request, RedirectAttributes rttr) {

		try {
			ArrayList<ProductVO> plist = productService.getAllProductNoDup(); // 모든 상품 중복없이 가져오기
			request.setAttribute("hdlist", plist);
		} catch (Exception e) {

			e.printStackTrace();
		}
		return "/product/saleList";

	}
```

#### ProductMapper.java 에 추가

```java
	// 모든 상품 정보 불러오기
	public ArrayList<ProductVO> getAllProduct();
```

#### ProductService.java 에 추가

```java
	// 모든 상품 정보 불러오기
	public ArrayList<ProductVO> getAllProduct() throws Exception;
```

#### ProductServiceImpl.java 에 추가

```java
	@Override
	public ArrayList<ProductVO> getAllProduct() throws Exception {

		return productmapper.getAllProduct();
	}
```

#### ProductMapper.xml 에 추가

```xml
	<!-- 모든 상품정보 불러오기 -->
	<select id="getAllProduct" resultType="com.db.model.ProductVO">

		select * from product

	</select>
```

#### saleList.jsp 추가

```jsp
<%@ page language="java" contentType="text/html; charset=utf-8" pageEncoding="utf-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt"%>
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
			<h1 class="font-weight-semi-bold text-uppercase mb-3">Sale</h1>
			<div class="d-inline-flex">
				<p class="m-0">
					<a href="/">Home</a>
				</p>
			</div>
		</div>
	</div>
	<!-- Page Header End -->
	<!-- Shop Start -->
	<div id="my-container" class="container-fluid pt-5">
		<!-- Shop Product Start -->
		<div class="col-lg-9 col-md-12">
			<div class="row pb-3">
				<c:forEach var="hdlist" items="${hdlist }">
					<c:if test="${hdlist.discountrate != 0 }">
						<div class="col-lg-4 col-md-6 col-sm-12 pb-1">
							<div class="card product-item border-0 mb-4">
								<div class="card-header product-img position-relative overflow-hidden bg-transparent border p-0">
									<a href="#">
										<img class="img-fluid w-100" style="height: 240px" src="../resources/img/${hdlist.imgUrl}" alt="">
									</a>
								</div>
								<div class="card-body border-left border-right text-center p-0 pt-4 pb-3">
									<h6 class="mb-3">${hdlist.pname}</h6>
									<div class="d-flex justify-content-center">
										<%-- 통화 단위 지정 --%>
										<h6>
											${hdlist.discountrate}% 할인
											<del style="text-decoration: line-through; color: gray;">
												<!-- 할인전 가격 -->
												<fmt:formatNumber value="${Integer.parseInt(hdlist.price)}" pattern="₩###,###" />
											</del>
											<br>
											<!-- 할인후 가격 -->
											<c:set var="discountedPrice" value="${Integer.parseInt(hdlist.price) * (100 - hdlist.discountrate) / 100}" />
											<fmt:formatNumber value="${Math.round(discountedPrice/100)*100}" pattern="₩###,###" />
										</h6>
									</div>
								</div>
							</div>
						</div>
					</c:if>
				</c:forEach>
			</div>
		</div>
		<!-- Shop Product End -->
	</div>
	<!-- Shop End -->
	<hr>
</body>
<jsp:include page="../footer.jsp"></jsp:include>
</html>
```

### 5. 상품 검색

#### ProductController.java 에 추가

```java
// 상품 검색
	@GetMapping("/searchProduct")
	public String searchProductGET(String pname, HttpServletRequest request) {
		request.setAttribute("pname", pname);
		try {
			ArrayList<ProductVO> bplist = productService.searchProduct(pname);
			if (bplist == null || bplist.isEmpty()) {
				return "/product/searchNotFound";
			}
			request.setAttribute("bplist", bplist);
		} catch (Exception e) {
			e.printStackTrace();
		}
		return null;
	}
```

검색한 상품이 없을 때는 searchNotFount.jsp로 이동하게 만든다.

#### ProductMapper.java 에 추가

```java
	// 상품 검색
	public ArrayList<ProductVO> searchProduct(String pname);
```

#### ProductService.java 에 추가

```java
	// 상품 검색
	public ArrayList<ProductVO> searchProduct(String pname) throws Exception;
```

#### ProductServiceImpl.java 에 추가

```java
	@Override
	public ArrayList<ProductVO> searchProduct(String pname) throws Exception {

		return productmapper.searchProduct(pname);
	}
```

#### ProductMapper.xml 에 추가

```xml
	<!-- 상품 검색 리스트 -->
	<select id="searchProduct" resultType="com.db.model.ProductVO">

		SELECT num, pGender,
		bName, kind, pName, imgUrl, pSize, balance, price,
		purchasedNum,
		explain, writedate, readcount, discountrate
		FROM (
		SELECT num, pGender,
		bName, kind,
		pName, imgUrl, pSize, balance, price,
		purchasedNum,
		explain, writedate, discountrate,
		readcount,
		ROW_NUMBER() OVER
		(PARTITION BY pName ORDER BY num) RN
		FROM
		PRODUCT
		WHERE pname LIKE '%'
		||#{pname}|| '%'
		)
		WHERE RN = 1

	</select>
```

위에 sql문과 마찬가지로 중복되지않은 상품들을 상품명(pname)으로 불러온다.

#### searchProduct.jsp

```html
<%@ page language="java" contentType="text/html; charset=utf-8" pageEncoding="utf-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt"%>
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
			<h1 class="font-weight-semi-bold text-uppercase mb-3">상품 "${pname }"의 검색 결과</h1>
			<div class="d-inline-flex">
				<p class="m-0">
					<a href="/">Home</a>
				</p>
			</div>
		</div>
	</div>
	<!-- Page Header End -->
	<!-- Shop Start -->
	<div id="my-container" class="container-fluid pt-5">
		<!-- Shop Product Start -->
		<div class="col-lg-9 col-md-12">
			<div class="row pb-3">
				<c:forEach var="bplist" items="${bplist }">
					<div class="col-lg-4 col-md-6 col-sm-12 pb-1">
						<div class="card product-item border-0 mb-4">
							<div class="card-header product-img position-relative overflow-hidden bg-transparent border p-0">
								<a href="#">
									<img class="img-fluid w-100" style="height: 280px" src="../resources/img/${bplist.imgUrl}" alt="">
								</a>
							</div>
							<div class="card-body border-left border-right text-center p-0 pt-4 pb-3">
								<h6 class="mb-3">${bplist.pname}</h6>
								<div class="d-flex justify-content-center">
									<h6>
										<c:if test="${bplist.discountrate==0}">
											<fmt:formatNumber value="${Integer.parseInt(bplist.price)}" pattern="₩###,###" />
										</c:if>
										<c:if test="${bplist.discountrate!=0}">
									   ${bplist.discountrate}% 할인
                                            <del style="text-decoration: line-through; color: gray;">
												<!-- 할인전 가격 -->
												<fmt:formatNumber value="${Integer.parseInt(bplist.price)}" pattern="₩###,###" />
											</del>
											<br>
											<!-- 할인후 가격 -->
											<c:set var="discountedPrice" value="${Integer.parseInt(bplist.price) * (100 - bplist.discountrate) / 100}" />
											<fmt:formatNumber value="${Math.round(discountedPrice/100)*100}" pattern="₩###,###" />
										</c:if>
									</h6>
								</div>
							</div>
						</div>
					</div>
				</c:forEach>
			</div>
		</div>
		<!-- Shop Product End -->
	</div>
	<!-- Shop End -->
	<hr>
</body>
<jsp:include page="../footer.jsp"></jsp:include>
</html>
```

검색한 상품이 Sale 상품일 경우 할인율을 불러오고 할인율이 적용된 가격으로 보이게 하여, 할인율이 적용되지않은 상품과 할인율이 적용된 상품을 각각 나누어 작성한다.

#### searchNotFound.jsp

```html
<%@ page language="java" contentType="text/html; charset=utf-8"  pageEncoding="utf-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt"%>
<!DOCTYPE html>
<html>
<head>
<jsp:include page="../header.jsp"></jsp:include>
</head>
<body>
	<hr>
	<!-- Page Header Start -->
	<div class="container bg-secondary mb-3" style="max-width: 800px; height: 350px;">
		<div class="d-flex flex-column align-items-center justify-content-center" style="min-height: 200px">
			<h1 class="font-weight-semi-bold text-uppercase mb-3">상품 "${pname }"의 검색 결과 없음</h1>
			<div class="d-inline-flex">
				<p class="m-0">
					<a href="/">Home</a>
				</p>
			</div>
		</div>
	</div>
	<hr>
 </body>
 <jsp:include page="../footer.jsp"></jsp:include>
 </html>
```

## [프로젝트 주소](https://github.com/GreenteaPIE/TeamProjectDBSpringVer)
