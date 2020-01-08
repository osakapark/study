# 1. [ASP.NET MVC] 1. ASP.NET 소개 
## 1.1 Web Form
 - builtwith.com 참고
 - Winform 을 그대로 이어 받은 것 
 - code behind 방식
 - url 이 .aspx 원칙적으로 끝남
 - 웹 페이지 내에 소스 코드 존재 (유지보수 어려움)
 
## 1.2 ASP.NET MVC
- View -> HTML CSS Javascript   (from end dev)
- Controller -> DB 통신, 기타 계산 (back end dev)
- Model -> User (사용자 정의 클래스)  (back end dev)  
 (배우기가 생각보다 까다롭지 않다)

## 1.3 Signal R
- 실시간 채팅 서비스  
- (웹 페이지가 실시간 통신할 필요 있을 때, 실제로는 mqtt 많이 씀)

## 1.4 Web API
- 데이터 베이스에서 나온 정보를 XML / JSON 형식으로 송출해주는 서비스
- RESTful API  
  예: http://www.example.com/api/GetBooKList  
  JSON 으로 데이터 넘겨 받음
- [Stateless]  단발적 DB 통신 <br>
   모든 플랫폼과 통신이 가능  
      예) Java Spring ajax  
      예) WPF Winform JavaFX -> 통상적으로 윈도우 프로그램에도 적용  
      예) 안드로이드, iOS 앱과 통신 가능  

## 1.5 ASP.NET  VS  ASP.NET CORE
- 기능상 모두 비슷함
- 차이점   
  - ASP.NET  - Full dot Net  
    - System.Net.XXX   을 주로 이용  
  - ASP.NET CORE  
    - System.Net.XXX   -> 제거  
    - 기존 느린 문제점 향상  

## 1.6 ASP.NET Versioning 
ASP.NET 4.X  - ASP.NET MVC 5  
ASP.NET 5    - ASP.NET MVC 6   (core)
