# ASP Tip

## 1.property 자동생성  
  prop + tab + tab
  ```c#
  public int MyProperty { get; set; }
  ```
## 2. model class의 PK => [Key] 추가
   ctrl + . : 자동으로 using 문 추가 항목 표시
## 3. 주석 
  - XML 주석 : 개발 모드에서 마우스 위치시키면 내용 표시됨
  - block 주석 처리 : ctrl + k + c
  - block 주석 풀기 : ctrl + k + u

## 4. 단축키 
  - 이름 제안      : ctrl + space	
  - 세로 편집      : alt + shift
  - 한 줄씩 지우기 : shift + delete
  - 새항목 추가    :   - ctrl + shift + a
  - 미사용 using 문 제거 : ctrl + . 
  - 마우스 없이 화면 위/아래 이동 : ctrl + 화살표
  - ctor + tab + tab : 생성자 생성
  - F12 :정의 이동
  - F2 : 이름바꾸기 단축키
  - 키보드의 'dropdown' 키 (shift key 아래) 이용하자!
  - Method 최소화, 최대화 : ctrl + m + m
  - 탭 닫기 : 탭에 마우스 위치 + 마우스 휠 클릭
  - 상단/하단 이동 : ctrl + home, ctrl + end
  - ctr + tab + (tab) : 탭 이동
  - ctrl + shift + b : build
  - (단어위에 커서) + ctrl + shift + f :  찾기
  
## 5. connection string
  https://www.connectionstrings.com/ <br>
  서버 탐색기 > 데이터 연결 > 연결 추가
  
## 6. EntityFramework 의 FK의 virtual
  lazy loading (virtual 선언 안해도 되기는 하지만, Entityframework 에서는 권장 사용)

## 7. 디버깅 
   - F9 : 중단점
   - F11 : 1칸씩 디버깅
   - F10 : 안에 안들어가는 디버깅
   - Shift + F5 : 중단

## 8. css 직접 확인 (크게 보기)
  크롬 > 검사 > css 확인 > css를 웹 페이지 주소에 복사

## 9. BusinessLogicLayer 의 Method를 DalInterface와 동일하게 구축하고 싶다면..
잠시 IUserDal 상속후 구현 => IUserDal 상속 제거    

## 10. C# coding 가이드
- 전역변수는 _XXX 식으로 사용
- BusinessLogicLayer 는 Interface 만들지 않는다. (기능이 같아서..)
