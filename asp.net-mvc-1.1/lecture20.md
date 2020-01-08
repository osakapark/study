# appsettings.json를 통한 ConnectionString 처리하기

## 1. Note Solution생성 (Empty 솔루션
## 2. 술루션 폴더 생성
  - BusinessLogicLayer
  - Common
  - DataAccessLayer
  - PresentationLayer
## 3. Model Class 생성
Common > Note.Model (프로젝트 생성, .NetCore Class Library)
<br> Notice.cs 생성
- code first annotation 사용
- [Key] : primary key
- [Required] : 필수 입력, Not null
```C# 
public class Notice
{
    [Key]
    public int NoticeNo { get; set; }
    [Required]
    public string NoticeTitle { get; set; }
    [Required]
    public string NoticeConntents { get; set; }
    [Required]
    public DateTime NoticeWriteDate { get; set; }
} 
```
## 4. DataAccessLayer
### interface
Note.IDAL 프로젝트 생성
```c#
public interface INoticeDal
{
    List<Notice> GetNoticeList();   //1.공지사항 리스트 출력        
    Notice GetNotice(int noticeNo); //2.공지사항 상세 출력        
    bool PostNotice(Notice notice); //3.공지사항 등록        
    bool UpdateNotice(Notice notice); //4.공지사항 수정        
    bool DeleteNotice(int noticeNo); //5.공지사항 삭제
}
```
### Class 생성Note
Note.DAL 프로젝트 생성
<br> interface 와 순서를 맞추어 주자 (vs 는 method 이름 순으로 만들어 버림)
```c#
public class NoticeDal : INoticeDal
{

    private readonly IConfiguration _configuration;

    public NoticeDal(IConfiguration configuration)
    {
        _configuration = configuration;
    }

    public List<Notice> GetNoticeList()
    {
        /*      
        using (var db = new NoteDbContext())
        {
            return db.Notices
                .OrderByDescending(n => n.NoticeNo)
                .ToList();
        }
        */
        using (var db = new NoteDbContext(_configuration))
        {
            return db.Notices
                .OrderByDescending(n => n.NoticeNo)
                .ToList();
        }
    }

    public Notice GetNotice(int noticeNo)
    {
        throw new NotImplementedException();
    }
    public bool PostNotice(Notice notice)
    {
        throw new NotImplementedException();
    }
    public bool UpdateNotice(Notice notice)
    {
        throw new NotImplementedException();
    }
    public bool DeleteNotice(int noticeNo)
    {
        throw new NotImplementedException();
    } 
}
```

## 5. BusinessLogicLayer
Note.BLL 프로젝트 생성
```C#
public class NoticeBll
{
    private readonly INoticeDal _noticeDal;

    public NoticeBll(INoticeDal noticeDal)
    {
        _noticeDal = noticeDal;
    }

    public List<Notice> GetNoticeList()
    {
        return _noticeDal.GetNoticeList();
    }

    public Notice GetNotice(int noticeNo)
    {
        if (noticeNo <= 0) throw new ArgumentException();
        return _noticeDal.GetNotice(noticeNo);
    }

    public bool PostNotice(Notice notice)
    {
        if (notice == null) throw new ArgumentException();
        return _noticeDal.PostNotice(notice);
    }

    public bool UpdateNOtice(Notice notice)
    {
        if (notice == null) throw new ArgumentException();
        return _noticeDal.UpdateNotice(notice);
    }

    public bool DeleteNotice(int noticeNo)
    {
        if (noticeNo <= 0) throw new ArgumentException();
        return _noticeDal.DeleteNotice(noticeNo);
    }
}
```

## 6.PresentationLayer
Note.MVC6 프로젝트 생성 (.net Core 1.1로)
<br> ASP.NET Core Web Application(.NET Core) 프로젝트 생성
<br> startup.cs 수정
```c#
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IConfiguration>(Configuration);   //타클래스 라이브리에서도 사용하도록 하는 기능
    
    services.AddTransient<NoticeBll>(); //DI
    services.AddTransient<INoticeDal, NoticeDal>(); //DI
    
    // Add framework services.
    services.AddMvc();    
}
```

## 7.NoteDbContext Class 생성 (DataAccessLayer)
    DataAccessLayer > Note.DAL > DataContext 폴더 생성

```c#    
public class NoteDbContext : DbContext
{
    private readonly IConfiguration  _configuration;

    public NoteDbContext(IConfiguration configuration)
    {
        _configuration = configuration;
    }

    public DbSet<Notice> Notices { get; set; }  //테이블 등록 (code first)

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        //optionsBuilder.UseSqlServer(@"Server=DESKTOP-CGQ3KQ9\SQLEXPRESS;Database=NoteDb;User Id=sa;Password=sa1234;");
        //optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=NoteDb;");
        optionsBuilder.UseSqlServer(_configuration.GetConnectionString("LocalDb"));
    }
}    
```   
ConnectionString 을  C# 코드안에 적지말아야 한다!! (보안상 문제)
- appsettings.json 에적어야 함
 
### EntityFrameworkCore (1.X 대 받을 것)
- Microsoft.EntityFrameworkCore by Microsoft
- Microsoft.EntityFrameworkCore.Sqlserver by Microsoft
- Microsoft.EntityFrameworkCore.Tools by Microsoft
  
### Package Manager Console
기본 프로젝트 => Note.DAL (MVC6 아님)
<br> PM > add-migration  NoteDb1
<br> PM > update-database

### Note.MVC6  EntityFramewwork 받을 것 (오류시에만..)
- Microsoft.EntityFrameworkCore by Microsoft
- Microsoft.EntityFrameworkCore.Tools by Microsoft
  
### localdb 보기
보기 > Sql Server 개체 탐색기
  
    
## 8.HomeController.cs (접속 테스트)
```C#
public class HomeController : Controller
{
    private readonly NoticeBll _noticeBll;

    public HomeController(NoticeBll noticeBll)
    {
        _noticeBll = noticeBll;
    }

    public IActionResult Index()
    {
        var list = _noticeBll.GetNoticeList();
        return View();
    }

    public IActionResult About()
    {
        ViewData["Message"] = "Your application description page.";

        return View();
    }

    public IActionResult Contact()
    {
        ViewData["Message"] = "Your contact page.";

        return View();
    }

    public IActionResult Error()
    {
        return View();
    }
}
```

## 9. connection string 을 appsettings.json 로 이동하기
 - optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=NoteDb;");  //NoteDbContext.cs
 - appsettings.json 은 (localdb) 뒤에  \\ 으로 (escape 문자 인식 방지)
```javascript    
  "ConnectionStrings": {
    "LocalDb": "Server=(localdb)\\mssqllocaldb;Database=NoteDb;",
    "DevDb:":"Server=(localdb)\\mssqllocaldb;Database=NoteDevDb;",
    "ProductionDb": "Server=(localdb)\\mssqllocaldb;Database=NoteProductionDb;"
  }, 
```   

startup.cs    
```c#
public void ConfigureServices(IServiceCollection services)
{
    //MVC 뿐만 아니라, 타클래스 라이브리에서도 사용하도록 하는 기능 (순서는 여기에, 아래에서 접근하기 전에1!)
    services.AddSingleton<IConfiguration>(Configuration);   
    
    services.AddTransient<NoticeBll>(); //DI
    services.AddTransient<INoticeDal, NoticeDal>(); //DI
    
    // Add framework services.
    services.AddMvc();    
}
```    
    
```c#    
public class NoteDbContext : DbContext
{
    private readonly IConfiguration  _configuration;

    public NoteDbContext(IConfiguration configuration)
    {
        _configuration = configuration;
    }

    public DbSet<Notice> Notices { get; set; }  //테이블 등록 (code first)

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseSqlServer(_configuration.GetConnectionString("LocalDb"));
    }
}    
```    

NoticeDal.cs 수정    
```c#
private readonly IConfiguration _configuration;
public NoticeDal(IConfiguration configuration)
{
    _configuration = configuration;
}

public List<Notice> GetNoticeList()
{ 
    using (var db = new NoteDbContext(_configuration))
    {
        return db.Notices
            .OrderByDescending(n => n.NoticeNo)
            .ToList();
    }
}
```
