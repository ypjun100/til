# ObjectMapper

* JSON 처리를 위한 라이브러리인 Jackson의 기능 중 하나로, 직렬화와 역직렬화 기능을 제공함
	* 객체를 JSON 형태로 변환하는 것은 직렬화, 반대로 JSON을 객체로 변환하는 것은 역직렬화
* ObjectMapper는 Java의 Reflection을 사용하기 때문에 상당한 오버헤드가 발생됨
* 따라서, **많은 양의 데이터를 처리하거나 트래픽이 많은 환경에서 운용할 경우 성능 이슈가 발생할 수 있음**

> Java의 Reflection은 구체적인 클래스 타입을 몰라도 그 클래스의 메서드, 타입, 변수들에 접근할 수 있도록 해주는 Java API로, 런타임에 특정 클래스의 정보를 추출할 때 사용된다.

### 직렬화 (Object -> JSON)
```java
ObjectMapper mapper = new ObjectMapper();

SomeObject obj = new SomeObject();
String JSON = mapper.writeValueAsString(obj); // 객체를 문자열로 변환
```

### 역직렬화 (JSON -> Object)
```java
ObjectMapper mapper = new ObjectMapper();

String json = "{\"key\":\"value\"}";
SomeObject obj = mapper.readvalue(json, SomeObject.class); // JSON 문자열을 객체로 변환
```
