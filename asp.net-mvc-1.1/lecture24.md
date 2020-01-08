# ASP.NET Core Identity 모델 클래스 설명

## 1. Vie Model 설명
 Note 솔루션에서 
### 1.1 AccountController추가 
추가 > 새항목 추가 > ASP.NET Core > 컨트롤로 클래스

### 1.2 NoteDbContext.cs
별도의 [Required] 가 없는 ViewModel 을 쓰지 않으려면 아래의 Method 사용하면 되나 어렵다고 함 (직관적이지 않아서 권장도 안하고)
```c#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);
}
```
### 1.3 ViewModel
Note.ViewModel (View Model 전용 프로젝트 생성)
<br> Common > 새 프로젝트 추가 > .Net Core > 클래스 라이브러리 (.NET Core) > Note.ViewModel


<br> 이하 IdentityTest Project

## 2.  _LoginPartial
 - _Layout.cshtml 에서 _LoginPartial 읽음 (소스 가독성 위함)
 <br> @await Html.PartialAsync("_LoginPartial")

-  Model 이름을 Role.cs, User.cs로 바꾸었다면.. (안바꾸면 실행 시 오류)
<br> @inject SignInManager<User> SignInManager
<br> @inject UserManager<User> UserManager

 
## 3. Display-Name
Register.cshtml 의 Label로 붙음
```c#
RegisterViewModel.cs
[Display(Name = "Email")]
public string Email { get; set; }

Register.cshtml
<label asp-for="Email" class="col-md-2 control-label"></label>
```

{0] = DisplayName, 
<br> StringLength 항목 위치 조심할 것!!
<br> [Reruired] 이외에는 잘 안씀 (서버 자원 이용하는 것은 옳은 방법이 아님, validation 은 javascript 로!!)
```c#
[Required]
[StringLength(100, ErrorMessage = "The {0} must be at least {2} and at max {1} characters long.", MinimumLength = 6)]
[DataType(DataType.Password)]
[Display(Name = "Password")]
public string Password { get; set; }
```
