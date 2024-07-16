# IoC와 DI를 위한 어노테이션

* IoC(Inverse of Control)와 DI(Dependency Injection)를 위한 어노테이션이 존재함
	* Spring에서 IoC는 모든 의존성 객체(Bean)들을 스프링 컨테이너에 저장시켜 관리하도록 하는 방식
	* DI는 IoC를 통해 스프링 컨테이너 내에 저장된 빈들을 필요한 클래스에 주입시키는 행위
	* **IoC와 DI를 통해 의존성 객체들을 중앙화하여 코드 가독성, 안정성 등의 이점을 얻을 수 있음**
* IoC를 위한 어노테이션으로 `@Component`가 있으며, 이를 기반한 여러 커스터마이즈 어노테이션을 제공함
* DI를 위한 대표적인 어노테이션으로는 `@Autowired`가 있으며, 해당 어노테이션을 통해 빈을 주입받음

### @ComponentScan
```java
@ComponentScan(basePackages = "com.junyeong.test")
public class AutoDependencyConfig { ... }
```
* `@ComponentScan` 어노테이션을 통해 탐색 위치에 `@Component`가 붙은 클래스를 스프링 빈으로 등록함
* 탐색할 클래스가 들어있는 패키지를 `basePackages`를 통해 지정해 줄 수 있음

### @Component
```java
@Component
public class ChatConfig { ... }
```
* Stereotype 어노테이션이란 Spring 애플리케이션에서 클래스의 역할을 지정하는 어노테이션
* `@Component`은 Root Stereotype 어노테이션으로, 이를 기반으로 파생된 여러 어노테이션이 존재함
	* `@Service`, `@Repository`, `@Controller` 어노테이션이 존재함
* Stereotype 어노테이션을 사용한 클래스는 ApplicationContext의 BeanFactory에 빈으로 등록됨

### @Autowired
* BeanFactory에 등록된 빈을 클래스 내부에서 의존성 주입받기 위한 어노테이션
* `Setter 메서드 주입`, `필드 주입`, `생성자 주입` 방법이 있으며, 이 중에서 생성자 주입 방법이 권장됨
* **Setter 메서드 주입방법**
	```java
	public class SetterInjectionMethod {
		private InjectedBean injectedBean;

		@Autowired
		public void setInjectedBean(InjectedBean injectedBean) {
			this.injectedBean = injectedBean;
		}
	}
	```
	* `SetterMethod` 클래스가 생성된 이후에 필드가 외부에서 메서드를 통해 변경되므로 권장되지 않음
* **필드 주입**
	```java
	public class FieldInjectionMethod {
		@Autowired
		private InjectedBean injectedBean;
	}
	```
	* Setter 주입방법과 마찬가지로 클래스 생성 이후에 외부에서 필드가 변경되므로 권장되지 않음
* **생성자 주입**
	```java
	public class ConstructorInjectionMethod {
		private final InjectedBean injectedBean;
		
		@Autowired // 명시하지 않아도 자동으로 추가됨
		public ConstructorInjectionMethod(final InjectedBean injectedBean) {
			this.injectedBean = injectedBean;
		}
	}
	```
	* 주입되는 빈이 상수로 선언되어, `ConstructorInjectionMethod` 클래스 생성이후 변경이 불가함
	* 따라서, 다른 방법들에 비해 안전하며, 해당 방법이 가장 권장됨

### @SpringBootApplication
```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(
    excludeFilters = {
       @Filter(type = FilterType.CUSTOM, classes = {TypeExcludeFilter.class}), 
       @Filter(type = FilterType.CUSTOM,
		       classes = {AutoConfigurationExcludeFilter.class})
    }
)
public @interface SpringBootApplication { ... }
```
* Spring Boot 애플리케이션으로 시작하기 위한 여러 어노테이션들을 통합하여 제공하는 어노테이션
* 어노테이션 내부에 `@ComponentScan`을 포함하기 때문에 내부 클래스에서 별도로 선언해 줄 필요 없음