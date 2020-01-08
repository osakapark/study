# 0. 이론
- 의존성 주입 (Dependency Injection)
  - 프로그래밍에서 구성요소간의 의존 관계가 소스코드 내부가 아닌 외부의 설정파일 등을 통해 정의되게 하는 디자인 패턴
- IOC (Inverson of Control)
  - 프로그래머가 작성한 프로그램이 재사용 라이브러리의 흐름 제어를 받게 되는 소프트웨어 디자인 패턴을 말한다.  
- Ioc Container
   - 객체를 프레임워크에서 간접적으로 생성, 소멸 시켜주는 컨테이너를 뜻함

# 1. Hello 빈 솔루션 만들기
  - 새프로젝트 > 기타 프로젝트 형식 > Visual Studio 솔루션 > 빈솔루션
  
# 2.가상의 솔루션 폴더 생성
  - BusinessLogicLayer
  - DataAccessLayer
  - Common
  - PresentationLayer

# 3. 프로젝트 생성
### 1  Class Libary 프로젝트 생성 (.Net Core > 클래스 라이브러리 (.NET Core)
  - BusinessLogicLayer
    - Hello.BLL
  - Common
    - Hello.Model
  - DataAccessLayer
    - Hello.IDAL
    - Hello.MSSQL.DAL
    - Hello.Oracle.DAL    

### 2. 웹 응용프로그램 생성
  - PresentationLayer
    - Hello.MVC6
    - .NET Core > ASP.NET Core 웹 응용프로그램 > 웹 응용 프로그램 (.NET Core, ASP.NET Core 1.1 선택)
 
# 4. 소스코드
Hello.BLL > UserBll
```C#
public class UserBll
{
    private readonly IUserDal _userDal; //반드시 Interface

    public UserBll(IUserDal userDal)
    {
        _userDal = userDal;
    }

    public List<User> GetUserList()
    {
        return _userDal.GetUserList();
    }

    public User GetUser(int userNo)
    {
        if (userNo <= 0)
        {
            throw new ArgumentException();
        }
        return _userDal.GetUser(userNo);
    }

    public bool SaveUser(User user)
    {
        if (user == null)
        {
            throw new ArgumentException();
        }
        return _userDal.SaveUser(user);
    }
}
```

Hello.Model > User.cs
```C#
public class User
{
    public int UserNo { get; set; }
    public string UserName { get; set; }
}
```
Hello.IDAL
```C#
public interface IUserDal
{
    List<User> GetUserList();
    User GetUser(int userNo);
    bool SaveUser(User user);
}
```
Hello.MSSQL.DAL
```C#
public class UserDal : IUserDal
{
    public User GetUser(int userNo)
    {
        throw new NotImplementedException();
    }

    public List<User> GetUserList()
    {
        throw new NotImplementedException();
    }

    public bool SaveUser(User user)
    {
        throw new NotImplementedException();
    }
}
```
Hello.Oracle.DAL
```C#
public class UserDal : IUserDal
{
    public User GetUser(int userNo)
    {
        throw new NotImplementedException();
    }

    public List<User> GetUserList()
    {
        throw new NotImplementedException();
    }

    public bool SaveUser(User user)
    {
        throw new NotImplementedException();
    }
}
```

HomeController (수정)
```C#
private readonly UserBll _userBll;
public HomeController(UserBll userBll)
{
    _userBll = userBll;
}

public IActionResult Index()
{
    var userList = _userBll.GetUserList();
    return View();
}
```

Startup.cs 수정
```C#
public void ConfigureServices(IServiceCollection services)
{
    // Add framework services.
    services.AddMvc();

    //MVC 6 - 의존성 주입을 할 수 있는 3가지
    // 1. AddSingleton<T>();
    //   - 웹사이트가 시작하면 웹사이트가 종료될 때까지 메모리 상에 유지 되는 객체 주입
    //   1번 사용자 -> UserBll (1번 사용자 session 끝날 때 까지 계속 유지)  ==> 맞나?
    // 2. AddScoped<T>();
    //  - 웹사이트가 시작되어 1 번의 요청이 있을 때 메모리 상에 유지되는 객제 주입
    // 3. AddTransient<T>();
    //  - 웹사이트가 시작되어 각 요청마다 새롭게 생성되는 객체 주입
    services.AddTransient<UserBll>();
    services.AddTransient<IUserDal, UserDal>();
}
```
