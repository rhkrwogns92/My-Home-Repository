# 목차
- [목차](#목차)
- [1. jQuery](#1-jquery)
- [2. jQuery  설치 과정](#2-jquery--설치-과정)
- [3. eclipse EE 버전 설치 후 환경 설정](#3-eclipse-ee-버전-설치-후-환경-설정)
- [4. 수업 시작전 test & 준비](#4-수업-시작전-test--준비)
- [5. 용어](#5-용어)
- [6. CDN 방식으로 jQuery 라이브러리 가져오기](#6-cdn-방식으로-jquery-라이브러리-가져오기)
- [7. jQuery 라이브러리 가져오기](#7-jquery-라이브러리-가져오기)
- [8. css와 jQuery 선택자 구분](#8-css와-jquery-선택자-구분)
- [9. DOM](#9-dom)
- [10. 다시 실습1](#10-다시-실습1)
- [11. JSON 표기 방법](#11-json-표기-방법)
- [11.1. KEY:VALUE => {KEY:VALUE, KEY:VALUE, ...}](#111-keyvalue--keyvalue-keyvalue-)
- [12. 다시 실습2 (aside 생성)](#12-다시-실습2-aside-생성)
- [13. filter](#13-filter)
- [13.1. filter 종류](#131-filter-종류)
- [13.2 filter 기능 익히기(선택된 요소 중 필요한 요소만을 걸려내는 방법)](#132-filter-기능-익히기선택된-요소-중-필요한-요소만을-걸려내는-방법)
- [14. 파일 링크 합치는 방법](#14-파일-링크-합치는-방법)
- [14.1. 동적 include (index에 filter파일 합치기)](#141-동적-include-index에-filter파일-합치기)

# 1. jQuery

1. html + CSS + javascript를 통합한 하나의 라이브러리
2. 굉장히 쉬운 접근방법을제공한다. --> 전체 처리 속도 감소.
3. 모든 javascript 문법은 그대로 사용 가능.
4. 대소문자 구별,
5. '' or ""는 구별하지 않음.
6. 라이브러리를 사용하는 방법
  - CDN(인터넷 안되면 사용 불가하다.)
  - 라이브러리를 해당 프로젝트내에 복사해서 사용.(인터넷 안되도 폴더로 복사 해놓기 때문에 언제든 사용 가능하다.)
    - 1. 분석해서 하고 싶은 개발자는 uncompressed 다운
    - 2. 속도 빠르고 싶은사람은?? compressed
7. jQuery는 모든것이 함수화되어 있기 때문에 함수에 대한 이해가 좋을수록 jQuery의 이해도가 높아질 수 있다.
  - ex) id = 'abc' 인 요소를 선택
    - javascript: var id = document.getElementById('abc');
    - javascript function: var getID = funtion(id) { return document.getElementById(id) }
       var id = getID('abc');
    - jQuery: var id = $('#abc');

# 2. jQuery  설치 과정

1. 자바 Enterprise 다운로드 받기
   - workspace 박원기강사님 전용 따로 만들기
2. jquery 사이트 접속 https://jquery.com/download/
3. 스크롤 내리다보면 linkUsing jQuery with a CDN에서 https://code.jquery.com 접속
   - CDN 라이브러리는 uncompressed, minified 둘중에 하나 쓰자 
4. 다시 위로 가서 Download the compressed, production jQuery 3.5.1 오른쪽 클릭-> 다른이름으로 링크저장-> 파일 따로 적당한곳에 저장
5. 이클립스 실행 -> dynamic web project(web, maven, spring 작업) 실행
   - 이름 kh-web-2020 으로 하고 그냥 다 next 후 finish(초기에 톰캣 설정 안함)
     - 기존 java 프로젝트 필요하면 src안에 패키지만들고 넣으면 똑같이 작동한다.
  
# 3. eclipse EE 버전 설치 후 환경 설정

1. 한글 인코딩(UTF-8)
   - 메뉴중 window -> preferences -> enc검색(나오는 6개 파일 전부다 변환 해줘야한다.)  
     - content types -> text -> text editor 누른 후 Default encoding: utf-8 로 써주고 업그레이드
   - 나머지들도 전부 utf-8로 바꿔주고 저장

2. 자바 연결 확인: 메뉴중 window -> preferences -> java -> installed JREs 눌러서 제대로 됐는지 설정 확인  
3. 웹서버 설정: JSP 컨테이너 (톰캣)
   - 메뉴중 window -> preferences -> Server -> Runtime Environment(처음 깔면 아무것도 없다) -> add 버튼 눌러서 톰캣9 버전 클릭 후 next -> browse 눌러서 톰캣 설치 파일 경로 설정 후 finish -> 빨간표시 없으면 끝난것 적용누르고 끝.
4. 현재 프로젝트에 웹서버 설정
   - 화면 밑쪽에 Server에서 밑에 글 클릭 -> 톰캣9 누르고 next -> 패키지 add 하고 finish -> start the server(스타트 버튼) 눌러서 정상 작동 확인.
   - 등록된 서버 더블클릭하면 Overview 화면이 나온다.
     - HTTP/1.1 port number 9090 확인 이상없으면 끄기

# 4. 수업 시작전 test & 준비

1. package에서 WebContent 오른쪽 클릭 -> new -> JSP File -> 제목 intro(아무거나 상관없음)로 입력 -> 알아서 확장자 설정된다.
   - 빨간표시 원인: 서버등록하고 서버연결 하긴 했지만 이클립스 버그? 라이브러리를 제대로 인식하지 못해서 생긴 경우이다.
   - 조치방법 : kh-web-2020(상위 프로젝트) 오른쪽클릭 Bulid Path 눌러서 들어간다 ->  Libraries 쪽 들어가서 Modulepath에다가 add Library -> Server Runtime -> 등록된 톰캣버전이 나온다. 클릭! finish

# 5. 용어

1. `console(입력)`
   - java console: System.out.println(), System.in()
   - javascript: document.write(), console.log()
   - jQuery: out.print("hi jQuery...");

`--------------jsp 표현--------------`
2. `<%  %>`
   - 스크립틀릿(scriptlet): java coding area

3. `<%@  %>`
   - 지시문(direction): 페에지의 여러가지 속성들을 명시(시스템사용언어, 텍스트타입, 인코딩타입)

4. `<%!  %>`
    - 선언문
    
5. `<%=  %>`
    - 표현식(out.print 역할을 한다. (브러우저에 출력해 달라))

# 6. CDN 방식으로 jQuery 라이브러리 가져오기

1. jquery 사이트 접솝 https://jquery.com/download/ 후 스크롤 내리다보면 linkUsing jQuery with a CDN에서 https://code.jquery.com 접속
  - uncompressed, minified 둘중에 하나 쓰면됨.
  - uncompressed 누른후 코드 복사
2. jsp 파일에 title밑쪽에 붙여넣기
3. 적용 잘 됐는지 실습해보기(body안에)
  - <%   // 스크립틀릿(scriptlet) : java coding area

   // java console: System.out.println(), System.in()
   // javascript: document.write(), console.log()
   // jQuery: out.print("hi jQuery...");
   out.print("hi jQuery...");   

    %>
    <h1 id='header'>jQuery Test를 위한 태그</h1>
    <script>
    $('#header').css('color','#f00');
    </script>

`---------------------------------------`
4. 네트워크가 안되는 곳에서는 사용 불가 하다.
5. 그래서 CDN 말고 jQuery 라이브러리 다운받아 놓고 사용

# 7. jQuery 라이브러리 가져오기

 - WebContent에서 new -> Folder로 library 이름의 폴더 만들기
 - 기존 Download the compressed, production jQuery 3.5.1 파일 다운로드 받은 폴더에서 library 폴더에 드래그 해서 옮기기
 - 작동시킬 jsp 파일에 library 폴더 가져오기 위해서, title 밑에 sctipt 작성하기(<script src='./library/jquery-3.5.1.min.js'></script>)
 - 정상작동 확인

# 8. css와 jQuery 선택자 구분

- CSS
 - id: #ele {...} `-->` $('#ele').css('key', 'value');
 - class: .ele
 - #ele > ele
 - #ele ele
 - #ele[]

# 9. DOM

- DOM: HTML을 트리구조로 표현한것.
  - jQuery의 DOM 표현 방식
    - selectors, Attributes, Traversing, Manipulation, CSS, Event, Effects, Ajax


# 10. 다시 실습1

- WebContent에 index.jsp 파일 만들기

```html
<!--(html만 사용)-->
<html>
<head>
<meta charset="UTF-8">
<title>index</title>
</head>
<body>
<div id='index'>
   <header id='header'>헤더</header>
   <section id='contents'>contents</section>
   <footer id='footer'>융합 S/W 개발(주)</footer>
</div>
</body>
</html>
```

```html
<!--(jQuery만 사용하여 꾸미기)-->
<html>
<head>
<meta charset="UTF-8">
<title>index</title>

<script src="https://code.jquery.com/jquery-3.5.1.js"
      integrity="sha256-QWo7LDvxbWT2tbbQ97B53yJnYU3WhH/C8ycbRAkjPDc="
      crossorigin="anonymous">      
</script>

</head>
<body>
<div id='index'>
   <header id='header'>헤더</header>
   <section id='contents'>contents</section>
   <footer id='footer'>융합 S/W 개발(주)</footer>
</div>
<script>

   // 모든 기본태그의 마진,패딩 0으로 설정
   $('*').css('margin', '0px');
   $('*').css('padding', '0px');
   // 패딩 증가의 영향을 최소화 하기 위한 조치
   $('*').css('box-sizing', 'border-box');
   
   // index 페이지의 넓이와 중앙배치
   $('#index').css('width', '1000px');
   $('#index').css('margin', 'auto');
   
   // header의 높이와 바탕색
   $('#index>#header').css('height', '150px');
   $('#index>#header').css('background-color', '#ccc');

   // contents는 최소 높이만
   $('#contents').css('min-height', '250px'); //[min-height] 최소값보다 작아지지않고 범위가 넓어지면 알아서 벌어진다. [height] 길이가 고정된다.
   
   // footer
   $('#index>#footer').css('height', '100px');
   $('#index>#footer').css('background-color', '#bbb');
   $('#index>#footer').css('text-align', 'center');
   $('#index>#footer').css('line-height', '100px');   
   
</script>
</body>
</html>
```

# 11. JSON 표기 방법

1. KEY:VALUE => {KEY:VALUE, KEY:VALUE, ...}
2. 배열 => [v1, v2, v3, ...]
3. 혼합형 : 배열안에 map
            [ { }, { }, ...]
            map안에 배열
            { key : [,,,,] }

# 11.1. KEY:VALUE => {KEY:VALUE, KEY:VALUE, ...}

```js
<!--footer JSON 표기법으로-->
$('#index>#footer').css({
        'height' : '100px',
        'background-color' : '#bbb',
        'text-align' : 'center',
        'line-height' : '100px'
        });
```

# 12. 다시 실습2 (aside 생성)

```html
<html>
<head>
<meta charset="UTF-8">
<title>index</title>

<script src="https://code.jquery.com/jquery-3.5.1.js"
      integrity="sha256-QWo7LDvxbWT2tbbQ97B53yJnYU3WhH/C8ycbRAkjPDc="
      crossorigin="anonymous">      
</script>

</head>
<body>
<div id='index'>
   <header id='header'>헤더</header>
   <section id='contents'>
   content
      <article class='content'></article>
      <aside class='aside'>
         <img src='http://placehold.it/150x100'>
      </aside>
   </section>
   <footer id='footer'>융합 S/W 개발(주)</footer>
</div>
<script>

   // 모든 기본태그의 마진,패딩 0으로 설정
   $('*').css('margin', '0px');
   $('*').css('padding', '0px');
   // 패딩 증가의 영향을 최소화 하기 위한 조치
   $('*').css('box-sizing', 'border-box');
   
   // index 페이지의 넓이와 중앙배치
   $('#index').css('width', '1000px');
   $('#index').css('margin', 'auto');
   
   // header의 높이와 바탕색, 마진
   $('#index>#header').css('height', '150px');
   $('#index>#header').css('background-color', '#ccc');
   $('#index>#header').css('margin-top', '5px');

   // contents는 최소높이와 마진
   $('#index>#contents').css({'margin-top':'3px', 'margin-bottom':'3px'});
   $('#contents').css('min-height', '250px'); //[min-height] 최소값보다 작아지지않고 범위가 넓어지면 알아서 벌어진다. [height] 길이가 고정된다.
   
   // footer
   //$('#index>#footer').css('height', '100px');
   //$('#index>#footer').css('background-color', '#bbb');
   //$('#index>#footer').css('text-align', 'center');
   //$('#index>#footer').css('line-height', '100px');
   
   // footer JSON 표기법으로
   $('#index>#footer').css({
        'height' : '100px',
        'background-color' : '#bbb',
        'text-align' : 'center',
        'line-height' : '100px'
        });
   
   // .content
   
   $('#index>#contents>.content').css({'width':'650px', 'float':'left'});
   
   // .aside
   
   $('#index>#contents>.aside').css({'width':'150px', 'float':'right'});

</script>
</body>
</html>
```

# 13. filter 

- 선택된 요소중 필요한 요소만을 걸려내는 방법
- 접두문자 ':' 을 사용한다
- 필터와 필터를 연결하여 사용할 수 있다

# 13.1. filter 종류

- :eq(index)
- :even(짝수)
- :odd(홀수)
- :first(처음)
- :last(마지막)
- :gt(index)
- :lt(index)

# 13.2 filter 기능 익히기(선택된 요소 중 필요한 요소만을 걸려내는 방법)

- WebContent -> jquery 폴더 생성 -> filter.jsp 파일 만들기

```html
<html>
<head>
<meta charset="UTF-8">
<title>filter</title>
<script src='../library/jquery-3.5.1.min.js'></script>
</head>
<body>
<div id='filter'>
   <fieldset>
      <legend>filter</legend>
      <ul>
         <li>선택된 요소중 필요한 요소만을 걸려내는 방법</li>
         <li>접두문자 ':' 을 사용한다</li>
         <li>필터와 필터를 연결하여 사용할 수 있다</li>
         <li>종류=> :eq(index) , :even(짝수) , :odd(홀수) , :first(처음) , :last(마지막) , :gt(index) , :lt(index)</li>   
      </ul>      
   </fieldset>
   
   <ol start='0'>
      <li>백두산</li>
      <li>금강산</li>
      <li>설악산</li>
      <li>치악산</li>
      <li>내장산</li>
      <li>속리산</li>
      <li>맛동산</li>
      <li>한라산</li>   
   </ol>
   
   <div id='items'>
      <div>방가...1</div>
      <div>방가...2</div>
      <div>방가...3</div>
      <div>방가...4</div>
      <div>방가...5</div>
      <div>방가...6</div>
      <div>방가...7</div>   
   </div>
   
</div>
<script>
   // ol 태그안에 있는 첫번째 li
   $('#filter>ol>li:first').css('color','#f00');
   $('#filter>ol>li:odd').css('font-weight','bolder');

   // items안에 있는 div의 바탕색상을 홀짝수로 나뉘어 표시되도록 구성
   $('#filter>#items>div:even').css({'background-color':'red', 'color':'yellow'})
   $('#filter>#items>div:odd').css({'background-color':'blue', 'color':'white'})   
</script>
</body>
</html>
```

# 14. 파일 링크 합치는 방법
https://news.naver.com/main/read.nhn`?`mode=LSD`&`mid=shm`&`sid1=100`&`oid=277`&`aid=0004801335

- ?: 파라미터의 시작을 알리는것
- &: 파라미터들을 연결하는 연결자 역할
- 파라미터: key값:value값

파일 링크 가져오는 방법
  - css: <link/> 
  - javascript: <script src: >
  - jsp
     - 동적 include
     - 정적 include

# 14.1. 동적 include (index에 filter파일 합치기)

동적 include로 index 파일에 intro파일 합치기

```jsp
<body>
<!--index.jsp 파일이다-->
<%
   //request.getParameter(key) 쓰면 value값을 리턴해준다.
   String incc = "intro.jsp";
   if(request.getParameter("inc") != null){
      incc = request.getParameter("inc");
   }
%>
<div id='index'>
   <header id='header'>헤더</header>
   <section id='contents'>
   content
      <article class='content'>
         <jsp:include page="<%=incc %>"></jsp:include> <!-- 동적 include -->
      </article>   
</body>
```
`-------------------------------------------------------------------------------`

주소창 index.jsp?`inc`=/jquery/filter.jsp에서도 조정 가능
- `inc`=/jquery/traverse.jsp라고 하면 traverse파일이 온다.

```jsp
 <%
  String incc = "intro.jsp";  //가져올 파일 경로와 파일명을 입력
    if(request.getParameter("inc") != null){
      incc = request.getParameter("inc");
   }
 %>
 <article class='content'>
   <jsp:include page="<%=incc %>"></jsp:include> <!--동적 include -->
 </article>
```

inc를 바꾸면 동일한 inc 다 바꿔줘야 한다.
incc도 바꾸면 동일한 incc 다 바꿔줘야 기능을 한다.
