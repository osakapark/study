# Bootstrap 네이비게이션 만들기

<nav> 꼭 써야 하나? ==> 안써도 되지만, 검색엔진 최적화에는 필요

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>부트스트랩 공부</title>
  <link rel="stylesheet" href="css/bootstrap.css">
  <link rel="stylesheet" href="https://fonts.googleapis.com/icon?family=Material+Icons">
</head>

<body>
  <div class="container">
    <nav>
      <ul class="nav nav-tabs">
        <li role="presentation" class="active" id="home"><a href="#" onclick="homeClick()">Home</a></li>
        <li role="presentation" id="about"><a href="#" onclick="aboutClick()">About</a></li>
        <li role="presentation"><a href="#">Messages</a></li>
      </ul>
    </nav>
  </div>

  <script src="js/jquery-3.4.1.js"></script>
  <script src="js/bootstrap.js"></script>
  <script type="text/javascript">
    $(document).ready(function () {

    });
    function homeClick() {
      $("#home").addClass("active");
      $("#about").removeClass("active");
    }
    function aboutClick() {
      $("#about").addClass("active");
      $("#home").removeClass("active");
    }
  </script>
</body>

</html>
```

navbar 단순화 1
```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>부트스트랩 공부</title>
  <link rel="stylesheet" href="css/bootstrap.css">
  <link rel="stylesheet" href="https://fonts.googleapis.com/icon?family=Material+Icons">
</head>

<body>
  <div class="container">
    <nav class="navbar navbar-default">
      <ul class="nav navbar-nav">
        <li class="active"><a href="#">메뉴1 <span class="sr-only">(current)</span></a></li>
        <li><a href="#">메뉴2</a></li>
      </ul>

      <ul class="nav navbar-nav navbar-right">
        <li><a href="#">화원가입</a></li>
        <li><a href="#">로그인</a></li>
      </ul>
    </nav>
  </div>
  <script src="js/jquery-3.4.1.js"></script>
  <script src="js/bootstrap.js"></script>
</body>

</html>
```

navbar 단순화 2
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>부트스트랩 공부</title>
    <link rel="stylesheet" href="css/bootstrap.css">
    <link rel="stylesheet" href="https://fonts.googleapis.com/icon?family=Material+Icons">
</head>

<body>
    <div class="container">
        <!-- <nav class="navbar navbar-default"> -->
        <nav class="navbar navbar-inverse">
            <div class="container-fluid">
                <!-- Brand and toggle get grouped for better mobile display -->
                <div class="navbar-header">
                    <button type="button" class="navbar-toggle collapsed" data-toggle="collapse"
                        data-target="#bs-example-navbar-collapse-1" aria-expanded="false">
                        <span class="sr-only">Toggle navigation</span>
                        <span class="icon-bar"></span>
                        <span class="icon-bar"></span>
                        <span class="icon-bar"></span>
                    </button>
                    <a class="navbar-brand" href="#">Brand</a>
                </div>

                <!-- Collect the nav links, forms, and other content for toggling -->
                <div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
                    <ul class="nav navbar-nav">
                        <li class="active"><a href="#">Link <span class="sr-only">(current)</span></a></li>
                        <li><a href="#">Link</a></li>
                    </ul>

                    <ul class="nav navbar-nav navbar-right">
                        <li><a href="#">Link</a></li>
                    </ul>
                </div><!-- /.navbar-collapse -->
            </div><!-- /.container-fluid -->
        </nav>
    </div>
    <script src="js/jquery-3.4.1.js"></script>
    <script src="js/bootstrap.js"></script>
</body>

</html>
```
