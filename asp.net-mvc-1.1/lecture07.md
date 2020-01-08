# 7. EntityFramework Core로 데이터베이스 생성하기 (실습)
Code First 특징
Model Class -> DbContext -> 실제 테이블 생성 

## 1. 프로젝트 생성 (Global JSON 없이)
  - 빈 솔루션
    - 새 프로젝트 > 설치됨 > 기타 프로젝트 형식 > Visual Studio 솔루션 > 빈 솔루션

  - 프로젝트 추가
    - 설치됨 > Visual C# > 웹 > .NET Core > ASP.NET Core 웹 응용프로그램
    - 웹 응용 프로그램(모델-뷰-컨트롤러) : .NET Core ASP.NET Core 1.1 (나머지는 그대로)
    - Properties > 응용 프로그램 > 대상 프레임워크(G) > .NET Core 1.1

### 2. 프레임워크 업그레이드 
  - Nuget 패키지 관리 (프로젝트 >오른쪽 마우스, 종속성 > 오른쪽 마우스)
    - 업데이트 (전체 선택 후 update, .NET Core 1.1 Version 관련 오류 발생 항목들은 update 하지 않아도 됨)
    - nugget.org 라고 있기는 있음

## 3. EntityFramework Core 설치 (1.X 대의 마지막 버전으로)
  (개발토끼님 강좌가 .net core 1.X 이라서, EntityFramework Core 도 1.x로 설치 하였음)
  -	Microsoft.EntityFrameworkCore by Microsoft
  - Microsoft.EntityFrameworkCore.Sqlserver by Microsoft 
  - Microsoft.EntityFrameworkCore.Tools by Microsoft   

## 4. Model Class 작성
  - Models 폴더 생성
  - Note Class 작성 

```C#
public class Note
{
    /// <summary>
    /// 게시물 번호
    /// </summary>
    [Key] //PK 설정
    public int NoteNo { get; set; }

    /// <summary>
    /// 게시물 제목
    /// </summary>
    [Required]
    public string NoteTitle { get; set; }

    /// <summary>
    /// 게시물 내용
    /// </summary>
    [Required]
    public string NoteContents { get; set; }

    /// <summary>
    /// 작성자 번호
    /// </summary>
    [Required]
    public int UserNo { get; set; }

    [ForeignKey("UserNo")]
    public virtual User User { get; set; }
}   
```
 
- User Class 작성	
```C#
public class User
{
    /// <summary>
    /// 사용자 번호
    /// </summary>
    [Key] //PK 설정
    public int UserNo { get; set; }

    /// <summary>
    /// 사용자 이름
    /// </summary>
    [Required] // Not Null 설정
    public string UserName { get; set; }

    /// <summary>
    /// 사용자 ID
    /// </summary>
    [Required] // Not Null 설정
    public string UserId { get; set; }

    /// <summary>
    /// 사용자 비밀번호
    /// </summary>
    [Required] // Not Null 설정
    public int UserPassword { get; set; }
}
```	

## 5. DbContext 생성 -> 테이블을 생성할 수 있는 코드를 작성
  - DataContext 폴더 생성 (폴더 이름은 자유)
  - AspnetNoteDbContext Class 생성 (클래스 이름도 자유)
  - @기호는 있는 그대로 전달 (특수문자 등 때문에)
  - Database 적힌 이름으로 생성됨
```C#
public class AspnetNoteDbContext : DbContext
{
    public DbSet<User> Users { get; set; }

    public DbSet<Note> Notes { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        //@기호는 있는 그대로 전달 하겠다는 의미  
        optionsBuilder.UseSqlServer(@"Server=localhost;Database=AspnetNoteDb;User Id=sa;Password=sa1234;");
    }
}
```

### 6. DbContext 통해서 -> 실제 테이블 생성
  - 패키지 관리자 콘솔 (보기 > 다른 창)
  - add-migration FirstMigratioin : Migrations 폴더에 class 생성됨
  - update-database : DB 생성됨
