#  Identity 생성 테이블 PK 타입 변경하기

## 1. Startup.cs (Identity 사용시 바뀌는 부분)
```C#
public void ConfigureServices(IServiceCollection services)
{
    // Add framework services.
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));
        
    services.AddMvc();

    // Add application services.
    services.AddTransient<IEmailSender, AuthMessageSender>();
    services.AddTransient<ISmsSender, AuthMessageSender>();
}

public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
{
  ...
    app.UseIdentity();
  ... 
}
```

## 2. HomeController.cs
Startup.cs 에서 DbContext 직접 관리 하는 경우 테스트
<br> 다시 강의 해 준다고 하심
```c#
private readonly ApplicationDbContext _dbContext;

public HomeController(ApplicationDbContext dbContext)
{
    _dbContext = dbContext;
}

public IActionResult Index()
{
    var user = _dbContext.Users.FirstOrDefault();
    return View();
}
```

## 3. USer Id 숫자로 바꾸기
ApplicationUser.cs 수정
```c#
public class ApplicationUser : IdentityUser<int>
{

}

```
ApplicationRole.cs 추가
```c#
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;
public class ApplicationRole : IdentityRole<int>
{
}
```

Startup.cs 수정
```c#
public void ConfigureServices(IServiceCollection services)
{
    // Add framework services.
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));

    services.AddIdentity<ApplicationUser, ApplicationRole>()
        .AddEntityFrameworkStores<ApplicationDbContext, int>()
        .AddDefaultTokenProviders();

    services.AddMvc();

    // Add application services.
    services.AddTransient<IEmailSender, AuthMessageSender>();
    services.AddTransient<ISmsSender, AuthMessageSender>();
}
```

ApplicationDbContext 수정
```c# 
//ApplicationRole, int 추가
public class ApplicationDbContext : IdentityDbContext<ApplicationUser, ApplicationRole, int>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }

    protected override void OnModelCreating(ModelBuilder builder)
    {
        base.OnModelCreating(builder);
        // Customize the ASP.NET Identity model and override the defaults if needed.
        // For example, you can rename the ASP.NET Identity table names and more.
        // Add your customizations after calling base.OnModelCreating(builder);
    }
}
```

## 4. DB Clear
 - DB 삭제 (기존 연결 닫기 체크 추가)
 - Migration 폴더 통삭제
 - DB 이름 바꾸기 (appsettings.json)
```javascript
  "ConnectionStrings": {
    "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=IdentityStudyDb;Trusted_Connection=True;MultipleActiveResultSets=true"
  },
``` 
