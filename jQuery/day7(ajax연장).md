# 목차

- [목차](#목차)
  - [1. Ajax(xml) 타입 맛보기](#1-ajaxxml-타입-맛보기)
    - [1.1. emp_search_xml.jsp](#11-emp_search_xmljsp)
    - [1.2. emp_search_form_xml.jsp](#12-emp_search_form_xmljsp)
- [2. JSON(JavaScript Object Notation)](#2-jsonjavascript-object-notation)
  - [2.1 Ajax(JSON) 타입 맛보기](#21-ajaxjson-타입-맛보기)
    - [2.1.1 emp_search_xml.jsp](#211-emp_search_xmljsp)
    - [2.1.2 emp_search_form_xml.jsp](#212-emp_search_form_xmljsp)

## 1. Ajax(xml) 타입 맛보기

- xml문서라는걸 명시하기 위해서 위에 반드시 이렇게 써줘야한다
- contentType="text/html 이거를 "text/xml" 로 바꿔줘야 한다.

두 파일을 추가로 만든다.
- emp_search_form_xml.jsp
- emp_search_xml.jsp 


### 1.1. emp_search_xml.jsp 

```jsp
<?xml version='1.0' encoding='UTF-8' ?>
<%@page import="ajax.EmpVo"%>
<%@page import="java.util.List"%>
<%@page import="ajax.EmpDao"%>
<%@ page language="java" contentType="text/xml; charset=UTF-8"
    pageEncoding="UTF-8"%>

<%
	String findStr = request.getParameter("findStr");
	EmpDao dao = new EmpDao();
	List<EmpVo> list = dao.search(findStr);
%>
<emps>          <!-- xml 형식 -->
	<%for(EmpVo vo: list){ %>
	<emp>
		<id><%=vo.getEmpno()%></id>
		<name><%=vo.getEname() %></name>
		<job><%=vo.getJob() %></job>
		<sal><%=vo.getSal() %></sal>
		<deptno><%=vo.getDeptno() %></deptno>
	</emp>
	<% } %>
</emps>
```

### 1.2. emp_search_form_xml.jsp

```html
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
	let param = $('#frm').serialize();//serialize(): findStr의 결과값 전달(findStr=123(예시))
	let req = new XMLHttpRequest();
	req.open('get', './ajax/emp_search_xml.jsp?'+ param); //get타입으로 ./ajax/emp_search.jsp? 에게 오픈시켜라
	req.onreadystatechange = function(){
		//console.log(req.status+","+req.readyState);
		if(req.status==200 && req.readyState==4){
			let xml = req.responseXML;      // xml 형식
			let emps = xml.getElementsByTagName("emp") // xml 형식. 배열로 값이 넘어온다.
			let rst = '';   //문자 담을 곳
			for(var i=0; i<emps.length; i++){       // xml 형식
				var emp = emps.item(i);
				var id = emp.getElementsByTagName("id").item(0).firstChild.nodeValue;
				var name = emp.getElementsByTagName("name").item(0).firstChild.nodeValue;
				var job = emp.getElementsByTagName("job").item(0).firstChild.nodeValue;
				var sal = emp.getElementsByTagName("sal").item(0).firstChild.nodeValue;
				var deptno = emp.getElementsByTagName("deptno").item(0).firstChild.nodeValue;
				rst += "<div class='emp'>"
					+ "	<div>"+id+"</div>"
					+ "	<div>"+name+"</div>"
					+ "	<div>"+job+"</div>"
					+ "	<div>"+sal+"</div>"
					+ "	<div>"+deptno+"</div>"
					+ "</div>";								
			}			
			$('#items').html(rst);
			//HTML은 responsehtml
			//XML은 responseXML 로 해주어야 한다.	
		}
	}
	req.send();
});
</script>
```

# 2. JSON(JavaScript Object Notation)

## 2.1 Ajax(JSON) 타입 맛보기

`특정언어에 관계없이 호환이 되도록 사용하자`가 만들어진 취지

- key값과 value값

두 파일을 추가로 만든다.
- emp_search_json.jsp 
- emp_search_form_json.jsp

### 2.1.1 emp_search_xml.jsp 

```jsp
<%@page import="ajax.EmpVo"%>
<%@page import="ajax.EmpDao"%>
<%@page import="java.util.ArrayList"%>
<%@page import="java.util.Arrays"%>
<%@page import="java.util.List"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
    
<% 
   String findStr = request.getParameter("findStr");
   
   EmpDao dao = new EmpDao();
   List<EmpVo> list = dao.search(findStr);   
   List<String> sb = new ArrayList<String>();   //json 형식
    
    // [{'id':'%d', 'name':'%s', 'job':'%s', 'sal':'%d', 'deptno':'%d'}, {}]
    //json 형식
    String fmt = "{"            
    		  	+ " 'id' : '%d' ,"  
	            + " 'name' : '%s' ,"   
				+ " 'job' : '%s' ,"
				+ " 'sal' : '%d' ,"
				+ " 'deptno' : '%d' "
   				+ "}"; 
       
  for(EmpVo vo : list){
   		String str = String.format(fmt,
  			vo.getEmpno(),
  			vo.getEname(),
    		vo.getJob(),
        	vo.getSal(),
        	vo.getDeptno()
        	);        	
   		str = str.replaceAll("'","\"");
   		sb.add(str);    //json 형식
   }	   
   out.print(Arrays.toString(sb.toArray()));  //json 형식
%>
```

### 2.1.2 emp_search_form_xml.jsp

```html
<html>
<head>
<meta charset="UTF-8">
<title>form_json</title>
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
		<h2>사원조회(JSON)</h2>
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
	let param = $('#frm').serialize();//serialize(): findStr의 결과값 전달(findStr=123(예시))
	let req = new XMLHttpRequest();
	req.open('get', './ajax/emp_search_json.jsp?'+ param); //get타입으로 ./ajax/emp_search.jsp? 에게 오픈시켜라
	req.onreadystatechange = function(){
		//console.log(req.status+","+req.readyState);
		if(req.status==200 && req.readyState==4){
			let data = JSON.parse(req.responseText);	//json 형식. 키포인트이다.
			let doc = '';
			for(var i=0; i<data.length; i++){       //json 형식
				doc += "<div class='emp'>"
					 + " <div>"+data[i].id+"</div>"
					 + " <div>"+data[i].name+"</div>"
					 + " <div>"+data[i].job+"</div>"
					 + " <div>"+data[i].sal+"</div>"
					 + " <div>"+data[i].deptno+"</div>"
					 + "</div>"
			}			
			$('#items').html(doc);			
		}
	}
	req.send();
});
</script>
</body>
</html>
```