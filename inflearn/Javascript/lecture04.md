# this 
thisbind 시점 = 실행 context 가활성화 될 때 

## 1.전역공간에서 
window/global
```
function a() {
	console.log(this);
}

a();
```
## 2.함수 호출시 
window/global
(함수는 전역객체의 메소드다 라고 생각하면 편함)

```
function b() {
	function c() {
		console.log(this);
	}
	c();
}
b();
```
```
var d = {
	e: function () {
		function f() {
			console.log(this);
		}
		f();	//이 것만 보면 된다..
	}
}
d.e();
```

```
\\ 내부함수에서 우회법 (ES6에서는 바뀜)
var a = 10;
var obj = {
	a : 20,
	b : function () {
		console.log(this.a);
		
		function c() {
			console.log(this.a);
		}
		c();
	}
}
obj.b();
	
\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\
var a = 10;
var obj = {
	a : 20,
	var self = this;
	b : function () {
		console.log(this.a);
		
		function c() {
			console.log(self.a);
		}
		c();
	}
}
obj.b();
```

## 3 메소드 
메소드 호출 주체 (메소드명 앞)
```
var a = {
	b : function () {
		console.log(this);
	}
}

a.b();
```

## 4 callback
기본적으로 함수 내부와 동일

### call, apply, bind 예제
this 를 개발자가 지정
```
function a(x,y,z) {
	console.log(this, x,y,z);
}

var b = {
	c: 'eee'
};

a.call(b,1,2,3);
a.apply(b,[1,2,3]);

var c= a.bind(b);
c(1,2,3);

var d= a.bind(b,1,2);
d(3);
	
```
