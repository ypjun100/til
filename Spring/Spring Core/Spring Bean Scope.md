# Spring Bean Scope

* 빈 스코프는 애플리케이션 컨텍스트에 존재하는 빈에 대한 생명 주기와 같은 특성을 정의함
* 스프링에서는 다양한 빈 스코프가 존재하며, 그 중 `싱글톤 스코프`와 `프로토타입 스코프`를 대표적임
* 웹 관련 스코프로는 `요청 스코프`, `세션 스코프`, `애플리케이션 스코프`, `웹소켓 스코프`가 있음

### 싱글톤 스코프
```java
@Bean
@Scope("singleton") // 싱글톤 스코프로 빈 생성
public SomeClass someClassSingleton() {
	return new SomeClass();
}
```
* 싱글톤 스코프로 정의하면, 스프링 컨테이너는 컨텍스트 내부에 단 하나의 인스턴스를 생성함
* 해당 인스턴스를 토대로 스프링 컨테이너는 빈에 대한 모든 요청을 처리하게 됨
* **별다른 스코프를 지정하지 않은 경우, 모든 빈은 싱글톤 스코프로 생성되게 됨**
* 싱글톤 스코프로 생성된 빈은 스프링 컨테이너에 의해 생명 주기가 관리됨

### 프로토타입 스코프
```java
@Bean
@Scope("prototype") // 프로토타입 스코프로 빈 생성
public SomeClass someClassSingleton() {
	return new SomeClass();
}
```
* 프로토타입 스코프로 선언된 빈은 요청이 들어올 때마다 새로운 인스턴스를 생성하여 반환함
* 따라서, `SomeClass` 빈을 주입받는 객체들은 서로 다른 빈에 대한 참조를 가지고 있음
* 스프링 컨테이너는 프로토타입 스코프을 생성하고 설정한 뒤로는 생명 주기를 더이상 관리하지 않음
	* 빈 제거 단계는 컨테이너에서 별도로 관리하지 않으며, 콜백 함수또한 지원되지 않음
	* 때문에 빈을 제거하기 위해서는 사용자가 직접 리소스를 해제하고 제거하는 절차가 필요함

### 웹 관련 스코프
* `요청 스코프` : 단일 HTTP 요청을 위한 빈 인스턴스를 생성하는 스코프
* `세션 스코프` : 단일 HTTP 세션을 위한 빈 인스턴스를 생성하는 스코프
* `애플리케이션 스코프` : `ServletContext`의 생명 주기를 관리하기 위한 빈을 생성하는 스코프
* `웹소켓 스코프` : 개별의 웹소켓 세션에 대한 빈을 생성하는 스코프