

# 1. 슬라이드

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<title>slick test</title>

<link rel="stylesheet" type="text/css" href="http://kenwheeler.github.io/slick/slick/slick.css" />
<link rel="stylesheet" type="text/css" href="http://kenwheeler.github.io/slick/slick/slick-theme.css" />
<script src="http://code.jquery.com/jquery-3.2.1.slim.min.js" integrity="sha256-k2WSCIexGzOj3Euiig+TlR8gA0EmPjuc79OEeY5L45g=" crossorigin="anonymous"></script>
<script type="text/javascript" src="http://kenwheeler.github.io/slick/slick/slick.min.js"></script>
<style>
body{
position: relative;
width: 700px;
background-color: pink;
}

.slick-items{
	postion: absolute;
	left:500px;
	top:500px;
	background-color: gray;
	color:white;
}
</style>

</head>
<body>
	<div class="slick-items">
       	<div>안녕안녕안녕안녕</div>
		<div>하세요</div>
		<div>반갑반갑습니다다반갑습니다반갑습니다반갑습니다반갑습니다습니다</div>
		<div><img src="./img/dog.png"/></div>
		<div>반가워요</div>
		<div>치킨</div>
		<div>먹고</div>
		<div>싶다</div>
		<div>배고프다</div>
		<div>야식</div>
		<div>치킨</div>						
   	</div>
</body>
 	
<script type="text/javascript">
$(document).ready(function () {
	//alert('123');
	
	$('.slick-items').slick({
		  centerMode: true,
		  centerPadding: '60px',
		  slidesToShow: 3,
		  responsive: [
		    {
		      breakpoint: 768,
		      settings: {
		        arrows: false,
		        centerMode: true,
		        centerPadding: '40px',
		        slidesToShow: 3
		      }
		    },
		    {
		      breakpoint: 480,
		      settings: {
		        arrows: false,
		        centerMode: true,
		        centerPadding: '40px',
		        slidesToShow: 1
		      }
		    }
		  ]
		});			

});
</script>
</html>
</html>
```