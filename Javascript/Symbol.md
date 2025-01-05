# Symbol

* 문자열, 숫자, 불리언, `undefined`, `null`, 객체 타입 이후에 도입된 변경 불가능한 원시 데이터 타입
* 다른 값과 중복되지 않는 값으로, 주로 객체에서 충돌 위험이 없는 유일한 프로퍼티 키를 만들기 위해 사용됨

### Symbol 함수를 이용한 생성

```js
const mySymbol = Symbol();

// 실제 심벌 값은 외부로 노출되지 않아 확인할 수 없음
console.log(mySymbol); // -> Symbol()
```

* 다른 원시값과 달리 리터럴 표기법을 통해 값을 생성할 수 없고, `Symbol` 함수를 활용해 생성함
* 이때 생성된 심벌 값은 외부로 노출되지 않아 확인할 수 없으며, 다른 값과 절대 중복되지 않는 값임
	* 따라서, 위에서 설명했듯이 객체의 유일한 키 값을 지정하기 위해 심벌을 사용하는 것임

```js
// 심벌 값에 대한 설명이 같더라도 유일무이한 심벌 값을 생성함
const mySymbol1 = Symbol('mySymbol');
const mySymbol2 = Symbol('mySymbol');

console.log(mySymbol1 === mySymbol2); // -> false
```

* `Symbol` 함수에는 선택적으로 문자열을 인수로 전달할 수 있으며, 이 값은 심벌 값에 대한 설명을 나타냄
	* 또한, 이 값은 디버깅 용도로만 사용되며, 심벌 값 생성에 어떠한 영향을 주지 않음
	* 따라서, 심벌 값에 대한 설명이 같더라도 생성된 심벌 값은 유일무이한 값임

### Symbol.for / Symbol.keyFor 메서드

```js
// 전역 심벌 레지스트리에 'mySymbol'이라는 키로 값을 생성함
const s1 = Symbol.for('mySymbol');

// 전역 심벌 레지스터리에 'mySymbol'로 저장된 키를 가져옴
const s2 = Symbol.for('mySymbol');

console.log(s1 === s2); // -> true
```

* `Symbol.for`는 인수로 전달받은 문자열로 심벌 레지스트리에 해당 키와 일치하는 심벌 값을 검색함
	* 검색에 성공하면 새로운 심벌 값을 생성하지 않고 검색된 심벌 값을 반환함
	* 검색에 실패하면 새로운 심벌 값을 생성한 후 심벌 레지스트리에 저장 및 반환함
* `Symbol()` 함수를 사용해 심벌 값을 생성하는 경우 심벌 레지스트리에 저장되지 않음
* 심벌 레지스트리에 키를 저장함으로써 애플리케이션 전역에서 유일한 상수인 심벌 값을 공유할 수 있음

```js
const s1 = Symbol.for('mySymbol');

Symbol.keyFor(s1); // -> mySymbol
```

* `Symbol.keyFor` 메서드를 사용하면 전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출할 수 있음