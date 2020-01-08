# 14. 게시물 상세보기 페이지 만들기

## 1. NoteController 수정
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

    public IActionResult Detail(int noteNo)
    {
        if (HttpContext.Session.GetInt32("USER_LOGIN_KEY") == null)
        {
            //로그인이 안된 상태
            return RedirectToAction("Login", "Account");
        }
        using (var db = new AspnetNoteDbContext())
        {
            var note = db.Notes.FirstOrDefault(n => n.NoteNo.Equals(noteNo));
            return View(note);
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
- public IActionResult Detail(int noteNo) 추가

## 2. Detail.cshtml 수정
```c#
<div class="row">
    <div class="col-lg-12">
        <h3 class="page-header">상세읽기 - @Model.NoteTitle</h3>
    </div>

    <div class="row">
        <div class="col-lg-10">
            <div class="panel panel-default">
                <div class="Panel-heading">
                    <label>제목 : </label>@Model.NoteTitle
                </div>
                <div class="Panel-body">
                    @Model.NoteContents
                </div>
                <div class="panel-footer text-right" >
                    <a class="btn btn-primary" asp-controller="Note" asp-action="Index">목록</a>
                </div>
            </div>
        </div>
    </div>
</div>
```

- asp-route-noteNo 
<br> NoteController 의 파라미터 명  맞아야 함 (대소문자는 안가리지만, 가능하면 맞추어 주자!)
<br> public IActionResult Detail(int noteNo)

## 3. Views > Note > Index.cshtml 수정
```c#
<table class="table table-bordered">
    <thead>
        <tr>
            <th>번호</th>
            <th>제목</th>
            <th>작성자</th>
        </tr>
    </thead>

    <tbody>
        @foreach(var note in Model)
        {
        <tr>
            <td>@note.NoteNo</td>
            <td>
                @*http://www.example.com/*@
                @*http://www.example.com/{noteNo}*@                
                @*http://www.example.com/Note/Detail?noteNo=1*@
                <a asp-controller="Note" asp-action="Detail" asp-route-noteNo="@note.NoteNo">@note.NoteTitle</a>                
            </td>
            <td>@note.UserNo</td>            
        </tr>
        }
    </tbody>
</table>

<a class="btn btn-warning" asp-controller="Note" asp-action="Add">게시물 작성</a>
```
a link 추가
