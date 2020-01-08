# 17. Enterprise Architecture 구성하기 (실습)
## 1.빈 솔루션 만들기
솔루션 > 추가 > 새 솔루션 폴더
<br>실제 폴더가 만들어 지지는 않음 (가상의 단위 분리)
<br>프로젝트 삭제 시, 실제 파일/폴더는 삭제 되지 않음. 직접 삭제할 것
- BusinessLogicLayer
- DataAccessLayer
- PresentationLayer
- Common


## 2. Common
 - Note.Model  프로젝트 생성
    - Common > 새 프로젝트 추가 > Visual C# > .Net Core > 클래스 라이브러리(.NET Core)
    - naming :  solution 이름 + Model
 - User, Note Class 생성

## 3. DataAccessLayer
  - Note.IDAL 프로젝트 생성  (interface 만 모아둔 프로젝트, IData Access Layer, 느슨한 결합, DI 목적)
  - Note.DAL 프로젝트 생성 (실제 코드)
  
## 4.BusinessLogicLayer
  - Note.BLL 프로젝트 생성
  - 종속성 정리 (Note.DAL 있으면 제거)
 
## 5. PresentationLayer
  - Note.MVC6 프로젝트 생성
  - Visual C# > 웹 > ASP.NET Core 웹 응용프로그램 > (.NET Core, ASP.NET Core 1.1 선택) 웹 응용프로그램
  
---
#  Enterprise Architecture 구성하기 (참고)
## 1.Enterprise Architecture
 1. 대형 솔루션, 프로그램 개발 사용하는 프로젝트 구성 방식
     - 큰 틀을 구성
 2. 다양한 플랫폼을 지원하는 재 사용성이 높은 코드를 작성하는 방식

## 2.Cache (캐시 기능도 중복없이  하나로 모은다. => 비지니스 로직 레이어)
  - 목적 : 자주 불러오는 데이터를 메모리에 담아서 출력하는 것
  - 장점 : 컴퓨팅 비용 감소, 데이터 출력 속도 증가
  - 단점 : 메모리를 많이 필요로 하게 됨.

## 3. 클래스 라이브러리 종류
1. net Framework 
2. net Core
3. net Framework (Portable) -Xamarin
4. net Standard (1,2,3 호환 안되 던 것 해결, 아직 preview 단계)

예1) 프레임 우크가 섞이면 안됨
<br>Asp.net 프로젝트  (닷넷프레임워크)
<br>data 클래스 라이브러리 (닷넷 코어)

## 4. 각 Tier 별 접근 순서
클라이언트 버튼 -> ASP.NET MVC 리스트 출력 호출 -> BLL -> IDAL -> DAL
