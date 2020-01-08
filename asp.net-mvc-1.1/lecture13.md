# 13. 게시물 추가하기 

## 1. Controllers > NoteController.cs 수정  
```c#
public class NoteController : Controller
{
    public IActionResult Index()
    {
        if(HttpContext.Session.GetInt32("USER_LOGIN_KEY") == null)
        {
            //로그인이 안된 상태
            return RedirectToAction("Login", "Account");
        }
        using (var db = new AspnetNoteDbContext())
        {
            var list = db.Notes.ToList();   
            return View(list);
        }            
    }

    public IActionResult Add()
    {
        if (HttpContext.Session.GetInt32("USER_LOGIN_KEY") == null)
        {
            //로그인이 안된 상태
            return RedirectToAction("Login", "Account");
        }
        return View();
    }

    [HttpPost]
    public IActionResult Add(Note model)
    {
        if (HttpContext.Session.GetInt32("USER_LOGIN_KEY") == null)
        {
            //로그인이 안된 상태
            return RedirectToAction("Login", "Account");
        }
        model.UserNo = int.Parse(HttpContext.Session.GetInt32("USER_LOGIN_KEY").ToString());

        if (ModelState.IsValid)
        {
            using (var db = new AspnetNoteDbContext())
            {
                db.Notes.Add(model);

                if (db.SaveChanges() > 0)   //commit (성공 갯수 반환)                   
                {
                    return Redirect("Index"); //동일 컨트롤러 이동 (RedirectToAction는 다른 controller)
                }                    
            }
            ModelState.AddModelError(string.Empty, "게시물을 저장할 수 없습니다."); //전역적 오류 메시지
        }
        return View(model);
    }

    public IActionResult Edit()
    {
        if (HttpContext.Session.GetInt32("USER_LOGIN_KEY") == null)
        {
            //로그인이 안된 상태
            return RedirectToAction("Login", "Account");
        }
        return View();
    }

    public IActionResult Delete()
    {
        if (HttpContext.Session.GetInt32("USER_LOGIN_KEY") == null)
        {
            //로그인이 안된 상태
            return RedirectToAction("Login", "Account");
        }
        return View();
    }
}
```

## 2. Views > Note > Add.cshtml 수정

```c#
@model AspnetNote.MVC6.Models.Note

<div class="row">
    <div class="cal-lg-4"></div>
    <div class="cal-lg-4">
        <form class="form-horizontal" method="post" asp-controller="Note" , asp-action="Add">
            <div class="text-danger" asp-validation-summary="ModelOnly"></div>
            <div class="form-group">
                <label>제목</label>
                <input type="text" class="form-control" asp-for="NoteTitle" placeholder="제목 입력" />
                <span class="text-danger" asp-validation-for="NoteTitle"></span>
            </div>
            <div class="form-group">
                <textarea class="form-control" rows="20" asp-for="NoteContents" placeholder="내용 입력"></textarea>
                <span class="text-danger" asp-validation-for="NoteContents"></span>
            </div>
            <div class="form-group">
                <button tyep="submit" class="btn btn-primary">저장</button>
                <a class="btn btn-default" asp-controller="Note" asp-action="Index">취소</a>
            </div>
        </form>
    </div>
    <div class="cal-lg-4"></div>
</div>
```

## 3. Models > Note.cs 수정
```c#            
public class Note
{
    [Key] //PK 설정
    public int NoteNo { get; set; }

    [Required(ErrorMessage = "제목을 입력하세요.")]
    public string NoteTitle { get; set; }

    [Required(ErrorMessage = "내용을 입력하세요.")]
    public string NoteContents { get; set; }

    [Required]
    public int UserNo { get; set; }

    [ForeignKey("UserNo")]
    public virtual User User { get; set; }
}
```

## 4. Controllers > AccountController (확인)
```c#
public class AccountController : Controller
{ 
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
        return View(model); //화면에서 validion 체크
    }

    public IActionResult Logout()
    {
        //HttpContext.Session.Clear();    //모든 세선 clear (관리자들이 사용)
        HttpContext.Session.Remove("USER_LOGIN_KEY");
        return RedirectToAction("Index", "Home");
    }

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
