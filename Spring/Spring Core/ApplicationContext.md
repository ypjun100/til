# ApplicationContext

* Spring IoC(Inversion of Control) 컨테이너는 애플리케이션 내의 객체들을 전반적으로 관리함
	* 이를 위해 의존성 주입(Dependency Injection)과 제어의 역전(Inversion of Control) 제공함
* `BeanFactory`와 `ApplicationContext` 인터페이스는 Spring IoC 컨테이너를 대변하게 됨
	* `BeanFactory`는 컨테이너 접근을 위한 루트 인터페이스로, 빈의 관리를 위한 기능들을 제공함
	* `ApplicationContext`는 `BeanFactory`를 상속받아 더 많은 기능들을 제공함

### 컨테이너 내에 빈을 구성하기 위한 방법
* `ApplicationContext`는 컨테이너에 빈을 구성하기 위한 방법들을 제공함
#### Java-Based Configuration
* 가장 최신 방법이자 선호되는 구성 방법으로 `@Bean`과 `@Configuration` 어노테이션을 이용함
* 메서드 위의 `@Bean` 어노테이션은 해당 메서드가 스프링 빈을 생성한다는 의미
* 클래스 위의 `@Configuration` 어노테이션은 클래스가 빈들을 정의한다는 의미
```java
@Configuration
public class AccountConfig {

	@Bean public AccountService accountService() {
		return new AccountService(accountRepository()); 
	}
	
	@Bean public AccountRepository accountRepository() {
		return new AccountRepository();
	}
	
}
```

#### Annotation-Based Configuration
* 기본적으로 제공하는 6가지의 어노테이션을 이용하여 빈을 구성하는 방법
	* `@Component`, `@Controller`, `@Service`, `@Repository`, `@Autowired`, `@Qualifer`
```java
// When defining a bean
@Component
public class UserService { ... }
```
```java
// When getting a bean
ApplicationContext context = (some context);
UserService userService = context.getBean(UserService.class);
```

#### XML-Based Configuration
* 가장 전통적인 방법으로, XML 파일 내에 정의하고 싶은 빈들을 매핑함
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	   xsi:schemaLocation=" http://www.springframework.org/schema/beans
	   http://www.springframework.org/schema/beans/spring-beans.xsd">
	<bean id="accountService"
		  class="com.baeldung.applicationcontext.AccountService">
		<constructor-arg name="accountRepository" ref="accountRepository" /> 
	</bean>
	<bean id="accountRepository"
		  class="com.baeldung.applicationcontext.AccountRepository" />
</beans>
```

### ApplicationContext의 종류
* 스프링은 서로 다른 요구 사항들을 충족하기 위한 다양한 `ApplicationContext` 인터페이스를 제공함

#### AnnotationConfigApplicationContext
* `@Configuration` 혹은 `@Component`로 정의된 클래스를 가져올 수 있음
```java
ApplicationContext context = new AnnotationConfigApplicationContext(AccountConfig.class);
AccountService accountService = context.getBean(AccountService.class);
```

#### AnnotationConfigWebApplicationContext
* XML-Based Configuration을 사용할 경우 xml 파일에서 정의된 클래스를 가져올 수 있음
* `FileSystemXMLApplicationContext`를 이용해 컴퓨터 내부에 있는 xml 파일을 가져올 수 있음
* `ClassPathXmlAPplicationContext`를 이용해 classpath 내부의 xml 파일을 가져올 수 있음
```java
XmlWebApplicationContext context = new XmlWebApplicationContext();
context.setConfigLocation("/WEB-INF/spring/applicationContext.xml");
context.setServletContext(container);
```