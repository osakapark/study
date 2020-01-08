# ASP.NET Core Identity 소개
## 1. Asp.net core identity (spring sequrity 유사)
- 로그인, 로그아웃, 개별인증 (관리자, 일반 사용자)
- 일반 사용자 (나이가 19세 이상)

## 2. 프로젝트 생성 (IdentityTest)
 Visual C# > .NET Core > ASP.NET Core 웹 응용 프로그램 > 웹응용프로그램 > 개별사용자 인증(인증변경)
 <br>update-database : 패키지 관리자 콘솔, sql server 개체 탐색기에서 확인
 
## 3.Identy 테이블 구조

IdentityUser 에서 상속해서 테이블 칼럼 생성 
- dbo.AspNetUsers => ApplicationUser (유저 클래스)
  - ID,비밀번호, 사용자 이름..
- dbo.AspNetRoles => ApplicationRole (롤 클래스, 실제 클래스는 안보임)
  - 이 웹사이트의 회원 분류 - 슈펴관리자, 관리자, 특별사용자, 일반사용자
- dbo.AspNetUserRoles => 사용자 와 Roles 테이블 연결  
  - 1번 사용자 (PK 1)  -> 관리자 (PK 1)
  - 2번 사용자 (PK 2) -> 일반사용자 (PK 4)
- dbo.AspNetUserClaims => 5명의 사용자 -> 일반 사용자(Role)
  <br>2명이 나이가 20세 미만 -> 20세 미만은 특정 게시판 진입 금지
  <br>Age, 20 => annotation 1줄로 [HttppGet, Authroize(Claims.. Age > 20)]
- dbo.AspNetRoleClaims => Role 자체의 제한
<br>관리자 - 1등(슈퍼관리), 2등(일반 관리자), 3등(부관리자)
- dbo.AspNetUserLogins  => 외부 로그인 지원
   <br> Auth 1.0, / 2.0 (예 : 구글ID로 자동 로그인)    
- dbo.AspNetUserTockens => 외부 로그인  관련

- EFMigrationsHistory (무시, code first 마이그레이션 이력, 삭제는 안하는게 좋음)
