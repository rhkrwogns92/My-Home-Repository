# 목차

- [목차](#목차)
- [1. mybatis 이클립스 사용하기](#1-mybatis-이클립스-사용하기)
	- [1.1. config.xml 파일(기본 xml 코드 틀)](#11-configxml-파일기본-xml-코드-틀)
	- [1.2. BoardFactory.java 파일 만들기](#12-boardfactoryjava-파일-만들기)
	- [1.3. BoardDao.java(처음 테스트)](#13-boarddaojava처음-테스트)
	- [1.4. config.xml 안에 mapper 작성(mapper로 board.xml 참조)](#14-configxml-안에-mapper-작성mapper로-boardxml-참조)
	- [1.4. sql에 board 테이블 생성](#14-sql에-board-테이블-생성)
	- [1.5. board.xml 작성 select문 연습! (모든 serial값 / 한개의 serial값 각각 선택하기)](#15-boardxml-작성-select문-연습-모든-serial값--한개의-serial값-각각-선택하기)
	- [1.6. BoardDao.java 다시 작성(모든 serial값, 한개의 serial값 각각 출력)](#16-boarddaojava-다시-작성모든-serial값-한개의-serial값-각각-출력)
	- [1.7. board.xml에 BoardVo 적용시키기](#17-boardxml에-boardvo-적용시키기)
	- [1.8. BoardDao.java 적용시키기](#18-boarddaojava-적용시키기)
- [2. insert문 연습!](#2-insert문-연습)
- [3. update문 연습!](#3-update문-연습)
- [4. delete문 연습!](#4-delete문-연습)
- [5. 게시판 요구분석](#5-게시판-요구분석)
	- [5.1. UI 설계](#51-ui-설계)
		- [5.1.1. select.jsp (조회)](#511-selectjsp-조회)
		- [5.1.2. insert.jsp (입력)](#512-insertjsp-입력)
		- [5.1.3. update.jsp (수정)](#513-updatejsp-수정)
		- [5.1.4. view.jsp (상세보기)](#514-viewjsp-상세보기)
		- [5.1.5. result.jsp (결과)](#515-resultjsp-결과)

# 1. mybatis 이클립스 사용하기

1. https://blog.mybatis.org/ -> products -> Mybatis3 다운로드 -> 최신버전 다운로드 -> 압축 푼 후 -> WEB-INF-> lib 폴더 안에 파일을 붙여 넣기 한다.
2. Eclips src안에 board 패키지 만들기 -> config.xml 파일 만들기 -> 안에 입력하기
3. BoardFactory.java 파일 만들기
4. BoardDao.java

## 1.1. config.xml 파일(기본 xml 코드 틀)

config.xml
```java
<?xml version="1.0" encoding="UTF-8"?>

<!DOCTYPE configuration PUBLIC "-mybatis.org//DTD Config 3.0//EN"
	"http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	<environments default="development">
		<environment id="development">
			<transactionManager type="JDBC"/>
			<dataSource type="POOLED">
				<property name="driver" value="oracle.jdbc.driver.OracleDriver"/>
				<property name="url" value="jdbc:oracle:thin:@localhost:1521:XE"/>
				<property name="username" value="system"/>
				<property name="password" value="oracle"/>			
			</dataSource>	
		</environment>
	</environments>
	
	<mappers>	
	
    CRUD 코드 작성하는 곳
		
	</mappers>
</configuration>
```

## 1.2. BoardFactory.java 파일 만들기

- JDBC에서 Connection conn 의 역할을 한다.

BoardFactory.java
```java
package board;

import java.io.Reader;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

public class BoardFactory {
	private static SqlSessionFactory factory;
	
	static {
		try {
			Reader reader = Resources.getResourceAsReader("board/config.xml");
			factory = new SqlSessionFactoryBuilder().build(reader);
			
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
	
	public static SqlSessionFactory getFactory() {
		return factory;
	}	
}

```

## 1.3. BoardDao.java(처음 테스트)

```java
package board;

import org.apache.ibatis.session.SqlSession;

public class BoardDao {

	SqlSession sqlSession;
	
	public BoardDao() {
		try {
			sqlSession = BoardFactory.getFactory().openSession();
			
			if(sqlSession == null) {
				System.out.println("연결중 오류 발생");
			}else {
				System.out.println("연결 성공!");
			}
			
			sqlSession.close();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
	
	public static void main(String[] args) {
		new BoardDao();
	}	
}
```

## 1.4. config.xml 안에 mapper 작성(mapper로 board.xml 참조)

- `<mapper resource="board/board.xml"/>` 추가

```xml
<?xml version="1.0" encoding="UTF-8"?>

<!DOCTYPE configuration PUBLIC "-mybatis.org//DTD Config 3.0//EN"
	"http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	<environments default="development">
		<environment id="development">
			<transactionManager type="JDBC"/>
			<dataSource type="POOLED">
				<property name="driver" value="oracle.jdbc.driver.OracleDriver"/>
				<property name="url" value="jdbc:oracle:thin:@localhost:1521:XE"/>
				<property name="username" value="system"/>
				<property name="password" value="oracle"/>			
			</dataSource>	
		</environment>
	</environments>
	
	<mappers>	
		<mapper resource="board/board.xml"/> <!-- 추가 -->
	</mappers>
</configuration>
```

## 1.4. sql에 board 테이블 생성

```text
/* 게시판 테이블 생성 및 시퀀스 생성 */
create table board(
    serial  integer primary key,
    mid     varchar2(20),
    pwd     varchar2(20),
    subject varchar2(200),
    doc     varchar2(3000),
    mdate   varchar2(20),
    hit     integer,
    pserial integer
);

/* 첨부파일 */
create table boardAtt(
    serial integer primary key,
    board_serial integer,
    oriFile varchar2(200),
    sysFile varchar2(200)
);

/* board, boardAtt 시퀀스 */
create sequence seq_board;
create sequence seq_boardAtt;

select * from board;

/* 임의 데이터 10건 입력 */
insert into board values(seq_board.nextval, 'park','1111','이건뭐지1','안녕하세요1',sysdate,'10','');
insert into board values(seq_board.nextval, 'kwak','1111','이건뭐지2','안녕하세요2',sysdate,'11','');
insert into board values(seq_board.nextval, 'cho','1111','이건뭐지3','안녕하세요3',sysdate,'5','');
insert into board values(seq_board.nextval, 'jeon','1111','이건뭐지4','안녕하세요4',sysdate,'7','');
insert into board values(seq_board.nextval, 'king','1111','이건뭐지5','안녕하세요5',sysdate,'10','');
insert into board values(seq_board.nextval, 'hong','1111','이건뭐지6','안녕하세요6',sysdate,'10','');
insert into board values(seq_board.nextval, 'lee','1111','이건뭐지7','안녕하세요7',sysdate,'7','');
insert into board values(seq_board.nextval, 'kang','1111','이건뭐지8','안녕하세요8',sysdate,'15','');
insert into board values(seq_board.nextval, 'kong','1111','이건뭐지9','안녕하세요9',sysdate,'20','');
insert into board values(seq_board.nextval, 'chu','1111','이건뭐지10','안녕하세요10',sysdate,'3','');

commit;
```

## 1.5. board.xml 작성 select문 연습! (모든 serial값 / 한개의 serial값 각각 선택하기)
  
- select는 선택된 값의 수에 따라 selectList(두개이상), slectOne(한개)이 있다. 테스트 해보기
- 모든 serial만 선택해 반환 해보기

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
	PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
	"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
	
<mapper namespace="board"> 
	<select id="all_serial" resultType="int"> <!-- 이 파일 접근 하려면 "board.all_serial" 하면 된다. -->
		select serial from board <!-- 모든 serial만 선택해 반환함 -->	
	</select>	
	
	<select id="one_serial" resultType="int"> 
		select serial from board where serial=1 <!-- 한가지의 serial만 선택해 반환함 -->	
	</select>	
</mapper>
```

## 1.6. BoardDao.java 다시 작성(모든 serial값, 한개의 serial값 각각 출력)

- 선택한 모든 serial 출력 해보기
    - 모든 serial값을 가져옴
    - List<Integer> list = sqlSession.selectList("board.all_serial");
	- System.out.println(Arrays.toString(list.toArray()));


```java
package board;

import java.util.Arrays;
import java.util.List;

import org.apache.ibatis.session.SqlSession;

public class BoardDao {

	SqlSession sqlSession;
	
	public BoardDao() {
		try {
			sqlSession = BoardFactory.getFactory().openSession();
			
			if(sqlSession == null) {
				System.out.println("연결중 오류 발생");
			}else {
				System.out.println("연결 성공!");
			
				// 모든 serial값을 가져옴
				List<Integer> list = sqlSession.selectList("board.all_serial");
				System.out.println(Arrays.toString(list.toArray()));
			
				// 한개의 serial값을 가져옴
				int serial = sqlSession.selectOne("board.one_serial");
				System.out.println("serial : " + serial);
			}
			
			
			sqlSession.close();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
	
	public static void main(String[] args) {
		new BoardDao();
	}	
}
```

## 1.7. board.xml에 BoardVo 적용시키기

- BoardVo 파일 만들기
  - db데이터 board를 참고하여 작성하기
- board.xml 파일에 BoardVo를 참조하기 위해 작성하기
    - `<select id="list" resultType="board.BoardVo"> `<br>
          `select * from board`<br>
	  `</select>`<br>





boardVo.java
```java
package board;

public class BoardVo {

	int serial;
	String mid;
	String pwd;
	String subject;
	String doc;
	String mdate;
	int hit;
	int pserial;
	
	
	public int getSerial() {
		return serial;
	}
	public void setSerial(int serial) {
		this.serial = serial;
	}
	public String getMid() {
		return mid;
	}
	public void setMid(String mid) {
		this.mid = mid;
	}
	public String getPwd() {
		return pwd;
	}
	public void setPwd(String pwd) {
		this.pwd = pwd;
	}
	public String getSubject() {
		return subject;
	}
	public void setSubject(String subject) {
		this.subject = subject;
	}
	public String getDoc() {
		return doc;
	}
	public void setDoc(String doc) {
		this.doc = doc;
	}
	public String getMdate() {
		return mdate;
	}
	public void setMdate(String mdate) {
		this.mdate = mdate;
	}
	public int getHit() {
		return hit;
	}
	public void setHit(int hit) {
		this.hit = hit;
	}
	public int getPserial() {
		return pserial;
	}
	public void setPserial(int pserial) {
		this.pserial = pserial;
	}
	
	
}

```

board.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
	PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
	"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
	
<mapper namespace="board"> <!-- 이 파일 접근 하려면 "board.all_serial" 하면 된다. -->
	
	<select id="all_serial" resultType="int"> 
		select serial from board <!-- 모든 serial만 선택해 반환함 -->	
	</select>	
	
	<select id="one_serial" resultType="int"> 
		select serial from board where serial=1 <!-- 한가지의 serial만 선택해 반환함 -->	
	</select>	
	
	<select id="list" resultType="board.BoardVo"> <!-- board 테이블에 있는 모든 정보를 BoardVo 안에 담아달라는 뜻. 개수가 2개이상이면 자동으로 ListCollection이 된다. -->
		select * from board	
	</select>
		
	<select id="view" resultType="board.BoardVo" parameterType="int">
		select * from board where serial = #{_parameter}
	</select>	
		
</mapper>
```

## 1.8. BoardDao.java 적용시키기

```java
package board;

import java.util.Arrays;
import java.util.List;

import org.apache.ibatis.session.SqlSession;

public class BoardDao {

	SqlSession sqlSession;
	
	public BoardDao() {
		try {
			sqlSession = BoardFactory.getFactory().openSession();
			
			if(sqlSession == null) {
				System.out.println("연결중 오류 발생");
			}else {
				System.out.println("연결 성공!");
		
				// 모든 serial값을 가져옴
				List<Integer> list = sqlSession.selectList("board.all_serial");
				System.out.println(Arrays.toString(list.toArray()));
			
				// 한개의 serial값을 가져옴
				int serial = sqlSession.selectOne("board.one_serial");
				System.out.println("serial : " + serial);
			
				
				// 모든 데이터를 BoardVo(객체형)에 담아 출력
				List<BoardVo> list2 = sqlSession.selectList("board.list");
				for(BoardVo vo: list2) {
					System.out.println(vo.getSerial() +" : "+ vo.getSubject());
				}
				
				// serial번호에 해당하는 BoardVo 한 건을 출력
				BoardVo vo = sqlSession.selectOne("board.view", 3); //(id값, parameterType) 숫자 3 자리는  board.xml 에서 parameterType 자리이다. 
				System.out.println("------------------------------------");
				System.out.println("serial : "+vo.getSerial());
				System.out.println("subject : "+vo.getSubject());
				System.out.println("mid : "+vo.getMid());
				
			}
			
			
			sqlSession.close();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
	
	public static void main(String[] args) {
		new BoardDao();
	}	
}

```



# 2. insert문 연습!

- 한 건의 데이터를 BoardVo에 담아 저장하시오.


- board.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
	PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
	"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
	
<mapper namespace="board"> <!-- 이 파일 접근 하려면 "board.all_serial" 하면 된다. -->
		
	<!-- ======================= insert ======================= -->
	<insert id="insert" parameterType="board.BoardVo">
		insert into board(serial, mid, subject) values(seq_board.nextval, #{mid}, #{subject})
	</insert>
	
</mapper>
```

- BoardDao.java

```java
package board;

import java.util.Arrays;
import java.util.List;

import org.apache.ibatis.session.SqlSession;

public class BoardDao {

	SqlSession sqlSession;
	
	public BoardDao() {
		try {
			sqlSession = BoardFactory.getFactory().openSession();
			
			if(sqlSession == null) {
				System.out.println("연결중 오류 발생");
			}else {
				System.out.println("연결 성공!");
		
				// 한 건의 데이터를 BoardVo에 담아 저장하시오.
				BoardVo vo2 = new BoardVo();
				System.out.println("------------------------------------");

				vo2.setMid("haha");
				vo2.setSubject("호우...");
				
				int cnt = sqlSession.insert("board.insert", vo2);
				if(cnt>0) {
					System.out.println("insert 성공");
					sqlSession.commit();
				}else {
					System.out.println("insert 실패");
				}			
			}			
			
			sqlSession.close();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
	
	public static void main(String[] args) {
		new BoardDao();
	}	
}

```

# 3. update문 연습!



- board.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
	PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
	"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
	
<mapper namespace="board"> <!-- 이 파일 접근 하려면 "board.all_serial" 하면 된다. -->
		
	<!-- ======================= update ======================= -->
	<update id="update" parameterType="board.BoardVo">
		update board set subject=#{subject}, doc=#{doc} where serial=#{serial} and pwd=#{pwd}
	</update>
	
</mapper>
```

- BoardDao.java

```java
package board;

import java.util.Arrays;
import java.util.List;

import org.apache.ibatis.session.SqlSession;

public class BoardDao {

	SqlSession sqlSession;
	
	public BoardDao() {
		try {
			sqlSession = BoardFactory.getFactory().openSession();
			
			if(sqlSession == null) {
				System.out.println("연결중 오류 발생");
			}else {
				System.out.println("연결 성공!");
		
				// serial = 1 인 자료의 subject과 doc를 수정하시오
				BoardVo vo3 = new BoardVo();
				System.out.println("------------------------------------");
				vo3.setSubject("수정된 제목");
				vo3.setDoc("수정된 내용");
				vo3.setSerial(3);
				vo3.setPwd("1111");
				
				int cnt = sqlSession.update("board.update", vo3);
				if(cnt>0) {
					System.out.println("수정됨");
					sqlSession.commit();
				}	
			}			
			
			sqlSession.close();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
	
	public static void main(String[] args) {
		new BoardDao();
	}	
}
```

# 4. delete문 연습!


- board.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
	PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
	"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
	
<mapper namespace="board"> <!-- 이 파일 접근 하려면 "board.all_serial" 하면 된다. -->
		
	<!-- ======================= delete ======================= -->
	<delete id="delete" parameterType="board.BoardVo">
		delete from board where serial=#{serial} and pwd=#{pwd}
	</delete>
	
</mapper>
```

- BoardDao.java

```java
package board;

import java.util.Arrays;
import java.util.List;

import org.apache.ibatis.session.SqlSession;

public class BoardDao {

	SqlSession sqlSession;
	
	public BoardDao() {
		try {
			sqlSession = BoardFactory.getFactory().openSession();
			
			if(sqlSession == null) {
				System.out.println("연결중 오류 발생");
			}else {
				System.out.println("연결 성공!");
		
				// serial과 암호를 전달받아 자료를 삭제
				BoardVo vo4 = new BoardVo();
				vo4.setSerial(10);
				vo4.setPwd("");
				
				int cnt = sqlSession.delete("board.delete",vo4);
				if(cnt>0) {
					System.out.println("삭제됨");
					sqlSession.commit();
				}
			}			
			
			sqlSession.close();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
	
	public static void main(String[] args) {
		new BoardDao();
	}	
}
```

# 5. 게시판 요구분석

1. 요구분석 (필요한 기능)
   - 여러 개의 첨부파일 기능  
2. UI 설계(wire frame, story board...)   
3. prototype web page 작성 + 퍼블리싱
4. 스크립트 작업
`--------------back 시작---------------`
5. mybatis와 mapper 파일 작성(BoardFactory...)
6. BoardServlet, BoardVo, BoardDao
7. TEST 및 배포(war)

## 5.1. UI 설계

`UI 작성`

1. WebContent/board 폴더 생성
   - select, insert, update, view, repl, result
2. WebContent/css/board.css
3. WebContent/js/board.js

`Controll(src/board)`

1. BoardFactory.java
2. config.xml, board.xml
3. BoardVo.java
4. BoardDao.java
5. BoardServlet.java

- header에 select(조회)링크 연결하기

### 5.1.1. select.jsp (조회)

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@taglib prefix="c" uri="http://java.sun.com/jstl/core_rt" %>    
    
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>select</title>
<link rel='stylesheet' type='text/css' href='./css/board.css'>
<script src='./js/board.js'></script>
</head>
<body>
<div id='board'>
	<h2 class='title_main'>게시판</h2>
	<form name='frm_board' method='post']>
		<input type='button' id='btnInsert' value='입력'>
		<div>
			<input type='text' name='findStr' id='findStr'>
			<input type='button' name='btnFind' id='btnFind' value='조회'>
		</div>
	</form>
	
	<div class='title'>
		<span class='no'>NO</span>
		<span class='subject'>제목</span>
		<span class='mid'>작성자</span>
		<span class='mdate'>작성일</span>
		<span class='hit'>조회수</span>	
	</div>
	<div class='items'>
		<c:forEach var='vo' begin='1' end='12'>
			<div class='item'>
			<span class='no'>NO</span>
			<span class='subject'>제목</span>
			<span class='mid'>작성자</span>
			<span class='mdate'>작성일</span>
			<span class='hit'>조회수</span>				
			</div>		
		</c:forEach>
	</div>
	
	<div class='btns'>
		<input type='button' value='맨첨'>
		<input type='button' value='이전'>
	</div>

	<c:forEach var='i' begin='1' end='5'>
		<input type='button' value='${i }'>
	</c:forEach>
	
	<input type='button' value='다음'>
	<input type='button' value='맨끝'>

</div>

<script>board()</script>
</body>
</html>
```

### 5.1.2. insert.jsp (입력)

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>insert</title>
<link rel='stylesheet' type='text/css' href='./css/board.css'>
<script src='./js/board.js'></script>
</head>
<body>
<div id='board'>
	<h2 class='title_main'>게시판</h2>
	<form name='frm_board' method='post'>
		<label>아이디</label>
		<input type='text' name='mid' placeholder='영숫자와!$_ 만 가능'/><br>
		
		<label>제목</label>
		<input type='text' name='subject' maxLength='200'/><br>
		
		<label></label>
		<textarea name='doc' rows="7" cols="80"></textarea><br>
		
		<label>암호</label>
		<input type='password' name='pwd' placeholder='영숫자와!$_ 만 가능'/><br>
		
		<label>첨부</label>
		<div id='attach_zone'>
			<img src="http://placehold.it/120X100">
			<img src="http://placehold.it/120X100">
			<img src="http://placehold.it/120X100">
			<img src="http://placehold.it/120X100">			
		</div>
		
		<div class='btns'>
			<input type='button' value='저장' id='btnSave'>
			<input type='button' value='목록' id='btnSelect'>
		</div>		
	</form>
</div>

<script>board()</script>
</body>
</html>
```

### 5.1.3. update.jsp (수정)

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>update</title>
<link rel='stylesheet' type='text/css' href='./css/board.css'>
<script src='./js/board.js'></script>
</head>
<body>
<div id='board'>
	<h2 class='title_main'>게시판</h2>
	<form name='frm_board' method='post'>
		<label>아이디</label>
		<input type='text' name='mid' placeholder='영숫자와!$_ 만 가능'/><br>
		
		<label>제목</label>
		<input type='text' name='subject' maxLength='200'/><br>
		
		<label></label>
		<textarea name='doc' rows="7" cols="80"></textarea><br>
		
		<label>암호</label>
		<input type='password' name='pwd' placeholder='영숫자와!$_ 만 가능'/><br>
		
		<label>첨부</label>
		<div>
			<img src="http://placehold.it/120X100">
			<img src="http://placehold.it/120X100">
			<img src="http://placehold.it/120X100">
			<img src="http://placehold.it/120X100">			
		</div>
		
		<label>새첨부</label>
		<div id='attach_zone'>
			<img src="http://placehold.it/120X100">
			<img src="http://placehold.it/120X100">
			<img src="http://placehold.it/120X100">
			<img src="http://placehold.it/120X100">			
		</div>
		
		<div class='btns'>
			<input type='button' value='수정' id='btnUpdate'>
			<input type='button' value='목록' id='btnSelect'>
		</div>		
	</form>
</div>

<script>board()</script>
</body>
</html>
```

### 5.1.4. view.jsp (상세보기)

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>view</title>
<link rel='stylesheet' type='text/css' href='./css/board.css'>
<script src='./js/board.js'></script>
</head>
<body>
<div id='board'>
	<h2 class='title_main'>게시판</h2>
	<form name='frm_board' method='post'>
		<label>아이디</label>
		<input type='text' name='mid' disabled/><br>
		
		<label>제목</label>
		<input type='text' name='subject' disabled/><br>
		
		<label></label>
		<textarea name='doc' rows="7" cols="80" disabled></textarea><br>
				
		<label>첨부</label>
		<div id='attach_zone'>
			<img src="http://placehold.it/120X100">
			<img src="http://placehold.it/120X100">
			<img src="http://placehold.it/120X100">
			<img src="http://placehold.it/120X100">			
		</div>
		
		<div class='btns'>
			<input type='button' value='수정' id='btnModify'>
			<input type='button' value='삭제' id='btnDelete'>
			<input type='button' value='댓글' id='btnRepl'>			
			<input type='button' value='목록' id='btnSelect'>
		</div>		
	</form>
</div>

<script>board()</script>
</body>
</html>
```

### 5.1.5. result.jsp (결과)

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>result</title>
<link rel='stylesheet' type='text/css' href='./css/board.css'>
<script src='./js/board.js'></script>
</head>
<body>
<div id='board'>
	<div id='msg'>
		입력|수정|삭제|댓글 처리가됨
	</div>
	<form name='frm_board' method='post'>
		<input type='button' value='목록' id='btnSelect'>
	
	</form>



</div>

<script>board()</script>
</body>
</html>
```







