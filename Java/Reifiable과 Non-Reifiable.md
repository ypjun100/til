# Reifiable과 Non-Reifiable

* Reifiable과 Non-Reifiable 개념은 Java의 제네릭 기능으로 인해 발생된 개념
* 제네릭의 타입 변수는 컴파일 시점에서 Type Erasure에 의해 제거되므로 제거되는 타입과 그렇지 않은 타입의 구분이 필요함

### Reifiable 타입
* **런타임 시에 완전하게 오브젝트 타입을 표현할 수 있는 타입**으로, 컴파일 단계에서 타입이 제거되지 않은 타입
* **Reifiable 타입의 예시**
	* 원시 타입 (int, float, double...)
	* 일반적인 클래스와 인터페이스 타입
	* Unbounded Wildcard(?)가 포함된 타입 변수 (List\<?>, ArrayList\<?>...)
	* Raw 타입 (List, ArrayList, Map...)

> Unbounded Wildcard는 애초에 타입 정보를 명시하지 않아 컴파일 시에 타입이 Object로 치환되기 때문에 Reifiable이라 볼 수 있음

### Non-Reifiable
* 런타임 시에 오브젝트 타입을 표현할 수 없는 타입으로, **Type Erasure에 의해 타입이 제거된 타입**
* **Non-Reifiable 타입의 예시**
	* 제네릭 타입
	* 기본적인 타입 변수 (List\<Integer>...)
	* 경계가 포함된 타입 변수 (List\<? extends Integer>...)

> 타입 변수의 경우 컴파일 시점에 Type Erasure에 의해 타입이 원시 타입으로 변환됨. 예를 들어 `List<String>`의 경우 컴파일 이후에는 원시 타입인 `List`로 변환됨

```java
public class Main {
	public static void main(String[] args) {
		List<Integer> integers = Arrays.asList(1, 2, 3);
		System.out.println(isInterList(integers));
	}

	public static <T> boolean isIntegerList(T list) {
		return list instanceof List<Integer>; // 컴페일 에러 발생
		// return list instanceof List<?>; // 정상적으로 작동함
	}
}

// 컴파일 시점에 제네릭은 원시 타입으로 변경되기 때문에 런타임 시점에 List<Integer>임을 확인할 수 없음
// 이로인해 instanceof 연산자는 피연산자로 제네릭을 받을 수 없도록 구현했기 때문에 사전에 컴파일 에러 발생
```