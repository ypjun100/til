# String Constant Pool

* Java에서 String을 생성할 때 리터럴을 사용하면, 값은 **힙 내에 있는 String Constant Pool에 저장됨**
* 그리고 String 객체 또한 String Pool Constant에 있는 값을 참조하게 됨
* **동일한 문자열을 갖는 두 String은 동일한 주소값을 갖게 됨**
* 문자열을 변경할 경우 **원래 값을 변경하는 것이 아닌 풀에 새로운 문자열을 생성하여 이를 참조함**
	* 문자열을 변경하는 경우 주소값이 바뀌게 되며, 그렇기 때문에 String은 불변(Immutable)임
* 두 개의 문자열을 비교할 때 `==` 연산자를 사용하지 않고, `equals()` 메서드를 사용하는 이유임
* 일반적으로 풀에 저장된 값은 GC의 대상이 되지 않음

> **String을 불변으로 설계함으로써 얻는 이점**
> * 문자열을 풀에 저장하여 다른 객체와 공유함으로써 데이터 캐싱이 발생되어 성능적 이득을 취할 수 있음
> * 데이터가 불변하다면 멀티 쓰레드 환경에서 동기화 문제가 발생하지 않음
> * 절대 값이 바뀌지 않기 때문에 데이터 무결성을 유지할 수 있음

### 문자열 생성 방식
```java
// 문자열 리터럴을 이용한 방식
String str1 = "Hello World";
String str2 = "Hello World";
// -> 두 객체는 동일한 주소값을 가짐

// 생성자를 이용한 방식
String str3 = new String("Hello World");
String str4 = new String("Hello World");
// -> 두 객체는 서로 다른 주소값을 가짐
```

* 리터럴을 이용한 방식은 두 객체가 String Constant Pool 내의 동일한 문자열을 참조하고 있기 때문에 동일한 주소값을 가짐
* 생성자를 이용한 방식의 경우 String Constant Pool에 저장되지 않고, 바로 Heap에 저장되므로 문자열이 동일해도 서로 다른 주소값을 가짐