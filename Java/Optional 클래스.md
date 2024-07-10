# Optional 클래스

* Optional<\T> 클래스는 지네릭 클래스로 T타입의 객체를 감싸는 **래퍼 클래스**
* **특정 값의 결과가 null일 수 있는 경우 Optional 클래스를 적용**
	* 매번 null 타입인지 조건문으로 확인하는 대신, Optional에서 제공하는 메서드 사용 가능
	* null 체크를 위한 조건문 없이도 예외가 발생하지 않는 보다 간결한 코드 작성 가능

### Optional 객체 생성
```java
String str = "abc";
Optional<String> optStr = Optional.of(str);
Optional<String> optStr = Optional.ofNullable(str);

Optional<String> optStr = Optional.of(null); // NullPointerException 발생
Optional<String> optStr = Optional.ofNullable(null);
```

* Optional은 `of()`나 `ofNullable()`로 생성할 수 있음
* **만약 참조변수의 값이 null일 가능성이 있으면, `of()`대신 `ofNullable()`을 사용해야 함**
	* `of()`는 매개변수의 값이 null이면 NullPointerException 예외를 발생시킴

> `of()`와 `ofNullable()` 메서드를 구분하는 이유는 만약 `of()` 대신 `ofNullable()`을 적용시킨 후에 프로그램 상에서 예측하지 못한 null이 발생된다면 `NullPointerException`이 발생하지 않고, 이후의 작업들을 계속 수행하기 때문이다. 만약 `of()`를 사용한다면 Optional을 생성할 때 예외를 발생시켜 프로그램을 정지시키고 이후에 작업들을 진행하지 않도록 할 수 있다.

### Optional 객체의 값 가져오기
```java
Optional<String> optStr = Optional.of("abc");
String str1 = optStr.get();       // optStr에 저장된 값을 반환. null이면 예외발생
String str2 = optStr.orElse("");  // optStr에 저장된 값이 null일 때는, ""를 반환

String str3 = strVal.orElseGet(String::new); // () -> new String()와 동일
String str4 = strVal.orElseThrow(NullPointerException::new); // null이면 예외발생
```

* 기본적으로 옵셔널 내의 값을 가져올 때는 `get()`이나 `orElse()`를 사용함
* `orElseGet()`을 통해 null인 경우 대체할 값을 반환하는 람다식을 적용할 수 있음
* `orElseThrow()`는 값이 null인 경우 다른 예외를 발생시킬 수 있음

```java
result = Optional.of("")
				.filter(x -> x.length() > 0)
				.map(Integer::parseInt).orElse(-1); // -> -1
```

* Optional도 Stream처럼 `filter()`, `map()`, `flatMap()`을 적용할 수 있음
* 만약 Optional 객체의 값이 null이면, 이 메서들을 아무 일도 하지 않음

```java
if (Optional.ofNullable(str).isPresent()) {
	System.out.println(str);
}

Optional.ofNullable(str).ifPresent(System.out::println);
```

* `isPresent()`는 Optional 객체의 값이 null이면 false를, 아니면 true를 반환함
* `ifPresent(Consumer<T> block)`은 값이 있으면 주어진 람다식을 실행, 없으면 아무 일도 하지 않음