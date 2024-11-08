# 함수 호출 방식과 this 바인딩

```js
// 함수는 다양한 방식으로 호출될 수 있음
const foo = function() {
	console.dir(this);
};

// 1. 일반 함수 호출
// foo 함수 내부의 this 객체는 전역 객체 window를 가리킴
foo(); // window

// 2. 메서드 호출
// foo 함수를 프로퍼티 값으로 할당하여 호출
// foo 함수 내부의 this는 메서드를 호출한 객체 obj를 가리킴
const obj = { foo };
obj.foo(); // obj{}

// 3. 생성자 함수 호출
// foo 함수를 new 연산자와 함께 생성자 함수로 호출
// foo 함수 내부의 this는 생성자 함수가 생성한 인스턴스를 가리킴
new foo(); // foo {}

// 4. apply(), call(), bind() 메서드에 의한 간접 호출
// foo 함수 내부의 this는 인수에 의해 결정됨
const bar = { name: 'bar' };
foo.call(bar); // bar{}
foo.apply(bar); // bar{}
foo.bind(bar)(); // bar{}
```

* `this` 바인딩은 함수 호출 방식에 따라 동적으로 결정됨

### 일반 함수 호출

* 일반 함수로 호출하면, `this`에는 전역 객체가 바인딩되며, strict mode에서는 `undefined`가 바인딩됨
* 중첩 함수나 콜백 함수도 일반 함수로 실행되면, `this`에 전역 객체가 바인딩됨
	* 객체의 콜백 함수 내에서 `this`를 사용하기 위해서 `apply()`, `bind()`, `call()`을 사용함
* 일반 함수 중에서 화살표 함수의 경우 상위 스코프의 `this`를 가리킴

	```js
	const obj = {
		value: 100,
		foo() {
			setTimeout(() => console.log(this.value), 100); // 100
		}
	};
	obj.foo();
	```

### 메서드 호출

* 메서드 내부의 `this`에는 해당 메서드를 소유한 객체가 아닌, 메서드를 호출한 객체가 바인딩 됨
	* `person.getName()` 이라고 할 때, `getName()` 메서드를 호출한 객체는 `person`
	* `getName()`은 독립적인 별도의 객체로, `person`에서 이를 참조하기 때문에 호출한 객체가 됨

### 생성자 함수 호출

* 생성자 함수 내부의  `this`에는 생성자 함수가 생성할 인스턴스가 바인딩됨

### `apply()`, `call()`, `bind()` 메서드에 의한 간접 호출

```js
// 주어진 this 바인딩과 인수 리스트 배열을 사용하여 함수를 호출함
Function.prototype.apply(thisArg[, argsArray])

// 주어진 this 바인딩과 ,로 구분된 인수 리스트를 사용하여 함수를 호출함
Function.prototype.call(thisArg[, arg1[, arg2[, ...]]])

// thisArg로 설정된 값을 this로 바인딩하여 함수를 반환함
Function.prototype.bind(thisArg[, arg1[, arg2[, ...]]])
```

* 세 함수 모두 `Function.prototype`로, 이들 메서드는 모든 함수가 상속받아 사용할 수 있음
* `apply()`와 `call()` 메서드는 함수를 호출하는 데 사용되며, 인수 전달 방식만 다를 뿐 동일하게 동작함
	* 두 메서드의 대표적 용도는 `arguments`와 같은 유사 배열 객체에 배열 메서드를 사용하는 경우
* `bind()`는 별도로 함수를 호출하지 않고, 인수로 전달된 값을 `this`로 바인딩하여 새로운 함수를 반환함
	* 메서드의 `this`와 메서드 내부 중첩, 콜백 함수의 `this`가 불일치하는 문제를 해결하기 위해 사용됨