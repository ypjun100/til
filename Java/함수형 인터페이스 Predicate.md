# 함수형 인터페이스 Predicate

```java
@FunctionalInterface // 해당 어노테이션을 적용한 인터페이스는 오직 하나의 메서드만 가짐
public interface Prediate\<T> {
	booelan test(T t);
}
```

* `java.util.function`에 정의된 함수형 인터페이스 중 하나로, 매개변수는 하나이고, 논리 값을 반환함
	* 해당 패키지에는 일반적으로 자주 쓰이는 형식의 메서드를 함수형 인터페이스로 미리 정의해 놓음
	* 필요에 따라 새로운 함수형 인터페이스를 정의하는 것이 아닌, 기존에 정의된 함수를 이용할 수 있음
* `Predicate`가 스트림에 의해 사용되는 경우 제네릭 타입은 자동으로 해당 스트림의 타입으로 치환됨
	* ex) `Stream<String>`인 경우, 해당 스트림의 `filter()` 메서드의 매개변수는 `Preciate<String>`이 됨

### filter()에서 사용되는 Predicate
* 스트림에서 `filter()`를 적용할 때 매개 타입으로 `Preciate`이 요구됨
* `filter()`는 `Preciate`에서 반환된 논리값을 바탕으로 해당 값을 유지하거나 제거함
```java
List<Integer> list = Arrays.asList(1, 2, 3, 4, 5, 6, 7 ,8, 9, 10);
List<Integer> collected = list.stream()
		.filter(x -> x > 5)             // 반환 타입으로 논리 타입이 주어짐
		.collect(Collectors.toList());

System.out.println(collected); // -> [6, 7, 8, 9, 10]
```

### Predicate.negate()
* `!`(not) 연산과 동일하게 작동함
```java
Predicate<String> startWithA = x -> x.startsWith("A");
List<String> list = Arrays.asList("A", "AA", "AAA", "B", "BB", "BBB");
List<String> collected = list.stream()
			.filter(startWithA.negate())
			.collect(Collectors.toList());

System.out.println(collected); // -> [B, BB, BBB]
```


### Chaining
* 여러 개의 `Predicate`를 `and()`, `or()`, `negate()`로 연결해서 새로운 `Preciate`로 결합할 수 있음
```java
Preciate<Integer> pre = i -> i < 100;
Preciate<Integer> post = pre.and(i -> i < 200).or(i -> i % 2 == 0).negate();
System.out.println(post.test(10)); // -> false
```
