
## 1. AccountController   수정
Controllers > AccountController.cs

```C#
[HttpGet]
public IActionResult Login()
{
    return View();
}

[HttpPost]
public IActionResult Login(LoginViewModel model) //파라미터는 model 이름 권장
{
    if (ModelState.IsValid)
    {
        using (var db = new AspnetNoteDbContext())
        {
            //Linq - 메서드 체이닝
            // => : A Go to B
            //== 비교는 메모리 누수                    
            var user = db.Users
                .FirstOrDefault(u => u.UserId.Equals(model.UserId) &&
                                u.UserPassword.Equals(model.UserPassword));
            if (user != null)
            {
                //로그인 성공
                return RedirectToAction("LoginSuccess", "Home"); //성공페이지로 이동
            }
        }
        //로그인 실패 (using 안의 코드는 최소화!)              
        ModelState.AddModelError(string.Empty, "사용자 ID 혹은 비밀번호가 올바르지 않습니다.");
    }
    return View(model); //화면에서 validion 체크
}

```
== 비교는 불필요한 string 객체 생성으로 메모리 누수 발생


## 2.View 생성
Views > Account > Login.cshtml

```C#
@model  AspnetNote.MVC6.ViewModel.LoginViewModel

<h2>로그인</h2>

<form class="form-horizontal" method="post" asp-controller="Account" asp-action="Login">
    <div class="text-danger" asp-validation-summary="ModelOnly"></div>

    <div class="form-group">
        <label>사용자 ID</label>
        <input type="text" asp-for="UserId" class="form-control" placeholder="사용자 ID 입력" />
        <span class="text-danger" asp-validation-for="UserId"></span>
    </div>

    <div class="form-group">
        <label>비밀번호</label>
        <input type="password" asp-for="UserPassword" class="form-control" placeholder="비밀번호 입력" />
        <span class="text-danger" asp-validation-for="UserPassword"></span>
    </div>

    <div class="form-group">
        <button type="submit" class="btn btn-success">로그인</button>
    </div>
</form>
```
- asp-validation-summary 
  <br> 로그인 실패시 오류 메시지 표시
   <br> Controller 확인 :   ModelState.AddModelError(string.Empty, "사용자 ID 혹은 비밀번호가 올바르지 않습니다.");
- asp-validation-for
  <br>개별 필수입력 항목 확인
- form-group 
   <br>input text 와 label을 예쁘게 묶음
- form-controller
   <br> 예쁜 테두리 

## 3.HomeController 수정
```c#
public class HomeController : Controller
{
    public IActionResult Index()
    {
        return View();
    }

    public IActionResult LoginSuccess()
    {
        return View();
    }
}
```
LoginSuccess Action 추가

## 4. LoginSuccess.cshtml 생성
```c#
<h1> 로그인에 성공하였습니다.</h1>
```

## 5. LoginViewModel class 생성
ViewModel(폴더 생성) > LoginViewModel.cs
```c#
public class LoginViewModel
{
    [Required(ErrorMessage ="사용자 ID를 입력하세요.")]
    public string UserId { get; set; }

    [Required(ErrorMessage = "사용자 비밀번호를 입력하세요.")]
    public string UserPassword { get; set; }
}
```
로그인에만 사용하는 ViewModel 
<br> 로그인 시 User 사용하면, 불필요한 사용자 이름 등으로 오류 발생

- Model
  <br> 기본 Entkty Class
- View Model 
  <br> View 를 위한 모델
  <br> MVC(Model, View Controller)
  <br> WPF(MVVM) (model, view, view model)

