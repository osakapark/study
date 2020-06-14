# Callback
callback 제어권을 넘긴다.

1.다른함수 (A)의 인자로 콜백함수 (B)를 전달하면
A가 B의 제어권을 갖게 된다.

2.A에 미리 정해놓은 방식에 따라 B를 호출한다.

3.미리정해 놓은 방식이란
 - 어떤 시점에 콜백을 호출할지
 - 인자에는 어떤 값을 지정할지
 - this에는 무엇을 바인딩할지 등




## 1.실행시점제어권
```
//주기함수 호출
setInterval(function() {
	console.log('every one second ')
}, 1000);
```

```
var cb = function () {
	console.log('every one second ')
};
setInterval(cb, 1000);
```

## 2. 인자
```
var arr = [1,2,3,4,5];
var  entries = [];
arr.forEach(function (v,i) {
	entries.push([i,v,this[i]]);
}, [10,20,30,40,50]);
console.log(entries);

//이건 오류 (forEach 정의를 따라야만 함..)
var arr = [1,2,3,4,5];
var  entries = [];
arr.forEach(function (index,value) {
	console.log(index, value);
}, [10,20,30,40,50]);
```


## 3.this
```
// 이벤트객체가 callback 함수의 첫번째 인자로 무조건 들어 간다..

document.body.innerHTML += '<div id="a">abc</div>';
function cbFunc(x) {
	console.log(this, x);
}
document.getElementById('a')
	.addEventListener('click', cbFunc);

$('#a').on('click', cbFunc);
```

## 주의사항
```		
var arr = [1,2,3,4,5];
var obj = {
	vals : [1,2,3],
	logValues : function (v,i) {
		if(this.vals) {
			console.log('aaaa');
			console.log(this.vals, v,i);
		} else {
			console.log('bbbb');
			console.log(this,v,i);
		}
	}
};

obj.logValues(1,2); //메소드로 전달
arr.forEach(obj.logValues);	//콜백함수로 전달
arr.forEach(obj.logValues.bind(obj));	//콜백함수로 전달
arr.forEach(obj.logValues, obj);	//콜백함수로 전달		
```
