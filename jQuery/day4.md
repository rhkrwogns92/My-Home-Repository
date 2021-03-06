# 목차

- [목차](#목차)
- [1. 이벤트](#1-이벤트)
- [1.1  toggle(even, odd)](#11--toggleeven-odd)
- [1.2. one](#12-one)
- [2. effect](#2-effect)
- [3. Interactions](#3-interactions)
- [3.1. Tab Widget(tabs)](#31-tab-widgettabs)

# 1. 이벤트

# 1.1  toggle(even, odd)

• even : toggle 이 짝수번째 발생했을 경우
• odd : toggle 이 홀수번째 발생했을 경우
• 1.9.X 버전에서는 even, odd 기능이 사라짐.

# 1.2. one

- 일반 유저가 만든 이벤트함수
- 지정된 이벤트를 한번만 실행한다

```html
<head>
<meta charset="UTF-8">
<title>one</title>
<style>
#one>#result {
	border: 1px solid #44f;
	height: 120px;
	width: 300px;
	text-align: center;
	line-height: 120px;
}

#btnSend {
	width: 70px;
	height: 30px;
}

</style>
</head>
<body>
	<div id='one'>
		<div id='result'>송금 버튼을 클릭하시면 송급됩니다.</div>
		<input type='button' value='송금' id='btnSend'>
		<!-- disabled='disabled' 클릭 안되게 하는 속성 --> 	
	</div>
	
<script>
	let count=0; //전송횟수

	// 첫번째 방법(attr, disabled 속성 사용)
	$('#btnSend').click(function(){
		$('#result').html(count+'회 송금됨.');
		$(this).attr('disabled','disabled');
	})
	

    // 두번째 방법(one 사용)
	$('#btnSend').one('click', function(){
		count++;
		$('#result').html(count+'회 송금됨.');		
	});

    // 세번째 방법(단순 if문 사용)
    $('#btnSend').click(function(){
		if(count == 1){
			return false;
		}
		count++;
		$('#result').html(count+'회 송금됨.');
		
	});
</script>

</body>
```

# 2. effect

Animation Speed
  - slow : 600/1000 초
  - fast : 200/1000 초
  - default : 400/1000 초
  - 속도는 n/1000 초 단위로 설정할 수 있음.(효과가 시작되고 끝날때 까지 걸리는 시간을 의미)
  - callback : 효과가 끝나면 호출되는 함수를 의미

종류
  - show() | show( speed [, callback] ) : 나타내기
  - hide() | hide( speed [, callback] ) : 숨기기
  - toggle() | toggle( speed [ , callback] ) : (show,hide) 합친 toggle 
  - slideDown( [speed] [, callback] ) : 아래로 펼치기(slideUp 원상복귀)
  - slideUp( [speed] [ , callback] ) : 위로 접기
  - slideToggle( [speed] [.callback] ) : (slideUp,slideDown) 합친 toggle
  - fadeIn( [speed] [.callback] ) : 점점 사라진다
  - fadeOut( [speed] [.callback] ) : 점점 나타난다.
  - faseTo( [speed] , opacity [.callback] ) : 
  - animate(params [,speed] [, easing] [, callback] )

현재 상태값 체크 상태값 체크
  - 요소명.is(":visible") : 현재 상태가 보여져 있는 상태
  - 요소명.is("hidden") : 현재 상태가 감추어져 있는 상태


```html
<html>
<head>
<meta charset="UTF-8">
<title>effect</title>
<style>
	#parent{
		height: 210px;
	}
	#target {
		width: 750px;
		height: 600px;
		background-repeat: no-repeat;
		background-size: 65%;
		background-image: url('./images/hj1.jpg');	
	}
	
	#btns {
		height: 50px; 
	}

</style>
</head>
<body>
	<div id='effect'>
		<div id='parent'>
			<div id='target'></div>
		</div>
		<br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>
		<div id='btns'>
			<input type='button' value='SHOW' 		  id='btnShow'/>
			<input type='button' value='HIDE' 		  id='btnHide'/>
			<input type='button' value='TOGGLE' 	  id='btnToggle'/>
			<input type='button' value='SLIDE DOWN'	  id='btnSlideDown'/>
			<input type='button' value='SLIDE UP' 	  id='btnSlideUp'/>
			<input type='button' value='SLIDE TOGGLE' id='btnSlideToggle'/>
			<input type='button' value='FADE IN' 	  id='btnFadeIn'/>
			<input type='button' value='FADE OUT' 	  id='btnFadeOut'/>
			<input type='button' value='ANI' 		  id='btnAni'/>
		</div>	
	</div>
	
<script>
$('#btnShow').on('click',function(){
	$('#target').show(1500);
});
$('#btnHide').on('click',function(){
	$('#target').hide(1500);
});
$('#btnToggle').on('click',function(){
	$('#target').toggle(1000);
});
$('#btnSlideDown').on('click',function(){
	$('#target').slideDown(1000);
});
$('#btnSlideUp').on('click',function(){
	$('#target').slideUp(1000);
});
$('#btnSlideToggle').on('click',function(){
	$('#target').slideToggle(1000);
});
$('#btnFadeIn').on('click',function(){
	$('#target').fadeIn(1500);
})
$('#btnFadeOut').on('click',function(){
	$('#target').fadeOut(1500);
})
$('#btnAni').on('click',function(){
	$('#target').animate({		// animate함수의 효과는 map 구조로 전달해주는게 정석이다.
		'width' : 'toggle',
		'opacity' : 'toggle',
		'height' : 'toggle'
	});
})
</script>
</body>
</html>
```

# 3. Interactions

- Tab Widget : 탭 항목을 쉽게 만들어주는것(최신트렌드는 잘안쓴다.)
- Dragged : 요소를 끌고왔을때 이벤트 발생하게
- Droppable
- Resizable
- Selectable
- Sortable

- 사용하려면 라이브러리가 필요하다.
- jQuery CDN 라이브러리로 들어간다 -> jQuery UI로 들어간다 -> Themes 항목에서 쓸 테마주소 복사해 `<link>`에 붙인다. -> `<script>`로도 만든다

ex) 
```html
<link rel='stylesheet' type='text/css'
	  href='https://code.jquery.com/ui/1.12.1/themes/trontastic/jquery-ui.css'/>
<script src='https://code.jquery.com/ui/1.12.1/jquery-ui.js'></script>
```

# 3.1. Tab Widget(tabs)

- `tabs()` 함수를 사용한다.
- 기존 프레임틀이 정해져있다.

```html
<html>
<head>
<meta charset="UTF-8">
<title>tabs</title>
<link rel='stylesheet' type='text/css'
	  href='https://code.jquery.com/ui/1.12.1/themes/trontastic/jquery-ui.css'/>
<script src='https://code.jquery.com/ui/1.12.1/jquery-ui.js'></script>
</head>
<body>
	<div id='tabs'>		
	<!-- 상단 탭 메뉴 -->
		<ul>
			<li><a href='#web'>WEB</a></li>
			<li><a href='#app'>APP</a></li>
			<li><a href='#framework'>FrameWork</a></li>
			<li><a href='#office'>Office</a></li>
			<li><a href='#bigData'>Big Data</a></li>
		</ul>		
	
		<div id='web'>
			<ul>
				<li>웹이란....</li>
				<li>웹이란..</li>
			</ul>
		</div>		
		<div id='app'>
			<ul>
				<li>앱이란.....</li>
				<li>앱이란..</li>
			</ul>
		</div>		
		<div id='framework'>
			<ul>
				<li>프레임워크란....</li>
				<li>프레임워크</li>
			</ul>
		</div>		
		<div id='office'>
			<ul>
				<li>오피이스</li>
				<li>오피이스으으</li>
			</ul>
		</div>		
		<div id='bigData'>
			<ul>
				<li>데이터란.....</li>
				<li>데이터란..</li>
			</ul>
		</div>	
	</div>

<script>
	$('#tabs').tabs();

</script>
</body>
</html>
```

