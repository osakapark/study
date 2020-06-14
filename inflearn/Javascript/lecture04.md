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

<결론>
1. 기본적으로 함수의 this와 같다
2. 제어권을 가진 함수가 call의 this를 명시한 경우 그에 따른다.
3. 개발자가 this를 바인당한 채로 callback 을 넘기면 그에 따른다.


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

```
var callback = function () {
	console.dir(this);
};

var obj = {
	a: 1,
	b: function (cb) {
		cb();	//이것만 보면 된다.
	}
};

obj.b(callback);
```

```
var callback = function () {
	console.dir(this);
};

var obj = {
	a: 1,
	b: function (cb) {
		cb();	//이것만 보면 된다.
		cb.call(this);	//this = obj가 됨
	}
};

obj.b(callback);
```

```
var callback = function () {
	console.dir(this);
};

var obj = {
	a: 1
};

setTimeout(callback, 100);	//this 별도 처리 안함
setTimeout(callback.bind(obj), 100); 	//this 설정
```

```
document.body.innerHTML += '<div id="a">를 클릭해라 </div>';

console.log(document);

document.getElementById('a')
	.addEventListener('click', function () {
		console.dir(this);
	});
```

```
document.body.innerHTML += '<div id="a">를 클릭해라 </div>';
var obj = {a : 1};

console.log(document);

document.getElementById('a')
	.addEventListener('click', function () {
		console.dir(this);
	}.bind(obj));
```

## 5. 생성자 함수 호출시 
instance

```
function Person (n, a) {
	this.name= n;
	this.age = a;
}

var gomugom = new Person ('고무곰', 30);
console.log(gomugom);	
```
