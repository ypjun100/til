# Generic Type Erasure

```java
// 컴파일 전
public class Test<T> {
	public void test(T test) {
		System.out.println(test.toString());
	}
}

// 컴파일 후 (타입 소거 후)
public class Test {
	public void test(Object test) {
		System.out.println(test.toString());
	}
}
```

* 컴파일 시 코드에서 제네릭 타입의 정보를 제거하고, Object와 같은 기본 타입으로 대체하는 과정
* 타입 소거(Type Erasure)를 수행하는 이유는 이전 버전과의 하위 호완성을 지원하기 위함
* 하지만 런타임 시점에 제네릭 타입의 정보를 알 수 없기 때문에 제네릭 객체에 대한 타입을 결정할 수 없음

### 타입 소거 규칙
* Unbounded Type(`<?>`, `<T>`)은 Object로 변환됨
* Upper Bound Type(`<E extends SomeClass>`)는 SomeClass로 변환됨
* 제네릭 타입을 사용할 수 있는 일반 클래스, 인터페이스, 메소드에만 소거 규칙을 적용함
* 제네릭 타입 보존을 위해선 타입 캐스팅을 추가하고, 타입 다형성 보장을 위해선 브릿지 메소드를 생성함

### 브릿지 메소드
```java
public class MyComparator implements Comparator<Integer> {
	public int compare(Integer a, Integer b) { ... }
}
```

* 위의 경우에는, 컴파일 타임에 `Comparator`의 `compare` 메소드의 파라미터 타입이 Object로 변환됨
* 따라서 `MyComparator`의 `compare` 메소드가 `Comparator`에 오버라이딩 될 수 없는 문제 발생
* 이를 해결하기 위해 자바에서는 자동으로 `MyComparator` 클래스에 아래와 같은 브릿지 메소드를 추가함

```java
// 컴파일 이후의 MyComparator
public class MyComparator implements Comparator {
	public int compare(Integer a, Integer b) { ... }

	// 컴파일러에서 자동으로 브릿지 메소드 추가
	public int compare(Object a, Object b) {
		return compare((Integer)a, (Integer)b);
	}
}
```

> **추가 설명**
> 위의 `MyComparator`를 사용하기 위해 `Comparator<Integer> comp = MyComparator();`로 객체를 생성했다고 할 때, 컴파일 이후에는 `Comparator`의 타입이 소거되어 Object 타입으로 변환된다. 그렇다면, `comp` 객체의 `compare` 메소드를 사용하기 위한 매개변수의 타입 또한 Object가 입력될 것이므로, `MyComparator`에서 오버라이딩한 `compare` 메소드가 실행되지 않을 것이다. 따라서, 브릿지 메소드를 추가하는 것이다.