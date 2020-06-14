#closure

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
