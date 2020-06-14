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
\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\
var d = {
	e: function () {
		function f() {
			console.log(this);
		}
		f();	//이 것만 보면 된다..
	}
}
d.e();
\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\
\\ 내부함수에서 우회법
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
