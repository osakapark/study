# 16. 텍스트 에디터 이미지 업로드 

## 1. UploadController 생성
```c#
public class UploadController : Controller
{
    private readonly IHostingEnvironment _environment;  //환경추적 매개체

    public UploadController(IHostingEnvironment environment)
    {
        _environment = environment;
    }

    //http://example.com/Upload/Image/ImageUpload   (real 주소)
    //http://example.com/Upload/api/upload    (route 로 재정의 주소)
    [HttpPost, Route("api/upload")]  //Image는 HttpPost 사용
    public async Task<IActionResult> ImageUpload(IFormFile file)
    {
        // # 이미지나 파일을 업로드할 때 필요한 구성
        // 1.Path(경로) - 어디에 저장할지 결정
        var path = Path.Combine(_environment.WebRootPath, @"images\upload"); //escape sequence 인식 방지
        // 2.Name(이름) - DateTime,GUID + GUID            
        // 3. Extension(확장자) - jpg, png, txt, ...
        var fileFullName = file.FileName.Split('.');
        var fileName = $"{Guid.NewGuid()}.{fileFullName[1]}";
        using (var fileStream = new FileStream(Path.Combine(path, fileName), FileMode.Create))
        {
            await file.CopyToAsync(fileStream); //async, await 는 짝
        }
        return Ok(new { file = "/images/upload/" + fileName, success = true }); //file 의 path 앞에 '/' 반드시.. (자바스크립트에서 사용!)

        // # URL 접근 방싱
        // ASP.NET - 호스트명/ + api/upload
        // JavaScript - 호스트명 + api/upload => http://www.example.comapi/upload
        // JavaScript - 호스트명 + "/" + api/upload => http://www.example.com/api/upload
    }
}
```
- Route 개념 이해
- ImageUpload(IFormFile file) 의 매개변수명 => trumbowyg.upload.js 의 fileFieldName 과 동일

## 2. _Layout.cshtml 수정
trumbowyg.upload.js 추가
```javascript
<environment names="Development">
    <script src="~/lib/jquery/dist/jquery.js"></script>
    <script src="~/lib/bootstrap/dist/js/bootstrap.js"></script>
    <script src="~/lib/trumbowyg/dist/trumbowyg.min.js"></script>
    <script src="~/lib/trumbowyg/dist/plugins/upload/trumbowyg.upload.js"></script>
    <script src="~/lib/trumbowyg/dist/langs/ko.min.js"></script>
    <script src="~/js/site.js" asp-append-version="true"></script>
</environment>
```

## 3. trumbowyg.upload.js 수정
- serverPath : '/' 필수
- fileFieldName : ImageUpload(IFormFile file) 의 매개면수 명과 동일해야함
```javascript
var defaultOptions = {
    serverPath: '/api/upload',      //반드시 제일 앞에 '/' 추가
    fileFieldName: 'file',          // controller 의 파라미터 이름과 동일 해야 함 => public IActionResult ImageUpload(IFormFile file)
    data: [],                       // Additional data for ajax [{name: 'key', value: 'value'}]
    headers: {},                    // Additional headers
    xhrFields: {},                  // Additional fields
    urlPropertyName: 'file',        // How to get url from the json response (for instance 'url' for {url: ....})
    statusPropertyName: 'success',  // How to get status from the json response 
    success: undefined,             // Success callback: function (data, trumbowyg, $modal, values) {}
    error: undefined,               // Error callback: function () {}
    imageWidthModalEdit: false      // Add ability to edit image width
};
```


## 4. site.js 수정
```javascript
$(".editor").trumbowyg({
    lang: 'ko',
    btnsDef: {
        image: {
            dropdown: ['insertImage', 'upload'],  //정의된 내용 (마음대로 바꾸면 안됨)
            ico:'insertImage'
        }
    },
    btns: [
        ['viewHTML'],
        ['undo', 'redo'], // Only supported in Blink browsers
        ['formatting'],
        ['strong', 'em', 'del'],
        ['superscript', 'subscript'],
        ['link'],
        ['insertImage'],
        'image',
        ['justifyLeft', 'justifyCenter', 'justifyRight', 'justifyFull'],
        ['unorderedList', 'orderedList'],
        ['horizontalRule'],
        ['removeformat'],
        ['fullscreen']
    ]
});   
//웹사이트가 시작함과 동시에 'editor' class 는 trumbowyg 적용하라!
```
btns 항목 참조 site : https://alex-d.github.io/Trumbowyg/ > Documentation > Button pane
