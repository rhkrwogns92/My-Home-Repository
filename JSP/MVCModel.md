# 1. MVC

- M: Model
- V: View(출력코드 및 html)
- C: Controller(java 코드)

자바 코드와 html 등의 코드가 섞여서 있는 코드들을 스파게티 코드라 한다. 바람직하지 않은 코드이다<br>
유지보수와 결함 찾기나 효율적이지 않다.

```jsp
<!-- Controller 자바코드 영역 -->
<%
	int num = 0;
	String num_ = request.getParameter("n");
	if (num_ != null && !num_.equals("")) {
		num = Integer.parseInt(num_);
	}	
%>

<!-- view 출력 코드 영역 -->
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<%if (num%2 != 0){ %>
		홀수입니다
	<%} else { %>
		짝수입니다		
	<%} %>
	
</body>
</html>
```

# 2. MVC model 1

Controller 와 View 가 물리적으로 분리되지 않은 방식이다.

한 파일 안에 Controller와 View를 분리해 따로따로 모아 놓은 것이다.

- Controller에는 자바코드만
- View에는 출력 코드만

```jsp
<!-- Controller 자바코드 영역 -->
<%
	int num = 0;
	String num_ = request.getParameter("n");
	if (num_ != null && !num_.equals("")) {
		num = Integer.parseInt(num_);
	}	
	
	String result;
		if (num%2 != 0){
			result = "홀수";
		} else {
			result = "짝수";		
		}
%>
<!-- view 출력 코드 영역 -->
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<%=result%>입니다.	
</body>
</html>
```
  
# 3. MVC model 2

Controller 와 View 가 물리적으로 분리된 방식

따로 따로 분리하려면 Controller와 View 사이의 정보를 주고 받을 저장소가 필요하다.

- java 파일에 클래스를 하나 만든다 : Controller
- WebContent에 파일을 하나 만든다 : View
- 둘의 중간을 이어줄 저장소를만든다 : Controller 안의 코드로 만듬
  -  redirect: 현재 작업한 내용과는 상관없이 새로운 요청을 하는 것.
  -  forward: 현재 작업한 내용을 이어갈 수 있도록 몬가를 공유 하는것(forward의 저장소는 request를 쓴다)

<h2>전체 흐름</h2>
Controller -> forward(작업을 이어가게함: request가 쓰인다) -> View
<br>
<br>
실행은 무조건 Controller에서 실행을 해줘야 한다.
<br>
<br>

Controller
```java
// Controller 자바코드 영역 서블릿을 만듬 
// 아래의 WebServlet은 해당페이지의 맵핑 이름이다.
@WebServlet("/spag")
public class Spag extends HttpServlet{

	@Override
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

		int num = 0;
		String num_ = request.getParameter("n");
		if (num_ != null && !num_.equals("")) {
			num = Integer.parseInt(num_);
		}	

		String result;
		if (num%2 != 0){
			result = "홀수";
		} else {
			result = "짝수";		
		}
		//forward의 저장소는 request이다."" 안에 쓴 문자는 꺼낼때 쓰는 단어 이다.
		request.setAttribute("result", result);
		
		//redirect 현재 작업한 내용과는 상관없이 새로운 요청을 하는 것.
		//forward  현재 작업한 내용을 이어갈 수 있도록 몬가를 공유 하는것
		
		//forward
        //getRequestDispatcher("") 안에는 연결하려는 jsp파일명을 써준다.
		RequestDispatcher dispatcher = request.getRequestDispatcher("spage.jsp");
		dispatcher.forward(request, response); //request의 담은 값을 request로 넘겨준다
		
	}
}
```

view
```html
<!-- view 출력 코드 영역 -->
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
    <!-- forward로 request의 담은 값을 꺼내서 이어서 진행한다. -->
	<%= request.getAttribute("result")  %>입니다.	
</body>
</html>
```

# 4. EL(Expression Language)

- View 파일에서 자바 코드를 하나도 안쓰기 위한 방법이다.
- ${ 출력값 }

view
```html
<!-- view 출력 코드 영역 -->
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
    <!-- forward로 request의 담은 값을 꺼내서 이어서 진행한다. -->
	<!-- <%=request.getAttribute("result")%>입니다.  자바 코드가 있다. -->
    
    ${result}입니다 <!-- 자바코드없이 초간단하게 출력 가능하다>

</body>
</html>
```

## 4.1. EL List를 출력할 때

view에서 List에 담긴 list의 0번째를 출력할 경우
 - ((List)request.getAttribute("list")).get(0)
EL을 쓸 경우
 - ${list[0]}

## 4.2. EL Map를 출력할 때

- Map n = new HashMap("title","제목");

view에서 Map에 담긴 n의 첫번째 key값으로 value값 출력할 경우
 - ((Map)request.getAttribute("n")).get("title")
EL을 쓸 경우
 - ${n.title}

```java
Map<String, Object> notice = new HashMap<String, Object>();
		notice.put("id", 1);
		notice.put("title", "EL은 좋아요");
		request.setAttribute("notice", notice);
```

view
```jsp
${notice.id}<br>
${notice.title}
```

# 5. EL의 데이터 저장소

EL로 뽑아 낼 수 있는 저장소들은
 - page 객체 : pageContext
 - request 객체
 - session 객체
 - application 객체 등등이 있다

같은페이지에서 사용 하는 것은 pageContext 객체라 한다.
```jsp
<%
	pageContext.setAttribute("aa", "hello");
%>
<body>
	${aa}
</body>
```

## 5.1. 주의사항

4가지 저장소에 같은 변수 값이 있다면 우선순위가 높은 곳이 출력된다.
    - 1순위: page
    - 2순위: request
    - 3순위: session
    - 4순위: application

특정 위치의 변수를 가져 오고 싶다면 Scope라는 한정사를 쓸 수 있다.(메소드가아니다 주의: 해당 메소드 쓰려고하면안된다.)
    - ${pageScope.변수명}: pageScope에 있는 변수를 가져와라
    - ${sessionScope.변수명}: session에 있는 변수를 가져와라

## 5.2. page의 내장 객체

    - pageScope: ${pageScope.변수명}
    - requestScope: ${requestScope.변수명}
    - sessionScope: ${sessopmScope.변수명}
    - applicationScope: ${applicationScope.변수명}
    - param: ${param.파람명}
    - header: ${header. }
    - cookie: ${cookie. }
    - pageContext
      - <%=pageContext.getRequest().getMethod() %
      -> ${pageContext.request.method} 로 쓸 수 있다. get을 뺴고 ()도 빼서 메소드가아닌 소스처럼 쓴다.


       