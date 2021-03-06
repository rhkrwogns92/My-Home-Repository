# 목차

- [목차](#목차)
- [1. function의 두가지 기능](#1-function의-두가지-기능)
- [2. Core(jQuery의 여려가지 표현식)](#2-corejquery의-여려가지-표현식)
- [3. Attribute 함수](#3-attribute-함수)
- [3.1. .attr(), .removeAttr()](#31-attr-removeattr)
- [4. 이벤트(Event)](#4-이벤트event)
- [4.1. 종류](#41-종류)
- [4.2. 직렬화 관련 메서드 관련 메서드](#42-직렬화-관련-메서드-관련-메서드)
- [4.3. keydown(fn) | keypress(fn) | keyup(fn)](#43-keydownfn--keypressfn--keyupfn)
- [4.4. trigger(fn)](#44-triggerfn)

# 1. function의 두가지 기능

a(); : 함수가 선언되기전에 호출해도 됨.
b(); : 함수가 선언되기 전이기 때문에 호출할 수 없음.

- function a(){}: 스크립트가 실행되면 메모리가 상주하게됨.  ( 함수가 hoising 호이징 된다고 한다.)
- var b = function(){}: 함수를 호출하는 시점에 메모리에 올라감.

# 2. Core(jQuery의 여려가지 표현식)

- $( expr, context)
  1. Parameter
     - expr : 찾는 표현식
     - context : Element | jQuery 
  2. Example
     - $('div') : <script/>div> 선택
     - $('div > p') : <script/>div>안에 <script/>p> 선택
     - $('input:radio', document.forms[0]) : 첫번째 폼의 라디오 버튼 선택

- $(html) | $(elements)
Html 를 추가하거나 선택할 수 있다.

```js
Html 을 특정 위치(뒤쪾에)에 추가하는예
[jQuery]
$('<h1>hi jobtc</h1>').appendTo('#core_append_div');

[html]
<div id='core_append_div'>aa</div>
```

-  $(fn)
   1. java script: window.onload(function(){})
   2. jQuery: $(document).ready(function(){})을 짧게 표현.
     - $( function(){ ... } )

- length | size()
지정된 요소의 개수를 구한다.

```js
[jQuery]
var len = $('div').length;
var s = $('div').size();
alert(len + ',' + s); // 4,4

[html]
<div></div><div></div><div></div><div></div>
```

- get() | get(n)
해당되는 모든 요소들을 배열로 가져오거나 n 번째 요소를 가져옴.

```js
[jQuery]
var len = $('div').get();
$('#get_result').html(len.join('<br/>'));

[html]
<div></div><div></div><div></
```

- each(fn)
지정된 항목을 순환 검색하여 fn 를 수행한다.
  - fn : function
java: for(v: collection){...}
java script: for(v in array){...}

```js
[jQuery]
var bg = [ '#f00', '#00f', '#0f0','#ff0']
$(document).ready(function(){
$('.each_div1').each(function(i){
$(this).css('background-color',bg[i]);
})
})

[html]
<div class='each_div1'>11</div>
<div class='each_div1'>22</div>
<div class='each_div1'>33</div>
<div class='each_div1'>44</div>
```

- index(subject) | find()
    1. index() : 지정된 요소를 검색하여 위치값을 반환함. 0 base 이며 찾지 못하면 -1 을 반환.
    2. find() : 저정된 요소를 검색하여 배열 타입으로 반환
     - subject : 찾고자 하는 요소. element

```js
[jQuery]
var bg = [ '#fdd', '#ddf', '#dfd','#ffd']
$(document).ready(function(){
var index = $('div').index($('.index_div2'));
$('.index_div' + (index+1)).css('background',bg[index]);
})

[html]
<div class='index_div1'>11</div>
<div class='index_div2'>22</div>
<div class='index_div3'>33</div>
<div class='index_div4'>44</div>

```

- eq(pos) | lt(pos) | gt(pos)
    1. 같거나 작거나 큰 요소들을 찾는다.
     - pos : Number

```js
[jQuery]
var bg = [ '#fdd', '#ddf', '#dfd','#ffd']
$(document).ready(function(){
var eq = $('.sub_div1:eq(2)');
var lt = $('.sub_div1:lt(2)');
var gt = $('.sub_div1:gt(2)');
lt.css('background-color', bg[0]);
eq.css('background-color', bg[1]);
gt.css('background-color', bg[2]);
})

[html]
<div class='sub_div1'>11</div>
<div class='sub_div1'>22</div>
<div class='sub_div1'>33</div>
<div class='sub_div1'>44</div>

```

# 3. Attribute 함수

HTML 요소에서 name 속성을 제외한 나머지 부분을 제어할 수 있다. 

- .attr() : 추가
- .removeAttr() : 제거
- .val() : value 값을 가져옴.
-  .text() | .html() : 해당 형식으로 값을 가져오거나 대입함.

# 3.1. .attr(), .removeAttr()

```html
<html>
<head>
<meta charset="UTF-8">
<title>attribute</title>
<style>
	#target{
		border: 1px solid #aaa;
		width: 400px;
		height: 200px;
	}

	.a_type {
		background-color: #00f;
		color: #fff;	
	}
	.a_type>h1{
		color: #fff;
		font-family: 궁서체;
	}

	.c_type{
		background-color: none;
		color: none;
		font-family: none;	
	}		
</style>
</head>
<body>
	<div id='attr'>
		<div id='target'>
			<h1>jQuery Attr</h1>
		</div>	
		<div id='btns'>
			<input type='button' value='A Type' id='btnAtype'/>
			<input type='button' value='C Type' id='btnCtype'/>
		</div>	
	</div>
<script>
// A, B type에 click event 추가
// .attr() 함수 사용
var target = $('#attr>#target');
$('#attr #btnAtype').on('click', function(){
	$(target).attr('class','a_type');
	
});

// .removeAttr() 함수 사용
$('#attr>#btns>#btnCtype').on('click', function(){
    $(target).removeAttr('class');
});
</script>	
</body>
</html>
```

# 4. 이벤트(Event)

# 4.1. 종류

- load(fn) | unload(fn)
- blur(fn) | blur() | focus(fn) | focus()
- hover(over, out)
  - 시작 함수, 끝나는 함수 두개 다 작성 해줘야한다.(css의 식이 더 편하다.)
- change(fn) | scroll(fn)
- click() | click(fn) | dbclick(fn)
- keydown(fn) | keypress(fn) | keyup(fn)
  - css의 식이 더 편하다.
- mousedown(fn) | mouseenter(fn) | mouseleave(fn) | mouseout(fn) | mouseup(fn) |
mousemove(fn) | mouseover(fn)
- ready(fn)
- resize(fn)
- select(fn) | select()
- submit(fn) | submit()
- bind(type, data, fn) | unbind(type, fn)
- one(type, data, fn)
- error(fn)
- trigger(type)
- toggle(even, odd)


- click()
 $().on('click', function(){..})
    - on: 이벤트를 다루기위한 핸들러 수식
    - click: 클릭이벤트 함수
    - function: 안에다가 이벤트가 발생 했을때 실행 할 코드를 적는다

# 4.2. 직렬화 관련 메서드 관련 메서드
key값과 value값의 직렬화

key값과 value값의 직렬화

- serialize() : 폼의 값을 get type 으로 뱐환한다.
- serializeArray() : 폼의 값을 jQuery 배열 객체로 반환 한다.

submit 함수
- 자바 스크립트: $().onsubmit = function(){ .. }
- jQuery: $().submit(function(){ .. })

ex) serialize
```html
<html>
<head>
<meta charset="UTF-8">
<title>serialize</title>
</head>
<body>
	<div id='serialize'>
		<form name='frm_serialize' method='post' id='frm_serialize'>
			<label>아이디</label>
			<input type='text' name='mId' value='a001'/><br/>
			<label>이름</label>
			<input type='text' name='mName' value='홍길동'/><br/>
			<label>연락처</label>
			<input type='text' name='phone' value='010-1234-1234'/><br/>
			<input type='submit' value="전송">		
		</form>
	</div>

<script>	
	//$('#frm_serialize')[0].onsubmit = function(){ }) // js 타입
	$('#frm_serialize').submit(function(){
		let data = $(this).serialize();
		console.log(data);	
		return false; // submit 기능 중지.
	}); // jQuery 타입	
</script>	
</body>
</html>
```

# 4.3. keydown(fn) | keypress(fn) | keyup(fn)

- event.keyCode 값에 의해 이벤트가 발생한 키 코드값을 가져 올 수 있음.
- event.type 에 의해 키보드의 상수값을 얻을 수 있다.

```html
<html>
<head>
<meta charset="UTF-8">
<title>keyup</title>
<style>
	#result {
		border: 1px solid #aaa;
		border-radius: 10px;
		color: black;
		padding: 15px;
		width: 350px;
		height: 200px;
		backhround-color: #eee;
		box-shadow: 2px 2px 4px #999;
		overflow: auto;
	}
</style>
</head>
<body>
	<div id='keyup'>
		<label>생년월일</label>
		<input type='text' id='birth' placeholder='YYYYMMDD'/>
		<label>-</label>
		<input type='text' id='gender' size='4'/>
		<p/>
		<div id='result'></div>
	</div>
	
<script>
$('#keyup>#birth').keyup(function(ev){
	var str = ev.keyCode;
	$('#result').text(str);
	
	// 입력된 문자열의 길이를 계산하여 8자미면 커서를 성별을 읿력하는 항목으로 이동.
	if($(this).val().length >= 8){
		$('#gender').focus().select(); 
		//focus(): 커서가 넘어간다.(커서이동)  
		//select(): 기존 입력되어있던 값이 선택된다.(사용자가 기존내용 클릭해서 안지워도 된다)
	}
});

// 성별이 입력되면 홀수이면 '남자', 짝수이면 '여자'를 결과창에 출력
$('#keyup>#gender').keyup(function(){
	var gen = Number($(this).val());
	var str = '';
	if(gen%2 == 0){
		str = '여성입니다.';
	}else{
		str = '남성입니다.';
	}
	$('#result').html(str);	
});
</script>
</body>
</html>
```

# 4.4. trigger(fn)

- 이벤트를 자동으로 발생 시킨다.
  - 절차
     1. 이벤트를 처리할 함수를 지정
     2. 이벤트 추가
	 3. trigger() 수행

```html
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>trigger</title>
<style>
#key_text { width:200px;height:20px;font-size:20px;}
.key_lbl{width:100px;text-align:right;display:inline-block;}
</style>
<script src='../lib/jquery-1.8.3.js'></script>
<script>
function chk(ev){
$('#af_div').text("이벤트 발생 발생 !!!");
}
$(document).ready(function(){
$('#trigger_btn').click(chk);
$('#trigger_btn').trigger('click');
})
</script>
</head>
<body>
<h3>trigger </h3>
<button type='button' id='trigger_btn'>누르지 않아도 발생 않아도 발생 발생 </button>
<fieldset>
<legend>Result</legend>
<div id='af_div'></div>
</fieldset>
</body>
</html>

```
