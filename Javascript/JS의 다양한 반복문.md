# JS의 다양한 반복문

### foreach
```js
var items = ['item1', 'item2', 'item3'];

items.forEach(function(value, index) {
	console.log(index + " : " + value);
});
// -> 0 : item1
// -> 1 : item2
// -> 2 : item3
```

* Array 객체와 Map, Set에서 사용가능한 메서드로, 각 요소에 접근하여 작업을 수행할 수 있음
* 콜백 함수에서 각 요소의 값과 인덱스 값을 가져올 수 있음

### for ... in
```js
var obj = {
	a: 1,
	b: 2,
	c: 3
};

for (var prop in obj) {
	console.log(prop, obj[prop]);
}
// -> a 1
// -> b 2
// -> c 3
```

* 객체의 속성들에 접근하여 작업을 수행할 수 있으며, 모든 객체에서 사용이 가능함
* `for ... in` 구문은 객체의 키값에 접근할 수는 있지만, value 값에는 바로 접근할 수 없음
* 객체의 순서대로 요소에 접근한다는 보장이 없기 때문에, 배열과 같은 요소에서 사용하지 않아야 함

### for ... of
```js
var iterable = [10, 20, 30];

for (var value of iterable) {
	console.log(value);
}
// -> 10
// -> 20
// -> 30
```

* ES6에서 추가된 컬렉션 전용 반복 구문으로, 객체가 `[Symbol.iterator]` 속성을 가지고 있어야 함
* `for ... in`과 달리 직접적으로 객체의 value 값에 접근이 가능함