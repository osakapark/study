#closure

1.A closure is the combination of a function and the lexical environment within which that function was declared;

2.실행컨텍스트 A와 함수 B가 콤비가 되어 무언가를 한다.

3.컨텍스트 A에서 선언한 변수를  
내부함수 B에서 접근할 경우에만 발생하는 특수한 현상

4.B의 outerEnvironmentReference 는 A의 environmentRecord 를 참조 

5.컨텍스트 A에서 선언한 변수 a를 참조하는 내부함수 B를  
  A의 외 부로 전달할 경우,
  A가 종료된 이후에도   
  a가 사라지지 않는 현상

6.지역변수가 함수종료 후에도 사라지지 않게 할 수 있다. 
  함수 종료 후에도 사라지지 않는 지역변수를 만들 수 있다.


```
var outer = function() {
	var a= 1;
	var inner = function () {
		console.log(++a);
	};
	inner();
}
outer();
```

```
var outer = function() {
	var a= 1;
	var inner = function () {
		return ++a;
	};
	return inner;
}

var outer2 = outer();
console.log(outer2());
console.log(outer2());
```


```
function a() {
	var localA = 1;
	var localB = 2;
	var localC = 3;
	return {
		get a() { return localA;},
		set a(v) { localA = v;},
		get b() { return localB + localC; },
		set b(v) { throw Error('read only');}
	}
}
var obj = a();

obj.a;
obj.a = 3;
obj.b;
//obj.b = 10; //오류
```
