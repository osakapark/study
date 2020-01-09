# Glyphicon과 Meterial Icon 사용하기

##1. glyphicon
https://getbootstrap.com/docs/3.4/components/#glyphicons
<br> bootstrap font folder도 있어야 함

## 2. material icon (구글에서 만든 것)
https://material.io/resources/icons/?style=baseline


```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewPoint content=" width-device-width, initial-scale=1">
    <title>부트스트랩 공부</title>
    <link rel="stylesheet" href="css/bootstrap.css">
    <link rel="stylesheet" href="https://fonts.googleapis.com/icon?family=Material+Icons">
</head>

<body>

    <div class="text-center" style="margin-top: 20px">
        <button type="button" class="btn btn-default">
            <span class="glyphicon glyphicon-ok"></span> 버튼
            <span class="glyphicon glyphicon-ok" style="color:orange ; font-size: 100px"></span>
            <i class="material-icons" style="color:orange ; font-size: 100px">
                fingerprint
            </i>
        </button>
    </div>
    <script src="js/jquery-3.4.1.js"></script>
    <script src="js/jquery-3.4.1.js"></script>
</body>

</html>
```
