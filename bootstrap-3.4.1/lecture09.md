 # container와 grid system
- container : 여백은 가득체우나, 내용물은 가운데로
- container-fluid  : 여백을 가득 채움
- row 의 합은 12가 되어야 함
  - col-md-x  잘 모르면 md 써라
  - col-xs-x
  
```html
<body>
    <div class="container">
        <div class="row">
            <div class="col-md-3 col-lg-3 col-sm-3">

                <ul class="list-group">
                    <li class="list-group-item">홈</li>
                    <li class="list-group-item">프로그래밍</li>
                    <li class="list-group-item">취업</li>
                    <li class="list-group-item">블로그</li>
                </ul>
            </div>
            <div class="col-md-9 col-lg-9 col-sm-9">
                <div class="panel panel-primary">
                    <div class="panel-heading">
                        <hi>안녕</hi>
                    </div>
                    <div class="panel-body">
                        <p>부트스트랩은 참 쉽고 재미있다</p>
                    </div>
                </div>
            </div>
        </div>

    </div>

    <script src="js/jquery-3.4.1.js"></script>
    <script src="js/jquery-3.4.1.js"></script>
</body>

```  
