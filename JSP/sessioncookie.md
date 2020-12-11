# 1. Session, Cookie

Session 이나 application 이나 코딩 식은 똑같지만 의미는 다르다.
   - Session은 개인공간, application 공동공간
   - Session은 
     1. 여러사람 개별적으로 접속 가능
     2. 브라우저마다 저장된 각 세션번호가 다르다.(세션번호가 다르면 다른 사용자로 인식)
     3. 브라우저를 끄기 전까지 세션은 저장되어 계속 볼수 있다. */

# 2. 서블릿에서 사용 할 수 있는 상태 저장소

1. 서버쪽
   - (Application, Session) 저장소
2. 클라이언트쪽
   - Cookie 저장소   