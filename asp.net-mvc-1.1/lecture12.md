## 1. Controllers > NoteController.cs 추가  
```c#
public class NoteController : Controller
{
    public IActionResult Index()
    {            
        using (var db = new AspnetNoteDbContext())
        {
            var list = db.Notes.ToList();   
            return View(list);
        }            
    }

    public IActionResult Add()
    {
        return View();
    }

    public IActionResult Edit()
    {
        return View();
    }

    public IActionResult Delete()
    {
        return View();
    }
}
```

## 2. Views > Note > Index.cshtml 추가
```C#
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
            <td>@note.NoteTitle</td>
            <td>@note.UserNo</td>            
        </tr>
        }
    </tbody>
</table>

<a class="btn btn-warning" asp-controller="Note" asp-action="Add">게시물 작성</a>
```

## 3. Views > Note > Add.cshtml 추가

## 4. Views > Note > Edit.cshtml 추가

## 5. _Layout.cshtml 수정
```c#
<div class="navbar-collapse collapse">
    <ul class="nav navbar-nav">
        <li><a asp-area="" asp-controller="Note" asp-action="Index">게시판</a></li>   /* 여기 수정 */
    </ul>
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
</div>
```

## 6. site.css 수정 (게시판 padding)
```c#
body {
    padding-top: 80px;
    padding-bottom: 20px;
}

/* Wrapping element */
/* Set some basic padding to keep content from hitting the edges */
.body-content {
    padding-top:20px;
    padding-left: 15px;
    padding-right: 15px;
}
```
