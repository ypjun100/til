# instanceof 연산자

```js
// 생성자 함수
function Person(name) {
	this.name = name;
}

const me = new Person('Lee');

// Person.prototype이 me 객체의 프로토타입 체인 상에 존재하므로 true로 평가됨
console.log(me instanceof Person); // true

// Object.prototype이 me 객체의 프로토타입 체인 상에 존재하므로 true로 평가됨
console.log(me instanceof Object);
```

* `instanceof` 연산자는 이항 연산자로, 좌변에는 객체를 가리키는 식별자, 우변에는 생성자 함수가 입력됨
	* 만약 우변의 피연산자가 함수가 아닌 경우 TypeError가 발생됨
* 우변의 생성자 함수의 객체가 좌변 객첵의 프로토타입 체인 상에 존재하면 true로, 아니면 false로 평가됨