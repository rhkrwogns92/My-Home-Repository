# 목차

- [목차](#목차)
- [1. Interactions](#1-interactions)
- [1.1. Dragged](#11-dragged)
- [1.2. Droppable](#12-droppable)
- [1.3. Sortable](#13-sortable)
- [2. Widgets(대표적인것만 알아보자)](#2-widgets대표적인것만-알아보자)
- [2.1. accordion](#21-accordion)
- [2.2. autocomplete](#22-autocomplete)
- [2.3. DB랑 연동하기!!!](#23-db랑-연동하기)
- [2.3.1 DB연동 방법](#231-db연동-방법)
- [2.3.1.1 src에 class생성(Application, AutoComplete)](#2311-src에-class생성application-autocomplete)
- [2.3.1.1 jQuery 폴더의 autocomplete.jsp에 연동](#2311-jquery-폴더의-autocompletejsp에-연동)
- [2.4. menu](#24-menu)

# 1. Interactions

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

# 1.1. Dragged

- `draggable()`함수를 사용한다.

```html
<html>
<head>
<meta charset="UTF-8">
<title>dragged</title>
<link rel='stylesheet' type='text/css'
	  href='https://code.jquery.com/ui/1.12.1/themes/trontastic/jquery-ui.css'/>
<script src='https://code.jquery.com/ui/1.12.1/jquery-ui.js'></script>
<style>
	#dragged>div{
		width: 250px;
		height: 250px;
		background-color: #88f;
		opacity: 0.5;
	}
</style>
</head>
<body>
	<div id='dragged'>
		<div id='target1'>이곳을 끌어 보세요~</div>
		<div id='target2'>이곳을 끌어 보세요~</div>	
	</div>
<script>
	$('#target1').draggable();
	$('#target2').draggable();
</script>
</body>
</html>
```

# 1.2. Droppable

- droppable() 함수 사용
- 드랍했을때, 벗어났을때
  - .droppable({ drop: function(){ ... } , out: function(){ ... } });	

```html
<html>
<head>
<meta charset="UTF-8">
<title>drop</title>
<link rel='stylesheet' type='text/css'
	  href='https://code.jquery.com/ui/1.12.1/themes/trontastic/jquery-ui.css'/>
<script src='https://code.jquery.com/ui/1.12.1/jquery-ui.js'></script>
<style>
	.abc{
		background-color: #aaa;
	}
	#droppable>div {
		border: 2px solid #00f;		
	}
	#drag{
		width: 100px;
		height: 100px;
	}
	#drop{
		width: 300px;
		height: 300px;
	}	
</style>
</head>

<body>
	<div id='droppable'>
		<div id='drag'>이 부분을 옮겨 보세요</div>
		<div id='drop'>여기에 떨어뜨리세요~</div>	
	</div>
<script>
$('#drag').draggable();
	$('#drop').droppable({
		drop: function(){
			$(this).addClass('abc'); //$(this).attr('class', 'abc');
			$(this).html('참 잘했어요~~'); 
		},
		out: function(){
			$(this).html('어디가세요?????');
			$(this).removeAttr('class');
		}	
	});
</script>
</body>
</html>
```

# 1.3. Sortable

- sortable() 함수 사용
- 기존 프레임틀이 정해져있다.

```html
<html>
<head>
<meta charset="UTF-8">
<title>sortable</title>
<link rel='stylesheet' type='text/css'
	  href='https://code.jquery.com/ui/1.12.1/themes/trontastic/jquery-ui.css'/>
<script src='https://code.jquery.com/ui/1.12.1/jquery-ui.js'></script>
<style>
	#items>li{
		background-color: #F0CCBF;
		color: #FF0000;
		border: 1px solid #FEE2CB;
		list-style: none;
		width: 300px;
		height: 50px;
		text-align: center;
		line-height: 50px;
		font-weight: bold;
		font-size: 17px;
		text-shadow: 1px 1px 10px #FF0000;		
	}
	#items>li:hover{
		cursor: pointer;
	}
</style>
</head>
<body>
	<div id=sortable>
		<ul id='items'>
			<li>html5</li>
			<li>css3</li>
			<li>javascript</li>
			<li>java</li>
			<li>jdbc</li>
			<li>jQuery</li>
			<li>ajax</li>
			<li>mybatis</li>
			<li>jsp</li>
			<li>spring boot</li>
			<li>final project</li>					
		</ul>
	</div>
<script>
	$('#items').sortable();
</script>
</body>
</html>
```

# 2. Widgets(대표적인것만 알아보자)

# 2.1. accordion

- accordion() 함수사용
- 기존 프레임틀이 정해져있다.

```html
<html>
<head>
<meta charset="UTF-8">
<title>accordion</title>
<link rel='stylesheet' type='text/css'
	  href='https://code.jquery.com/ui/1.12.1/themes/trontastic/jquery-ui.css'/>
<script src='https://code.jquery.com/ui/1.12.1/jquery-ui.js'></script>
</head>
<body>
	<div id='accordion'>
		<h2>HTML5</h2>
		<div>
			<ul>
				<li>웹표준의 시작</li>
				<li>현재 5버전이지만 대중화된 버전은 4.x대 버전</li>
				<li>canvas나 localStorage등과 같은 기술이 있지만 국내에서는 아직은...</li>				
			</ul>
		</div>
		<h2>CSS3</h2>
		<h2>JavaScript</h2>
		<h2>Java</h2>
		<h2>JDBC</h2>
		<h2>jQuery</h2>
		<h2>ajax</h2>
		<h2>jsp</h2>
		<h2>spring</h2>	
	</div>
<script>
	$('#accordion').accordion();
</script>	

</body>
</html>
```

# 2.2. autocomplete

- autocomplete() 함수 사용
- 자동 완성기능

```html
<html>
<head>
<meta charset="UTF-8">
<title>autocomplete</title>
<link rel='stylesheet' type='text/css'
	  href='https://code.jquery.com/ui/1.12.1/themes/sunny/jquery-ui.css'/>
<script src='https://code.jquery.com/ui/1.12.1/jquery-ui.js'></script>
<style>
	
</style>
</head>
<body>
	<div id='autocomplete'>
		<label>성명을 입력하세요.</label>
		<input type='text' name='findStr' id='findStr'/>
		<input type='button' value='검색' id='btnFind'/>	
	</div>
<script>
	var data = ['java','jara','jagopa','jsp','javascript','like','love','life']
	$('#findStr').autocomplete({
		source : data	
        //자동완성기능(autocomplete)에 사용될 소스(source)는 data에 저장되어있다.
	});
</script>
</body>
</html>
```

# 2.3. DB랑 연동하기!!!

- DB기본 공식
  1. jdbc 드라이버 파일 복사 WEB-INF/lib
     - C:\oraclexe\app\oracle\product\11.2.0\server\jdbc\lib
  2. 드라이버 로딩(Class.forName())
  3. Connection 객체 생성(DriverManager)

# 2.3.1 DB연동 방법
  1. src에 class생성(Application, AutoComplete)
     - Application : db랑 java 연동
     - AutoComplete : jsp랑 연동할 코드 작성
  2. jQuery 폴더의 autocomplete.jsp에 연동

# 2.3.1.1 src에 class생성(Application, AutoComplete)

`Application 클래스`
```java
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

`AutoComplete 클래스`
```java
public class AutoComplete {
	Connection conn;
	PreparedStatement ps;
	ResultSet rs;
	
	public AutoComplete() {
		conn = new Application().getConn();
	}
	
	//var data = ['java','jara','jagopa','jsp','javascript','like','love','life'] => ''(x), ""(o)
	public String getData() {
		StringBuffer sb = new StringBuffer();
		
		try {
			String sql = "SELECT ENAME FROM emp";
			ps = conn.prepareStatement(sql);
			rs = ps.executeQuery();	
			//CRUD(Create Read Update Delete)(insert, select, update, delete)
			//select= executeQuery()
			//insert,update,delete= executeUpdate()
			
			sb.append("[");
			rs.next();
			sb.append("'"+rs.getString("ENAME")+"'");
			while(rs.next()) {
				sb.append(", '"+rs.getString("ENAME")+"'");	
			}			
			sb.append("]");
			
		} catch (Exception e) {
			e.printStackTrace();
		}finally {
			 String temp = sb.toString();
			 temp = temp.replaceAll("'", "\"");			
			return temp;			
		}
	}	
}
```

# 2.3.1.1 jQuery 폴더의 autocomplete.jsp에 연동

```html
<html>
<head>
<meta charset="UTF-8">
<title>autocomplete</title>
<link rel='stylesheet' type='text/css'
	  href='https://code.jquery.com/ui/1.12.1/themes/sunny/jquery-ui.css'/>
<script src='https://code.jquery.com/ui/1.12.1/jquery-ui.js'></script>
</head>
<body>
<%
	//Oracle DB 연결
	AutoComplete ac = new AutoComplete();
	String data = ac.getData();
%>
	<div id='autocomplete'>
		<label>성명을 입력하세요.</label>
		<input type='text' name='findStr' id='findStr'/>
		<input type='button' value='검색' id='btnFind'/>	
	</div>
<script>
	//var data = ['java','jara','jagopa','jsp','javascript','like','love','life']
	var data = <%=data%>
	$('#findStr').autocomplete({
		source : data	//자동완성기능(autocomplete)에 사용될 소스(source)는 data에 저장되어있다.
	});
</script>
</body>
</html>
```

# 2.4. menu

- 구조 : ul(큰틀) > li(주메뉴) > ul(작은틀) > li(작은메뉴)

```html
<head>
<meta charset="UTF-8">
<title>menu_ui</title>
<link rel='stylesheet' type='text/css'
	  href='https://code.jquery.com/ui/1.12.1/themes/sunny/jquery-ui.css'/>
<script src='https://code.jquery.com/ui/1.12.1/jquery-ui.js'></script>
<style>
	#menu {
		width: 200px;
		text-align: center;
	}
</style>
</head>
<body>
	<div id='menu_ui'>
		<ul id='menu'>
			<li><a href='' class='ui-state-disabled'>강이름</a>
				<ul>
					<li><a href=''>금강</a></li>
					<li><a href=''>한강</a></li>
					<li><a href=''>낙동강</a></li>
					<li><a href=''>나일강</a></li>
					<li><a href=''>영산강</a></li>				
				</ul>
			</li>			
			<li><a href=''>산이름</a>
				<ul>
					<li><a href=''>마이산</a></li>
					<li><a href=''>북한산</a></li>
					<li><a href=''>에베레스트</a></li>
					<li><a href=''>한라산</a></li>
					<li><a href=''>백두산</a></li>				
				</ul>
			</li>
			<li><a href=''>과일명</a>
				<ul>
					<li><a href=''>딸기</a></li>
					<li><a href=''>사과</a></li>
					<li><a href=''>수박</a></li>
					<li><a href=''>참외</a></li>
					<li><a href=''>복숭아</a></li>				
				</ul>
			</li>
			<li>
				<a href=''>동물명</a>
				<ul>
					<li><a href=''>사자</a></li>
					<li><a href=''>고양이</a></li>
					<li><a href=''>강아지</a></li>
					<li><a href=''>원숭이</a></li>
					<li><a href=''>사슴</a></li>				
				</ul>
			
			</li>			
		</ul>			
	</div>
<script>
	$('#menu').menu();
</script>
</body>
</html>
```