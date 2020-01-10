#부트스트랩 form 

```html
<body>
    <h1 class="page-header">회원가입</h1>
    <form>
        <div class="form-group">
            <label>이름</label>
            <input type="text" class="form-control" />
        </div>
        <div class="form-group">
            <label>ID</label>
            <input type="text" class="form-control" />
        </div>
        <div class="form-group">
            <label>비밀번호</label>
            <input type="password" class="form-control" />
        </div>
        <div class="form-group">
            <label>사진</label>
            <input type="file" class="form-control" />
        </div>
    </form>
    <script src="js/jquery-3.4.1.js"></script>
    <script src="js/bootstrap.js"></script>
</body>
```

```html
<body>
    <form class="form-inline">
        <div class="form-group">
            <label for="exampleInputName2">Name</label>
            <input type="text" class="form-control" id="exampleInputName2" placeholder="Jane Doe">
        </div>
        <div class="form-group">
            <label for="exampleInputEmail2">Email</label>
            <input type="email" class="form-control" id="exampleInputEmail2" placeholder="jane.doe@example.com">
        </div>
        <button type="submit" class="btn btn-default">Send invitation</button>
    </form>
</body>
```

```html
<body>
    <div class="container">
        <form class="form-horizontal">
            <div class="form-group">
                <div class="input-group">
                    <div class="input-group-addon">
                        가격
                    </div>
                    <input type="number" class="form-control" />
                    <div class="input-group-btn">
                        <button type="button" class="btn btn-primary">구입</button>
                    </div>
                </div>
            </div>
        </form>
    </div>
</body>
```

horizontal 예제
```html
<body>
    <div class="container">
        <form class="form-horizontal">
            <div class="form-group">
                <label for="inputEmail3" class="col-sm-2 control-label">Email</label>
                <div class="col-sm-10">
                    <input type="email" class="form-control" id="inputEmail3" placeholder="Email">
                </div>
            </div>
            <div class="form-group">
                <label for="inputPassword3" class="col-sm-2 control-label">Password</label>
                <div class="col-sm-10">
                    <input type="password" class="form-control" id="inputPassword3" placeholder="Password">
                </div>
            </div>
            <div class="form-group">
                <div class="col-sm-offset-2 col-sm-10">
                    <div class="checkbox">
                        <label>
                            <input type="checkbox"> Remember me
                        </label>
                    </div>
                </div>
            </div>
            <div class="form-group">
                <div class="col-sm-offset-2 col-sm-10">
                    <button type="submit" class="btn btn-default">Sign in</button>
                </div>
            </div>
        </form>
    </div>
</body>

```
textarea
```html
<body>
    <div class="container">
        <textarea class="form-control" rows="3"></textarea>
    </div>
</body>

```
