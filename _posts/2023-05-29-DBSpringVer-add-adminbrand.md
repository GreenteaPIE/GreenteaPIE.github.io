---
layout: post
title: 19 - DB Spring 어드민 브랜드 관리
date: 2023-05-29
excerpt: "팀 프로젝트 DB Spring Version"
tags: [project, java, jsp, Oracle, css, HTML, BootStrap, API, JQuery, JavaScript, Spring, FrameWork]
feature: /img/SpringDB/logo.png
comments: true



---


> **사용한 플랫폼 : Spring, Oracle**



### 1.  브랜드 추가 기능

#### AdminController.java 에 추가

브랜드, 상품 관리로 이동하는 버튼 페이지를 만들어 준다.

```java
	// 브랜드, 상품관리 페이지
	@GetMapping("productManagementPage")
	public void productManageGET() {
		System.out.println("productManagementPage 접속");
	}
```

#### productManagementPage.jsp 에 추가

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<jsp:include page="../header.jsp"></jsp:include>
</head>
<body>
	<hr>
	<div class="mypage-container">
		<div class="mypage-header"></div>
		<div class="mypage-wrapper">
			<div class="mypage-header">
				<div class="mypage-userinfo">
					<h4>
						<strong>${user.userid}</strong> 님
					</h4>
				</div>
				<div class="mypage-btns">
					<div class="mypage-menu">
						<ul>
							<button onclick="location.href='adminBrandList'">브랜드 관리</button>
							<button onclick="location.href='adminProductList'">상품 관리</button>
						</ul>
					</div> 
				</div>
			</div>
		</div>
	</div>
	<hr>
</body>
<jsp:include page="../footer.jsp"></jsp:include>
</html>
```

![_config.yml]({{ site.baseurl }}/img/SpringDB/brandgo.png)

브랜드 관리 버튼을 눌르면 이동되는 브랜드 관리 페이지를 만들어 준다.

#### AdminController.java 에 추가

```java
	@Autowired
	ProductService productService;

    // 브랜드 리스트
	@GetMapping("adminBrandList")
	public void adminBrandListGET(Model model) throws Exception {
		System.out.println("adminBrandList 접속");
		List<BrandVO> bList = productService.brandList();
		model.addAttribute("blist", bList);
	}
```

#### adminBrandList.jsp 추가

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<script type="text/javascript" src="../resources/js/admin.js"></script>
<style type="text/css">
img {
	width: 400px;
}
</style>
<jsp:include page="../header.jsp"></jsp:include>
</head>
<body>
	<hr>
	<div id="wrap" align="center">
		<hr>
		<h1 style="margin-top: 80px;" class="font-weight-semi-bold text-uppercase mb-3">브랜드 리스트 관리</h1>
		<input type="button" style="margin: 40px 0" class="btn btn-primary px-3" value="브랜드 등록" onclick="location.href='adminbrandWriteForm'"> <input type="button" style="margin: 40px 0" class="btn btn-primary px-3" value="브랜드 삭제" onclick="location.href='adminBrandDelete'">
		<!-- Categories Start -->
		<div class="container-fluid pt-5">
			<div class="row px-xl-5 pb-3">
				<c:forEach var="blist" items="${blist }">
					<div class="col-lg-4 col-md-6 pb-1">
						<div class="cat-item text-center" style="padding: 30px;">
							<p class="text-right"></p>
							<a href='/product/brandProductList?bname=<c:out value="${blist.bname }"/>' class="cat-img position-relative overflow-hidden mb-3">
								<img class="img-fluid" src="../resources/img/${blist.imgurl}" alt="">
							</a>
							<h5 class="font-weight-semi-bold m-0"></h5>
						</div>
					</div>
				</c:forEach>
			</div>
		</div>
	</div>
	<hr>
</body>
<jsp:include page="../footer.jsp"></jsp:include>
</html>
```

![_config.yml]({{ site.baseurl }}/img/SpringDB/adminbrandlist.png)

#### AdminController.java 에 추가

```java
	// 브랜드 등록페이지
	@GetMapping("adminbrandWriteForm")
	public void adminBrandWriteFormGET() {
		System.out.println("adminbrandWriteForm 접속");
	}
```

#### adminbrandWriteForm.jsp 추가

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<script src="https://code.jquery.com/jquery-3.4.1.js" integrity="sha256-WpOohJOqMqqyKL9FccASB9O0KwACQJpFTUBLTYOVvVU=" crossorigin="anonymous"></script>
<style>
.image-regit {
	position: absolute;
	opacity: 0;
	top: 50%;
	left: 50%;
	color: white;
	font-size: 4em;
	font-weight: 300;
	transform: translate(-50%, -50%);
	text-align: center;
	background-color: transparent;
	transition: color 0.3s ease;
	border: none;
	outline: none;
	cursor: pointer;
	width: 100%;
	height: 100%;
}

.blur {
	display: flex;
	justify-content: center;
	align-items: center;
	transition: opacity 0.3s ease;
	position: absolute;
	width: 100%;
	height: 100%;
	top: 0;
	left: 0;
	background-color: gray;
	opacity: 0;
	text-align: center;
	color: white;
	font-size: 3em;
}

.carousel-inner:hover .blur {
	opacity: 0.5;
}
</style>
<jsp:include page="../header.jsp"></jsp:include>
</head>
<body>
	<hr>
	<div class="container bg-secondary mb-3" style="max-width: 800px;">
		<div class="d-flex flex-column align-items-center justify-content-center" style="min-height: 200px">
			<h1 class="font-weight-semi-bold text-uppercase mb-3">브랜드 등록</h1>
			<div class="d-inline-flex">
				<p class="m-0">
					<a href="adminBrandList">Brand List</a>
				</p>
			</div>
		</div>
	</div>
	<!-- Shop Detail Start -->
	<div class="container-fluid py-5">
		<form action="brandWrite.do" method="post" name="frm" enctype="multipart/form-data">
			<div class="row px-xl-5">
				<div class="col-lg-5 pb-5">
					<div id="product-carousel" class="carousel slide" data-ride="carousel">
						<div class="carousel-inner border" style="height: 500px;">
							<div class="blur">이미지 등록</div>
							<input type="file" class="btn btn-primary px-3 image-regit" name="uploadFile" id="file-upload-1" onchange="previewFile(), selectSameFile(event)">
							<img style="width: 100%;" class="img-fluid preview-img" src="" alt="">
						</div>
					</div>
				</div>
				<div class="col-lg-7 pb-5">
					<div class="d-flex mb-3" style="margin-top: 150px;">
						<p class="text-dark font-weight-medium mb-0 mr-3">Brand Name:</p>
					</div>
					<div style="display: flex;">
						<input type="text" class="form-control" name="bname" maxlength="50" style="width: 200px;">
					</div>
					<div class="d-flex mb-3" style="margin-top: 40px;">
						<p class="text-dark font-weight-medium mb-0 mr-3">Brand Image</p>
					</div>
					<div class="d-flex align-items-center mb-4 pt-2">
						<div class="custom-control custom-control-inline">
							<input type="file" class="btn btn-primary px-3" name="uploadFile" id="file-upload-2" onchange="previewFile(event), selectSameFile(event)">
						</div>
					</div>
				</div>
				<input type="submit" class="btn btn-primary px-3" style="margin: 0 auto;" value="브랜드 등록하기">
			</div>
		</form>
	</div>
	<!-- Shop Detail End -->
	<hr>
	<script>
		//미리보기 사진
		function previewFile() {
			const preview = document.querySelector('.preview-img');
			const file = event.target.files[0];
			const reader = new FileReader();

			reader.addEventListener("load", function() {
				// 파일을 읽어서 이미지 URL로 설정합니다.
				preview.src = reader.result;
			}, false);

			if (file) {
				// 파일을 읽습니다.
				reader.readAsDataURL(file);
			}
		}
		//두개의 input이 같은 파일을 선택하도록 지정
		function selectSameFile(event) {
			const inputElements = document.querySelectorAll('input[type=file]');

			for (let i = 0; i < inputElements.length; i++) {
				inputElements[i].files = event.target.files;
			}
		}

		let regex = new RegExp("(.*?)\.(jpg|png)$");
		let maxSize = 1048576 * 5; //5MB	

		function fileCheck(fileName, fileSize) {

			if (fileSize >= maxSize) {
				alert("파일 사이즈 초과");
				return false;
			}

			if (!regex.test(fileName)) {
				alert("해당 종류의 파일은 업로드할 수 없습니다.");
				return false;
			}

			return true;
		}
	</script>
</body>
<jsp:include page="../footer.jsp"></jsp:include>
</html>
```

![_config.yml]({{ site.baseurl }}/img/SpringDB/brandupload.png)

#### AdminMapper.java 에 추가

```java
	public void brandEnroll(BrandVO bVo); // 브랜드 추가
```

#### AdminMapper.xml 에 추가

```xml
	<!-- 브랜드 등록 -->
	<insert id="brandEnroll">
		insert into brand values(#{bname}, #{imgurl})
	</insert>
```

#### AdminSerivce.java 에 추가

```java
	public void brandEnroll(BrandVO bVo) throws Exception; // 브랜드 추가
```

#### AdminServiceImpl.java 에 추가

```java
	@Override
	public void brandEnroll(BrandVO bVo) throws Exception {
		mapper.brandEnroll(bVo);		
	}
```

#### AdminController.java 에 추가

```java
	@PostMapping("brandWrite.do")
	public String brandWritePOST(String bname, MultipartFile uploadFile) throws Exception {
		System.out.println("brandWrite.do 접속");
		String uploadFolder = "C:\\Users\\User\\OneDrive\\바탕 화면\\Develop\\DiamondBlakc-SpringVer\\src\\main\\webapp\\resources\\img";

		/* 폴더 생성 */
		File uploadPath = new File(uploadFolder);

		System.out.println("uploadPath" + uploadPath);
		if (uploadPath.exists() == false) {
			uploadPath.mkdirs();
		}

		/* 파일 이름 */
		String uploadFileName = uploadFile.getOriginalFilename();

		System.out.println("uploadfilename: " + uploadFileName);

		/* 파일 위치, 파일 이름을 합친 File 객체 */
		File saveFile = new File(uploadPath, uploadFileName);

		/* 파일 저장 */
		try {
			uploadFile.transferTo(saveFile);
		} catch (Exception e) {
			e.printStackTrace();
		}
		BrandVO brand = new BrandVO();
		brand.setBname(bname);
		brand.setImgurl(uploadFileName);
		adminService.brandEnroll(brand);
		return "redirect:/admin/adminBrandList";
	}

```

### 2. 브랜드 삭제 기능

#### AdminController.java 에 추가

```java
	@GetMapping("adminBrandDelete")
	public void adminBrandDelete(Model model) throws Exception {
		System.out.println("adminBrandDelete 접속");
		List<BrandVO> bList = productService.brandList();
		model.addAttribute("blist", bList);
	}
```

#### adminBrandDelete.jsp 추가

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<style type="text/css">
img {
	width: 400px;
}

.delete-btn {
	position: absolute;
	opacity: 0;
	top: 50%;
	left: 50%;
	color: white;
	font-size: 3em;
	font-weight: 300;
	transform: translate(-50%, -50%);
	text-align: center;
	background-color: transparent;
	transition: color 0.3s ease;
	border: none;
	outline: none;
}

.blur {
	transition: opacity 0.3s ease;
	position: absolute;
	width: 100%;
	height: 100%;
	top: 0;
	left: 0;
	background-color: gray;
	opacity: 0;
}

.cat-item:hover .delete-btn {
	opacity: 1;
}

.cat-item:hover .delete-btn:hover {
	color: red;
	opacity: 1;
}

.cat-item:hover .blur {
	opacity: 0.7;
}
</style>
</head>
<jsp:include page="../header.jsp"></jsp:include>
<body>
	<hr>
	<div class="container bg-secondary mb-3" style="max-width: 800px;">
		<div class="d-flex flex-column align-items-center justify-content-center" style="min-height: 200px">
			<h1 class="font-weight-semi-bold text-uppercase mb-3">브랜드 삭제</h1>
			<div class="d-inline-flex">
				<p class="m-0">
					<a href="adminBrandList">Brand List</a>
				</p>
			</div>
		</div>
	</div>
	<!-- Categories Start -->
	<div class="container-fluid pt-5">
		<div class="row px-xl-5 pb-3">
			<c:forEach var="blist" items="${blist }">
				<div class="col-lg-4 col-md-6 pb-1">
					<div class="cat-item text-center" style="padding: 30px;">
						<a href='' class="cat-img position-relative overflow-hidden mb-3">
							<img class="img-fluid" src="../resources/img/${blist.imgurl}" alt="">
						</a>
						<div class="blur"></div>
						<button class="delete-btn font-weight-semi-bold m-0" onclick="deleteBrand('${blist.bname}')">브랜드 삭제</button>
						<form action="deleteBrand.do" method="post" id="${blist.bname}">
							<input type="hidden" name="bname" value="${blist.bname}">
						</form>
						<h5 class="font-weight-semi-bold m-0"></h5>
					</div>
				</div>
			</c:forEach>
		</div>
	</div>
	<hr>
	<script>
		function deleteBrand(bname) {
			if (confirm("삭제하면 브랜드 내의 모든 상품이 사라집니다. \n브랜드<"+bname+">을(를) 삭제하시겠습니까?")) {
				$("#" + bname).submit();
			}
		}
	</script>
</body>
<jsp:include page="../footer.jsp"></jsp:include>
</html>
```

![_config.yml]({{ site.baseurl }}/img/SpringDB/branddelete.png)

#### AdminMapper.java 에 추가

```java
	public void deleteBrand(String bname); // 브랜드 삭제
```

#### AdminMapper.xml 에 추가

```xml
	<!-- 브랜드 삭제 -->
	<delete id="deleteBrand">
		delete from brand where bname = #{bname}
	</delete>
```

#### AdminSerivce 에 추가

```java
	public void deleteBrand(String bname) throws Exception; // 브랜드 삭제
```

#### AdminServiceImpl.java 에 추가

```java
	@Override
	public void deleteBrand(String bname) throws Exception {
		mapper.deleteBrand(bname);
	}
```

#### AdminController.java 에 추가

```java
	@PostMapping("deleteBrand.do")
	public String deleteBrandPOST(String bname) throws Exception {
		System.out.println("deleteBrand.do 실행");
		System.out.println("삭제할 브랜드:" + bname);
		adminService.deleteBrand(bname);
		return "redirect:/admin/adminBrandList";
	}
```



## [프로젝트 주소](https://github.com/GreenteaPIE/TeamProjectDBSpringVer)
