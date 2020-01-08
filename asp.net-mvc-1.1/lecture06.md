# 6. ASP.NET MVC
## 6.1 C# DB 통신 
- ADO.NET
  - (Query 직접 작성 오류 발생의 소지가 높음)
- Entriprise Library 5
  - Query 직접 작성 -> 값을 처리
  - logging
  - (Query 직접 작성 오류 발생의 소지가 높음)
- ORM 
  - Java JAP –기준  -> Hibernate
  - C# EntityFramework Mapper  1.0 ~ 7.0 
  
## 6.2 게시판 프로젝트
- 준비물 : Asp.net MVC, MS SQL, EntityFramework

- Version
  - EntityFramework 1.0 ~ 6.0 -> .net Framework
  - ASP.NT Core -> 7.0 -> EntityFramework Core
  - ASP.NET Core 1.x
  - EntityFramework Core 1.x

## 6.3 EntityFramework 개발방식 (2가지)
- Database First 방식
  - Database DBA (데이터 베이스 관리자)
  - 설계 완료, 물리적 데이터 베이스도 모두 완성된 상태
  - Database 기준으로 Application 개발
- code First 방식
  - Database 기준으로 Application 개발 역으로 
  - Code -> database 생성해서 Application 개발

## 6.4 MS SQL
- SQL  Server 2014 2016
  - SQL 엔진
  - management Studio 14(SQL Server 2014),  16(SQL Server 2016)
- MSSQL SA 비밀번호 설정 이유
  -	Web Server와 SQL Server가 다른 컴퓨터일 경우
  예 (http://db.naver.com:1433)
