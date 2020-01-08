## 1.NuGet 추가
  Microsoft.AspNetCore.Session  (1.X대 마지막으로 설치)
  
## 2.AccountController 수정
```C# 
[HttpPost]
public IActionResult Login(LoginViewModel model) //파라미터는 model 이름 권장
{
    if (ModelState.IsValid)
    {
        using (var db = new AspnetNoteDbContext())
        {
            //Linq - 메서드 체이닝
            // => : A Go to B
            //== 메모리 누수                    
            var user = db.Users
                .FirstOrDefault(u => u.UserId.Equals(model.UserId) &&
                                u.UserPassword.Equals(model.UserPassword));
            if (user != null)
            {
                //로그인 성공
                HttpContext.Session.SetInt32("USER_LOGIN_KEY", user.UserNo);
                return RedirectToAction("LoginSuccess", "Home"); //성공페이지로 이동
            }
        }
        //로그인 실패 (using 안의 코드는 최소화!)              
        ModelState.AddModelError(string.Empty, "사용자 ID 혹은 비밀번호가 올바르지 않습니다.");
    }
    return View(model); //화면에서 validation 체크
}

public IActionResult Logout()
{
    //HttpContext.Session.Clear();    //모든 세선 clear (관리자들이 사용)
    HttpContext.Session.Remove("USER_LOGIN_KEY");

    return RedirectToAction("Index", "Home");
}
```

## 3.Startup.cs 수정
```c#
public void ConfigureServices(IServiceCollection services)
{
    // Add framework services.
    services.AddMvc();

    //DI 의존성 주입 - ASP.NET MVC4, 5 + Unity(게임 X)
    //Session - 서비스에 등록 함
    services.AddSession();
    //Identity
    //Web API 관련 기능
}

// This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
{
    loggerFactory.AddConsole(Configuration.GetSection("Logging"));
    loggerFactory.AddDebug();

    if (env.IsDevelopment())
    {
        app.UseBrowserLink();
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseExceptionHandler("/Home/Error");
    }

    app.UseStaticFiles();
    app.UseSession();  //Application 에서 사용하겠다. (이것 추가)
    app.UseMvc(routes =>
    {
        routes.MapRoute(
            name: "default",
            template: "{controller=Home}/{action=Index}/{id?}");
    });
}
```

## 4._Layout.cshtml
```c#
<ul class="nav navbar-nav navbar-right">
    @if (Context.Session.GetInt32("USER_LOGIN_KEY") == null)
    {
        <li><a asp-area="" asp-controller="Account" asp-action="Register">회원가입</a></li>
        <li><a asp-area="" asp-controller="Account" asp-action="Login">로그인</a></li>
    }
    else
    {
        <li><a asp-area="" asp-controller="Account" asp-action="Logout">로그아웃</a></li>
    }
</ul>
```
GetInt32()  : int? => 없으면 null 이라는 의미

## 5.참고
- ASP.NET MVC -> IIS(웹서버) = 자바의 아파치
  - WebAPI 압축 
- ASP.NET CORE -> 리눅스, 윈도우, MacOS 모두 동작 필요
  -	IIS에 종속적이면 안됨
  -	Middle ware로 구축 
