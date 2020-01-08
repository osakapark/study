## 1. 사용자 로그인, 회원가입
    회원가입 -> 사용자 정보 입력 -> DB 저장
    로그인 -> 사용자 정보 입력 -> 사용자 정보 보유

## 2.Security
  - Spring Security
  - ASP.NET Identity (Security)
    - 사용자 로그인 (인증)
    - 사용자 Role (Admin, User, Power User)
    - SMS 인증, Email 인증

## 3. Session 
Session 이용한 로그인, 회원가입 구현 <br>
로그인 2가지 방법론
- Session
  - 웹 서버가 사용자 정보를 메모리에 담아 놓고 
  - 보안성이 높다 (장점) 
  - 웹 서버 메모리 부하가 높아진다 
  
- Cookie
  - 웹 서버 로그인 -> 사용자 정보 -> 브라우저에 전달
  - 쿠키로 저장
  - 웹 서버 부하가 낮아짐
  - 보안성이 낮다.

- 결론 :Cookie 
  - Cookie -> 암호화 -> 복호화 / 위변조 위험
  - SSL (위키 참조)
  - SSL -> 유료 (100만원, 20만원, 매년 결재), 무료 
  
## 4. Get, Post
- Get : DB에서 받는다 (원칙, delete를 get으로 하는 사람도 있다)
  <br> 예) http://example.com/api/GetBookList?bookNo=1 
- Post : DB에 정보 전달
  <br> 예) http://www.example.com/PostUser

