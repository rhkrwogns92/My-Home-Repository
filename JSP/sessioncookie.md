# 1. Session, Cookie

WAS: Web Application Server

Session 이나 application 이나 코딩 식은 똑같지만 의미는 다르다.
   - Session은 개인공간, application 공동공간
   - Session은 
     1. 여러사람 개별적으로 접속 가능
     2. 브라우저마다 저장된 각 세션번호가 다르다.(세션번호가 다르면 다른 사용자로 인식)
     3. 브라우저를 끄기 전까지 세션은 저장되어 계속 볼수 있다. */

# 2. 서블릿에서 사용 할 수 있는 상태 저장소

대표적인 4가지 저장소가 있다
- page
- request
- session
- application

1. 서버쪽
   - (Application, Session) 저장소
2. 클라이언트쪽
   - Cookie 저장소   

## 2.1. Application

- 사용범위: 전역범위에서 사용하는 저장 공간
- 생명주기: WAS가 시작해서 종료할 때 까지
- 저장위치: WAS 서버의 메모리

## 2.2. Session

- 사용범위: 세션범위에서 사용하는 저장 공간
- 생명주기: 세션이 시작해서 종료할 때 까지
- 저장위치: WAS 서버의 메모리

## 2.3. Cookie

- 사용범위: Web Browser별 지정한 path 범주 공간(특정 URI에따라 지정가능)
- 생명주기: Browser에 전달한 시간부터 만료시간까지
- 저장위치: Web Browser의 메모리 또는 파일

# 3. 무엇을 써야 하나
-1년 등 오랜기간에 저장하고 싶으면 무조건 Cookie에 저장해야 한다.
-특정 URI에서만 쓰는것도 Cookie에 저장하는게 좋다


# 4. 서버에서 페이지 전환 해주기(Redirect)

`response.sendRedirect("calc2.html");`
- calc2.html: 전환할 페이지 이름 입력

# 5. 동적인 페이지 