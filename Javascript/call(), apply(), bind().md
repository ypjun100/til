# call(), apply(), bind()

```js
function sum(num1, num2, num3) {
	console.log(num1 + num2 + num3);
}

sum(1, 2, 3);                // 일반적인 함수 호출 방법
sum.call(null, 1, 2, 3);     // call()을 이용한 함수 호출 방법
sum.apply(null, [1, 2, 3]);  // apply()를 이용한 함수 호출 방법
```

* 자바스크립트에서 함수를 호출하기 위한 세 가지 방법으로 `call()`, `apply()`, `bind()`가 존재함
* 이때 화살표 함수의 경우에는 세 메서드를 적용해도 `this` 바인딩은 절대 변경되지 않음
	* 화살표 함수는 무조건 상위 스코프의 `this`를 참조하기 때문

### `call()`을 이용한 함수 호출

```js
call(thisArg)
call(thisArg, arg1)
call(thisArg, arg1, arg2)
call(thisArg, arg1, arg2/*...*/, argN)
```

* `thisArg`는 함수에 전달되는 this 값이고, `arg`는 함수에 전달되는 인자값들임
* 만약 함수 내에 사용되는 `this` 키워드를 다른 객체에 연결하고 싶은 경우에 `thisArg`에 해당 객체를 넣음

### `apply()`를 이용한 함수 호출

```js
apply(thisArg, [argsArray])
```

* `thisArg`는 함수에 전달되는 this 값, `argsArray`는 배열 형태의 인자값들임
* `call()`과 유사하지만, 전달되는 인자값을 배열 형태로 넣는다는 특징이 있음

### `bind()`를 이용한 함수 호출

```js
bind(thisArg, arg1, arg2/*...*/, argN)
```

* `thisArg`는 함수에 전달되는 this 값이고, `arg`는 함수에 전달되는 인자값들임
* `call()`과 `apply()`와 달리 함수를 바로 호출하지 않고 반홤함

```js
let person1 = { name: 'Junyeong' };
let person2 = {
	name: 'Gildong',
	greet: function() {
		console.log(`Hello! I'm ${this.name}.`);
	}
}

person2.greet(); // -> Hello! I'm Gildong.

let junyeong = person2.greet.bind(person1);
junyeong(); // -> Hello! I'm Junyeong.
```