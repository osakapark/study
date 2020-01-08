
## 1. trumbowyg 설치 
  종속성 > Bower > Bower 패키지 관리 > 찾아보기 > trumbowyg
  (A lightweight WYSIWYG editor 설치)
  아래 버전 맞출 것
```c#
프로젝트 폴더 > bower.json  
{
  "name": "asp.net",
  "private": true,
  "dependencies": {
    "bootstrap": "3.3.7",
    "jquery": "2.2.0",
    "jquery-validation": "1.14.0",
    "jquery-validation-unobtrusive": "3.2.6",
    "trumbowyg": "v2.21.0"
  }
}
```    
 
## 2 . _Layout.cshtml 수정
```c#
<environment names="Development">
    <link rel="stylesheet" href="~/lib/bootstrap/dist/css/bootstrap.css" />
    <link rel="stylesheet" href="~/lib/trumbowyg/dist/ui/trumbowyg.min.css" />  /* 추가 */
    <link rel="stylesheet" href="~/css/site.css" />
</environment>
  
  
 <environment names="Development">
    <script src="~/lib/jquery/dist/jquery.js"></script>
    <script src="~/lib/bootstrap/dist/js/bootstrap.js"></script>
    <script src="~/lib/trumbowyg/dist/trumbowyg.min.js"></script>    /* 추가 */
    <script src="~/lib/trumbowyg/dist/langs/ko.min.js"></script>     /* 추가 */
    <script src="~/js/site.js" asp-append-version="true"></script>
</environment> 
```
순서는 그대로 할 것! 
1. jquery
2. bootstrap(UI 관련)
3. trumbowyg (jquery 를 참조)


## 3. Add.cshtml 수정
```c#
@model AspnetNote.MVC6.Models.Note

<div class="row">
    <div class="cal-lg-2"></div>
    <div class="cal-lg-8">
        <form class="form-horizontal" method="post" asp-controller="Note" , asp-action="Add">
            <div class="text-danger" asp-validation-summary="ModelOnly"></div>
            <div class="form-group">
                <label>제목</label>
                <input type="text" class="form-control" asp-for="NoteTitle" placeholder="제목 입력" />
                <span class="text-danger" asp-validation-for="NoteTitle"></span>
            </div>
            <div class="form-group">
                <textarea class="form-control editor" rows="20" asp-for="NoteContents" placeholder="내용 입력"></textarea>
                <span class="text-danger" asp-validation-for="NoteContents"></span>
            </div>
            <div class="form-group">
                <button tyep="submit" class="btn btn-primary">저장</button>
                <a class="btn btn-default" asp-controller="Note" asp-action="Index">취소</a>
            </div>
        </form>
    </div>
    <div class="cal-lg-2"></div>
</div>
```
editor class 추가 (trumbowyg editor 반영)

## 4. site.js 수정
```c#
$(".editor").trumbowyg({
    lang: 'ko'
});   //웹사이트가 시작함과 동시에 'editor' class 는 trumbowyg 적용하라!
```

## 5. Detail.cshtml 수정
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
                    @Html.Raw(@Model.NoteContents)                    
                </div>
                <div class="panel-footer text-right" >
                    <a class="btn btn-primary" asp-controller="Note" asp-action="Index">목록</a>
                </div>
            </div>
        </div>
    </div>
</div>
```
입력항목 html 로 표시

## 6. 참고  
NPM = Node Package Manger 
 (Node.js에서 왔다)

CLI (Command Line Interface)
