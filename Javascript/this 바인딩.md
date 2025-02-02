# this

* 자바스크립트의 함수가 호출될 때에는 매개변수로 전달되는 인자값 이외에 `this`를 암묵적으로 전달받음
* 따라서, 자바스크립트에서 `this`는 선언의 종류가 아닌, 호출되는 방식에 따라 다르게 바인딩됨
* 함수의 호출 방식은 크게 함수 호출, 메서드 호출, 생성자 함수 호출 등 여러 방식이 존재함
	* 함수를 호출하는 객체가 있는 경우 `메서드`, 호출하는 객체가 없는 경우 `함수`라고 함
	* 아래 예시들은 실질적으로 함수를 호출하게 되는 경우에 대한 바인딩의 예시

> 함수의 상위 스코프를 결정하는 방식인 렉시컬 스코프는 함수 정의가 평가되어 함수 객체가 생성되는 시점에 상위 스코프를 결정하지만, `this` 바인딩은 함수 호출 시점에 결정된다.

### 단독으로 사용된 `this`

```js
var x = this;
console.log(x); // Window (node.js 환경에서는 global 객체가 바인딩됨)
```

* 단독으로 사용된 `this`는 무조건 global 객체를 가리킴

### 함수 안에서 사용된 `this`

```js
function myFunction() {
	return this;
}
console.log(myFunction()); // Window
```

* 함수 안에서 사용된 `this`는 해당 함수를 호출한 곳의 `this`에 바인딩됨
* 하지만 만약, 엄격 모드인 경우에는 `this`에 디폴트 바인딩이 없기 때문에 `undefined`가 반환됨
* **이때 함수 내에 함수가 존재하는 형태인 내부 함수의 경우 무조건 전역 객체를 참조함**

### 메서드에서 사용된 `this`

```js
var num = 0;

function showNum() {
	console.log(this.num);
}

showNum(); // 0

var obj = {
	num: 200,
	func: showNum
};

obj.func(); // 200
```

* `this` 키워드를 메서드 내부에서 사용 시, 해당 메서드를 호출한 객체로 바인딩됨

### 생성자 안에서 사용된 `this`

```js
function Person(name) {
	this.name = name;
}

var kim = new Person('kim');
var lee = new Person('lee');

console.log(kim.name); // kim
console.log(lee.name); // lee
```

* 생성자 안에서 사용된 `this`는 생성자 함수가 생성한 객체로 바인딩됨

### 화살표 함수에서 사용된 `this`

```js
var obj = {
	outer: function() {
		console.log(this); // obj
		var innerFunc = () => {
			console.log(this); // obj
		}
		innerFunc();
	}
};
obj.outer();
```

* ES6에서 도입된 화살표 함수는 바인딩 과정이 생략되어 상위 스코프의 `this`를 그대로 사용할 수 있음