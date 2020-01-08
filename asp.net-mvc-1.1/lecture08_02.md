# 1. Controller Class 생성
Controllers > AccountController.cs

```C#
public class AccountController : Controller
{
    /// <summary>        
    /// 로그인
    /// </summary>
    /// <returns></returns>
    [HttpGet]
    public IActionResult Login()
    {
        return View();
    }

    /// <summary>
    /// 회원가입
    /// </summary>
    /// <returns></returns>
    public IActionResult Register()
    {
        return View();
    }

    [HttpPost]
    public IActionResult Register(User model)
    {
        if (ModelState.IsValid) //필수 입력값 존재 여부 확인
        {
            using (var db = new AspnetNoteDbContext())  //db open,close
            {
                db.Users.Add(model); //메모리상까지만
                db.SaveChanges();    //Commit
            }
            return RedirectToAction("Index", "Home");
        }
        return View(model);
    }
}
```
- [HttpGet] 은 기본 값 (없어도 됨)


# 2.View 생성
Views > Account (폴더 이름에 controller 접미사는 빼라!)
<br> (Razor View Page)

```C#
@model  AspnetNote.MVC6.Models.User 

<h2>회원가입</h2>

<form class="form-horizontal" method="post" asp-controller="Account" asp-action="Register">
    <div class="form-group">
        <label>사용자 ID</label>
        <input type="text" class="form-control" asp-for="UserId" placeholder="사용자 ID를 입력" />
        <span class="text-danger"  asp-validation-for="UserId"></span>
    </div>

    <div class="form-group">
        <label>사용자 비밀번호</label>
        <input type="password" class="form-control" asp-for="UserPassword" placeholder="사용자 비밀번호 입력" />
        <span class="text-danger" asp-validation-for="UserPassword"></span>
    </div>

    <div class="form-group">
        <label>사용자 이름</label>
        <input type="text" class="form-control" asp-for="UserName" placeholder="사용자 이름 입력" />
        <span class="text-danger" asp-validation-for="UserName"></span>
    </div>

    <divl class="form-group">
        <button type="submit" class="btn btn-primary">회원가입</button>
        <a class="btn btn-warning" href="#">취소</a>
    </divl>
</form>
```

- @model  AspnetNote.MVC6.Models.User 
  asp-for 등에서 Model 의 속성 참고 목적
- asp-validation-for
  개별 필수 값 입력 여부 검증
- asp-validation-summary 
  전체 확인 메시지
- 이 소스는 service side validation (client side는 javascript, jquery 로)
- bootstrap 
  <br> form-control, btn-primary(파란색), btn-warning(노란색)
  
- form 의 textbox width 변경
  wwwroot > css > site.css 
```css
  /* Set widths on the form inputs since otherwise they're 100% wide */
input,
select,
textarea {
    max-width: 400px;
}
```

변경 화면 반영 안되면
  <br> 크롬 오른쪽 마우스 > 검사 
  <br> 크롬 정보 초기화  (설정 > 고급 > 인터넷 사용기록 삭제), 크롬 캐시 기능 때문에

