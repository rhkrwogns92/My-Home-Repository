# 목차

- [목차](#목차)
- [1. Ajax](#1-ajax)


# 1. Ajax

Ajax의 특징

- 별도의 외부 플러그인이나 라이브러리, ActiveX와 같은 API 프로그램들이 필요없다.
- 브라우저내부에 있는 자바스크립트만으로 처리 가능하다.
- 비동기방식으로 서버와 통신하며, 브라우저의 새로고침 없이 특정 서버(영역)의 내용을 동적으로 변경할 수 있다.
- 프로그램 방식을 Web방식에서 App방식으로 처리할 수 있다.
- AJAX(Asynchronous JavaScript XML)의 약자
- Ajax의 가장 중추적인 역할을 하는 객체는 XMLHttpRequest 이다.


웹은 값을 입력 할 경우 전체 페이지를 새로고침 후 입력값은 초기화 되고 결과 값이 나타난다.
`<input type='text' size='6' name='su' value='${param.su}'/>` 
- ${param.su}(태그 라이브러리(el))를 사용 함으로써 값 초기화는 해결되지만, 페이지 새로고침은 고칠 수 없다.

이러한 증상은 Ajax를 사용함으로써 해결할 수 있다.(ajax를 통해 전체 페이지가 새로고침되는 현상을 피할 수 있다)

# 2. Ajax를 통해 전체 페이지가 새로고침되게 하지 않으려면,

    1. 서버에서 처리할 내용을 UI에서 분리
    2. 서버는 처리된 내용을 화면에 출력하는 로직을 추가한다.
    3. UI단에서는 XMLHttpRequest 객체를 사용하여 서버의 처리 결과를 가져온다.


UI코드, 서버코드 한파일에 통합

```html
<html>
<head>
<meta charset="UTF-8">
<title>begin</title>
<style>
	#result {
		width: 400px;
		height: 250px;
		background-color: #eee;
	}
</style>
</head>
<body>
	<div id='begin'>
				
		<p>간단 테스트(서버의 결과를 표시해 주기 위해 전체 화면이 새로고침 되느냐, 아니면 필요한 부분만 새로고침 되느냐)</p>
		<h3>홀짝 체크</h3>
		<%
        	String temp = request.getParameter("su");
	        String msg = "없습니다"; //결과 메세지
	        int su = 0;
	        if(temp != null && !temp.equals("")){
	        	su = Integer.parseInt(temp);
	        	if(su%2 == 0){
	        		msg = su+"는 짝수 입니다.";
	        	}else {
	        		msg = su+"는 홀수 입니다.";
	        	}
        	}			
         %>				
		<form name='frm' method='post'>
			<label>정수를 입력하세요.</label>
			<input type='text' size='6' name='su' value='${param.su}'/>
			<input type='submit' value='Check'/>		
		</form>
		<div id='result'><%=msg%></div>
	</div>	
</body>
</html>
```

# 2.1. 서버에서 처리할 내용을 UI에서 분리

UI코드, 서버코드 두파일로 분리

UI코드
```html
<html>
<head>
<meta charset="UTF-8">
<title>begin</title>
<style>
	#result {
		width: 400px;
		height: 250px;
		background-color: #eee;
	}
</style>
</head>
<body>
	<div id='begin'>
				
		<p>간단 테스트(서버의 결과를 표시해 주기 위해 전체 화면이 새로고침 되느냐, 아니면 필요한 부분만 새로고침 되느냐)</p>
		<h3>홀짝 체크</h3>
						
		<form name='frm' method='post'>
			<label>정수를 입력하세요.</label>
			<input type='text' size='6' name='su' value='${param.su}'/>
			<input type='submit' value='Check'/>		
		</form>
		<div id='result'><%=msg%></div>
	</div>	
</body>
</html>
```

서버 코드
```jsp
<%
	// file name : odd_even_check.jsp

	String temp = request.getParameter("su");
	String msg = "없습니다"; //결과 메세지
	int su = 0;
	if(temp != null && !temp.equals("")){
		su = Integer.parseInt(temp);
		if(su%2 == 0){
			msg = su+"는 짝수 입니다.";
		}else {
			msg = su+"는 홀수 입니다.";
		}
	}		
%>
```

# 2.2. 서버는 처리된 내용을 화면에 출력하는 로직을 추가한다.

서버코드는 `out.print(msg);` 로직 추가
UI코드는 `result div값` 삭제

```html
<html>
<head>
<meta charset="UTF-8">
<title>begin</title>
<style>
	#result {
		width: 400px;
		height: 250px;
		background-color: #eee;
	}
</style>
</head>
<body>
	<div id='begin'>
				
		<p>간단 테스트(서버의 결과를 표시해 주기 위해 전체 화면이 새로고침 되느냐, 아니면 필요한 부분만 새로고침 되느냐)</p>
		<h3>홀짝 체크</h3>
						
		<form name='frm' method='post'>
			<label>정수를 입력하세요.</label>
			<input type='text' size='6' name='su' value='${param.su}'/>
			<input type='submit' value='Check'/>		
		</form>
		<div id='result'></div>
	</div>	
</body>
</html>
```

```jsp
<%
	// file name : odd_even_check.jsp

	String temp = request.getParameter("su");
	String msg = "없습니다"; //결과 메세지
	int su = 0;
	if(temp != null && !temp.equals("")){
		su = Integer.parseInt(temp);
		if(su%2 == 0){
			msg = su+"는 짝수 입니다.";
		}else {
			msg = su+"는 홀수 입니다.";
		}
    }	
    
    out.print(msg);
%>
```

# 2.3. UI단에서는 XMLHttpRequest 객체를 사용하여 서버의 처리 결과를 가져온다.

필수 코드
1. new XMLHttpRequest();
2. .open();
3. .onreadystatechange  = function(){
    if( .satus == 200 && .readyState == 4){}
    }
4. .send();

순수하게 자바스크립트만 사용. jQuery쓰면 더 간단하게 할 수 있다.

```html
<html>
<head>
<meta charset="UTF-8">
<title>begin</title>
<style>
	#result {
		width: 400px;
		height: 250px;
		background-color: #eee;
	}
</style>
</head>
<body>
	<div id='begin'>		
		<p>간단 테스트(서버의 결과를 표시해 주기 위해 전체 화면이 새로고침 되느냐, 아니면 필요한 부분만 새로고침 되느냐)</p>
		<h3>홀짝 체크</h3>
						
		<form name='frm' method='post'>
			<label>정수를 입력하세요.</label>
			<input type='text' size='6' name='su' value='${param.su}'/>
			<input type='submit' value='Check'/>		
		</form>
		<div id='result'></div>
	</div>	
<script>
	//순수하게 자바스크립트만 사용. jQuery쓰면 더 간단하게 할 수 있다.
	// ajax를 통해 서버의 처리 결과를 result에 표시
	var frm = document.frm;
	frm.onsubmit = function(){
		var param = "?su=" + frm.su.value;
		var req = new XMLHttpRequest();
		req.open('get', './ajax/odd_even_check.jsp' + param); // 요청 정보를 생성
		req.onreadystatechange = function(){
			//req.status==200: 정상요청됨, req.readyState==4: 정상응답.
			if(req.status==200 && req.readyState==4){
				var r = document.getElementById('result');
				r.innerHTML = req.responseText; // Text는(String, html, text 다 받을 수 있다.)
			}
		}
		req.send(); // 요청 정보를 서버에 전송
		
		return false; // submit 고유의 기능을 취소
	}
</script>
</body>
</html>
```

jQuery 사용

```html
<script>
	//순수하게 자바스크립트만 사용. jQuery쓰면 더 간단하게 할 수 있다.
	// ajax를 통해 서버의 처리 결과를 result에 표시
	var frm = document.frm;
	frm.onsubmit = function(){
		var param = "?su=" + frm.su.value;
		var req = new XMLHttpRequest();
		req.open('get', './ajax/odd_even_check.jsp' + param); // 요청 정보를 생성
		req.onreadystatechange = function(){
			//req.status==200: 정상요청됨, req.readyState==4: 정상응답.
			if(req.status==200 && req.readyState==4){
			/*	var r = document.getElementById('result');
				r.innerHTML = req.responseText; */
                $('#result').html(req.responseText);
			}
		}
		req.send(); // 요청 정보를 서버에 전송
		
		return false; // submit 고유의 기능을 취소 (submit의 기능이 아닌 기본 button이라면 안써도된다.)
	}
</script>
```

```jsp
<%
	// file name : odd_even_check.jsp

	String temp = request.getParameter("su");
	String msg = "없습니다"; //결과 메세지
	int su = 0;
	if(temp != null && !temp.equals("")){
		su = Integer.parseInt(temp);
		if(su%2 == 0){
			msg = su+"는 짝수 입니다.";
		}else {
			msg = su+"는 홀수 입니다.";
		}
    }	
    
    out.print(msg);
%>
```

# 3. Ajax 응용

- %s String
- %d digit(정수)
- %c char(문자)
- %f flot(실수)

검색어를 입력하여 ename or empno or job에 포함되어 있으면
empno, ename, job, sal, deptno를 화면에 표시하시오.

- 파일 5개 생성
 - Application.java ,emp_search_form.jsp / emp_search.jsp / EmpDao.java / EmpVo.java

    1. Application: Db계정 연동 정보
    2. emp_search_form: 검색어 입력 하면 ajax로 emp_search에 정보 요청함 (UI)
    3. emp_search: 사용자의 요청 정보를 EmpDao에 전달한 후 그 결과를 다시 사용자에게 반환 (UI + Control)
    4. EmpDao(Date Access Objectd, Dao or Dto(네트워크기능)): List<EmpVo>에서 찾은 자료를 emp_search 넘김 (Control)
    5. EmpVo(value object): DB의 데이터를 GET/SET으로 만들기 (Control)

## 3.0. Application.java 파일 (control)
```java
//자바로 db 데이터 delete 하기

public class Application {

	private static final String JDBC_URL = "jdbc:oracle:thin:@localhost:1521:XE";
	private static final String DB_USER = "system";
	private static final String DB_PASS = "oracle";

	Connection conn;

	public Application() {
		try{		
			// 1. 클래스 로드
			Class.forName("oracle.jdbc.driver.OracleDriver");
			// 2. 데이터베이스에 접속
			conn = DriverManager.getConnection(JDBC_URL, DB_USER, DB_PASS);
		} catch (Exception e) {
			e.printStackTrace();			
		}
	}
	public Connection getConn() {
		return conn;
	}
}
```

## 3.1. EmpVo.java 파일 (control)
```java
public class EmpVo {

	// 검색어를 입력하여 ename or empno or job에 포함되어 있으면
	// empno, ename, job, sal, deptno를 화면에 표시하시오.
	 
	int empno;
	String ename;
	String job;
	int sal;
	int deptno;
	
	public int getEmpno() {
		return empno;
	}
	public void setEmpno(int empno) {
		this.empno = empno;
	}
	public String getEname() {
		return ename;
	}
	public void setEname(String ename) {
		this.ename = ename;
	}
	public String getJob() {
		return job;
	}
	public void setJob(String job) {
		this.job = job;
	}
	public int getSal() {
		return sal;
	}
	public void setSal(int sal) {
		this.sal = sal;
	}
	public int getDeptno() {
		return deptno;
	}
	public void setDeptno(int deptno) {
		this.deptno = deptno;
	}	
}
```

## 3.2. EmpDao.java 파일 (control)
```java
public class EmpDao {
	Connection conn;
	PreparedStatement ps;
	ResultSet rs;
	
	public EmpDao() {
		conn = new Application().getConn();		
	}
	
	public List<EmpVo> search(String findStr){
		List<EmpVo> list = new ArrayList<>();
		
		try {
			String sql = "SELECT * FROM hr.emp WHERE ename like ? or empno like ? or job like ?";
			
			ps = conn.prepareStatement(sql);
			ps.setString(1, "%"+findStr+"%");
			ps.setString(2, "%"+findStr+"%");
			ps.setString(3, "%"+findStr+"%");
			
			rs = ps.executeQuery();
			while(rs.next()) {
				EmpVo vo = new EmpVo();
				vo.setEmpno(rs.getInt("empno"));
				vo.setEname(rs.getString("ename"));
				vo.setJob(rs.getString("job"));
				vo.setSal(rs.getInt("sal"));
				vo.setDeptno(rs.getInt("deptno"));

				list.add(vo);
			}			
		} catch (Exception e) {
			e.printStackTrace();
		}finally {
			try {
				conn.close();
			} catch (SQLException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
			return list;
		}		
	}	
}
```

## 3.3. emp_search_form.jsp 파일 (UI)
```HTML
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<style>
	#items>div {
		display: inline-block;
		width: 190px;
		height: 150px;
		border: 1px solid #ccc;
		padding: 10px;
		box-shadow: 2px 2px 3px #999;
		border-radius: 9px;
	}	
</style>
</head>
<body>
	<div id='emp_search_form'>
		<form id='frm' method='post' name='frm'>
			<label>검색어를 입력하세요.</label>
			<input type='text' name='findStr'/>
			<input type='button' value='검색' id='btnFind'/>		
		</form>	
		<hr>
		<div id='items'></div>		
	</div>
<script>
$('#btnFind').on('click',function(){
	let param = $('#frm').serialize();//serialize(): findStr=123(예시)
	let req = new XMLHttpRequest();
	req.open('get', './ajax/emp_search.jsp?'+ param); //get타입으로 ./ajax/emp_search.jsp? 에게 오픈시켜라
	req.onreadystatechange = function(){
		//console.log(req.status+","+req.readyState);
		if(req.status==200 && req.readyState==4){
			$('#items').html(req.responseText);			
		}
	}
	req.send();
});

</script>
</body>
</html>
```

## 3.4. emp_search.jsp 파일 (UI + Control)

```JSP
<%@page import="ajax.EmpVo"%>
<%@page import="java.util.List"%>
<%@page import="ajax.EmpDao"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
	// 사용자의 요청 정보를 Dao에 전달한 후 그 결과를 다시 사용자에게 반환
	String findStr = request.getParameter("findStr");
	
	EmpDao dao = new EmpDao();
	List<EmpVo> list = dao.search(findStr);
	StringBuilder sb = new StringBuilder();
	String fmt = "<div class='emp'>" 
				+ " <div>사원번호:%d</div>"
				+ " <div>이름:%s</div>"
				+ " <div>직업:%s</div>"
				+ " <div>급여:%d</div>"
				+ " <div>부서번호:%d</div>"
				+ "</div>";
				
	for(EmpVo vo : list){
		String str = String.format(fmt, vo.getEmpno(), vo.getEname(), vo.getJob(), vo.getSal(), vo.getDeptno());
		sb.append(str);
	}	
	out.print(sb.toString());	
%>
```
