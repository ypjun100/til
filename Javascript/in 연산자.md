# in 연산자

```js
const person = {
	name: 'Lee',
	address: 'Seoul'
};

// 객체 내에 특정 프로퍼티가 존재하는지 확인
console.log('name' in person); // -> true
console.log('age' in person); // -> false
console.log('toString' in person); // -> true

// 프로퍼티 순회
for (const key in person) {
	console.log(key + ": " + person[key]);
}
// name: Lee
// address: Seoul
```

* `in` 연산자는 객체 내에 특정 프로퍼티가 존재하는지 여부를 확인하거나 순회할 수 있음
* 프로토타입 체인 내에 있는 모든 프로퍼티를 검색하기 때문에 `toString`의 결과도 참이 됨
* `for ... in` 연산자에서 `toString`과 같은 `Object.prototype` 프로퍼티는 열거되지 않음
	* `toString` 메서드는 프로퍼티 속성인 `[[Enumerable]]`의 값이 false이기 때문에 열거되지 않음
	* 따라서, 객체 프로토타입 체인 상에 존재하는 프로퍼티 중 `[[Enumerable]]` 값이 참인 것만 열거됨
* 배열에는 `for ... in` 대신 `for ... of`나 `forEach` 메서드를 사용하는 것이 권장됨