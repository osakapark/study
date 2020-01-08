# 25. AccountController 소스코드 분석하기

## 1. Account Controller
웹 사이트의 로그인, 로그아웃, 회원가입 등 인증관련 액션 메소들의 모입
- [Authorize] ==> 인증 된 사용자만 접근가능
- [AllowAnonymous] ==> 익명접근 가능
- [ValidateAntiForgeryToken] => csrf  방지
 <br> (참고 site)  
   http://egocube.pe.kr/translation/content/asp-net-web-api/201402030001
- [Authorize, AllowAnonymous] => 한줄표현 가능
- returnUrl : 사용자가 로그인/회원가입 후 기존에 보던 페이지로 바로 이동 할 때 사용 => 중급개발자라면!!

## 2. 전역변수 설명
- 로그인한 사용자 정보 관리 - 쿠키 (사용자 번호, 사용자 이름)
<br>private readonly UserManager<User> _userManager;

- 로그인 관리
<br> private readonly SignInManager<User> _signInManager;

## 3. returnUrl = null
```c#
public async Task<IActionResult> Login(string returnUrl = null)
{
    //Account/Login  => returnUrl = "" : (returnUrl = null )  => null로 처리

    //기존 쿠키 ExpireTimeout
    //쿠키 생성시에 Timeout -> 쿠키 생성된 이후에 특정시간이 지나면 사용 불가

    // Clear the existing external cookie to ensure a clean login process
    await HttpContext.Authentication.SignOutAsync(_externalCookieScheme);

    ViewData["ReturnUrl"] = returnUrl;
    return View();
}
```

## 4. 생성자 생성 방식
```c#
/*
 * 전통적 방식
User user = new User();
user.UserName = model.Email;
user.PasswordHash = model.Password;
*/                

var user = new User { UserName = model.Email, Email = model.Email };
```

## 5. Logout
```C#
public async Task<IActionResult> Logout()
{
    await _signInManager.SignOutAsync();    //로그아웃 -> 쿠키존재 (유명무실)
    _logger.LogInformation(4, "User logged out.");

    //기존 
    //return RedirectToAction("Index", "Home");

    //오타방지
    return RedirectToAction(nameof(HomeController.Index), "Home");
}
```
## 6.시큐어 코딩
```c#
private IActionResult RedirectToLocal(string returnUrl)
{
    //정확하게 이루어 져아 할 것들. if() - 개발자가 의도한 것들
    //1.명시되어 있는 것들은 합버으로 하되, 명시 되어 있지 않은 것들은 불법입니다. (여기선 이거..)
    //2.명시가 되어 있지 않는 것은 합법이고, 명시가 된 것들은 불법입니다. 
    if (Url.IsLocalUrl(returnUrl))
    {
        return Redirect(returnUrl);
    }
    else
    {
        //개발자가 의도하지 못하는 것들 (해킹등)
        return RedirectToAction(nameof(HomeController.Index), "Home");
    }
}
```
