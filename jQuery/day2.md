# 목차

- [목차](#목차)
- [1. traverse](#1-traverse)
- [2. traverse 종류](#2-traverse-종류)
- [3. traverse 사용 ex)](#3-traverse-사용-ex)
- [4. manipulation](#4-manipulation)
- [5. 이벤트 처리 방법 3가지](#5-이벤트-처리-방법-3가지)
- [5.1. input 버튼안에서 click event 처리](#51-input-버튼안에서-click-event-처리)
- [5.2. jQuery를 사용하여 값(id,class,tag) 가져와서 쓰기](#52-jquery를-사용하여-값idclasstag-가져와서-쓰기)
- [5.3. 순수하게 자바스크립트 코드](#53-순수하게-자바스크립트-코드)
- [5.4. 이벤트 처리 적용하기 (traverse.jsp)](#54-이벤트-처리-적용하기-traversejsp)
- [6. 홈페이지 만들기1 (index.jsp)](#6-홈페이지-만들기1-indexjsp)

# 1. traverse

traverse는 `.` 을 찍어서 표현한다. filter는 `:` 을 사용하여 표현.
- 명령어는 filter와 traverse가 매우 유사하거나 상충되기 때문에 개인적 취향에 따라 사용 빈도수가 달라질 수 있다.

# 2. traverse 종류

명령어
  - .eq(index)
  - .first()
  - .last()
  - .slice(start [, end])
    - start:  <=(이상) , end: < (미만)
  - ... 그외 많이있음

메소드(method)
  - .children(expr)
  - .find(expr)
  - .parent(expr)
  - .next(expr)
  - .prev(expr)
  - ... 그외 많이있음

# 3. traverse 사용 ex)

```html
<body>
	<div id='traverse'>
		<h1>TRAVERSE</h1>
		<fieldset>
		<legend>traverse</legend>
		<ul>
			<li>traverse는 '.' 을 찍어서 표현한다.</li>
			<li>명령어는 filter와 traverse가 매우 유사하거나 상충되기 때문에 개인적 취향에 따라 사용 빈도수가 달라질 수 있다.</li>
		</ul>		
		</fieldset>
		
		<ul>
			<li>백두산</li>
			<li>금강산</li>
			<li>설악산</li>
			<li>치악산</li>
			<li>내장산</li>
			<li>속리산</li>
			<li>맛동산</li>
			<li>한라산</li>	
		</ul>
		
		<!-- .parent() example code -->
        <div id='div1'>
            <input type='button' value='x' class='btn1'/>    
        </div>

        <div id='div2'>
            <input type='button' value='x' class='btn2'/>    
        </div>            		
        		
	</div>

<script>
	$('#traverse>ul>li').first().css('color', '#f00');
	$('#traverse>ul>li').filter(':odd').css('color','#00f');
	$('#traverse>ul>li').slice(1,4).css('font-family','궁서체');

	// div1, div2의 넓이와 높이를 지정하시오.(TODO)
	$('#traverse>div').css({
		'width'	 : '200px',
		'height' : '200px',
		'border' : '1px solid #444'
		})
	
	// btn1, btn2의 넓이와 높이를 지정하시오.(TODO)
	$('#traverse>#div1>.btn1, #traverse>#div2>.btn2').css({
		'width'  : '100px',
		'height' : '100px'
		})
	
</script>
</body>
```

# 4. manipulation

지정된 요소의 내부환경, 외부환경에 대한 요소를 추가, 제거
  - wrap: 덮어씌우기
  - append: 뒤쪽 추가
  - prepend: 앞쪽 추가
  - before
  - after
  - remove
  - empty: 상태정보 체크
  - ....

# 5. 이벤트 처리 방법 3가지 

- input 버튼안에서 click event 처리 
- jQuery를 사용하여 값(id,class,tag) 가져와서 쓰기
- 순수하게 자바스크립트 코드

# 5.1. input 버튼안에서 click event 처리 

```html
<div id='btn_zone'>
	<input type='button' value='WRAP' id='btnWrap' onclick='abc(this)'/>
</div>

<script>
<!-- 1. WRAP 버튼에 click event 처리 -->
function abc(button){
	//console.log(button.value)
	var wrap = "<label><input type='checkbox'>확인</label>";
	$('#mani>#target').wrap(wrap);// wrap 덮어씌워서 생긴다.
}
</script>
```

# 5.2. jQuery를 사용하여 값(id,class,tag) 가져와서 쓰기

```html
<div id='btn_zone'>
    <input type='button' value='APPEND' id='btnAppend'/>
</div>

<script>
<!-- 2. jQuery를 사용하여 id값가져와서 쓴것 -->
var btnAppend = $('#btn_zone>#btnAppend');
btnAppend.on('click', function() {
	//console.log($(this).val())
    var wrap = "<label><input type='checkbox'>확인</label>";
	$('#mani>#target').append(wrap);// append 뒤쪽에 생긴다.
});
</script>
```

# 5.3. 순수하게 자바스크립트 코드

```html
<div id='btn_zone'>
	<input type='button' value='PREPEND' id='btnPrepend'/>
</div>

<script>
<!-- 3. 순수하게 자바스크립트 코드로 -->
var btnPrepend = document.getElementById('btnPrepend');
btnPrepend.onclick = function() {
	//console.log(this.value)
    var wrap = "<label><input type='checkbox'>확인</label>";
	$('#mani>#target').prepend(wrap);// prepend 앞쪽에 생긴다.
} 
</script>
```

# 5.4. 이벤트 처리 적용하기 (traverse.jsp)

- this: 이벤트 범위에서 발생된 자기 자신을 나타낸다.
  
btn1, btn2에 이벤트 처리

```html

<body>
<div id='traverse'>
		
	<!-- .parent() example code -->
    <div id='div1'>
        <input type='button' value='x' class='btn1'/>    
    </div>
    <div id='div2'>
        <input type='button' value='x' class='btn2'/>    
    </div>              		
        		
/div>

<script>

	<!-- div1, div2의 넓이와 높이를 지정하시오.(TODO) -->
	$('#traverse>div').css({
		'width'	 : '200px',
		'height' : '200px',
		'border' : '1px solid #444'
		})
	
	<!-- btn1, btn2의 넓이와 높이를 지정하시오.(TODO) -->
	$('#traverse>#div1>.btn1, #traverse>#div2>.btn2').css({
		'width'  : '100px',
		'height' : '100px'
		})

	<!-- .parent() example code -->    
	<!-- btn1 이벤트 처리 -->
	var btn1 = $('.btn1')[0];
	btn1.onclick = function() {
	    var p = $(btn1).parent();
		$(p).css('background-color','#88f');
	    // $(btn1).parent().css('background-color','#88f');
	}
	<!-- btn2 이벤트 처리 -->
	var btn2 = $('.btn2')[0];
	btn2.onclick = function() {
	    $(this).parent().css('background-color','#f99');
      	//this: 이벤트 범위에서 발생된 자기 자신을 나타낸다.
	}

</script>

</body>
```

# 6. 홈페이지 만들기1 (index.jsp)

```html
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>manipulation</title>
</head>
<body>
<div id='mani'>
	<div id='target'>TARGET</div>
	
	<div id='btn_zone'>
		<input type='button' value='WRAP' id='btnWrap' onclick='abc(this)'/>
		<input type='button' value='APPEND' id='btnAppend'/>
		<input type='button' value='PREPEND' id='btnPrepend'/>
	</div>
</div>

<script>
	// 버튼 3개에 각각 click event 처리하기
	
	// 1. WRAP 버튼에 click event 처리
	function abc(button){
		//console.log(button.value)
		
		var wrap = "<label><input type='checkbox'>확인</label>";
		$('#mani>#target').wrap(wrap);// wrap 덮어씌워서 생긴다.
	}
	// 2. jQuery를 사용하여 id값가져와서 쓴것
	var btnAppend = $('#mani>#btn_zone>#btnAppend')[0];
	btnAppend.onclick = function() {
		//console.log($(this).val())
		
		var wrap = "<label><input type='checkbox'>확인</label>";
		$('#mani>#target').append(wrap);// append 뒤쪽에 생긴다.
	}
	// 3. 순수하게 자바스크립트 코드로
	var btnPrepend = document.getElementById('btnPrepend');
	btnPrepend.onclick = function() {
		//console.log(this.value)
		
		var wrap = "<label><input type='checkbox'>확인</label>";
		$('#mani>#target').prepend(wrap);// prepend 앞쪽에 생긴다.
	}
</script>

</body>
</html>
```