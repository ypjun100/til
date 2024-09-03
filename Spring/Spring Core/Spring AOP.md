# Spring AOP

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F994AA3335C1B8C9D28)

* AOP(Aspect Oriented Programming)는 관점 지향 프로그래밍 기법임
* 어떤 로직을 `핵심적인 관점`, `부가적인 관점`으로 나누어서 보고, 각 관점을 기준으로 모듈화하는 방식
	* `핵심적인 관점`은 우리가 적용하고자 하는 핵심 비즈니스 로직
	* `부가적인 관점`은 핵심 로직을 실행하기 위한 데이터베이스 연결, 로깅, 파일 입출력 로직
* 전체 로직에서 반복되어 사용되는 코드(DB 연결, 로깅 등)들을 모듈화하여 이를 재활용함
	* 하나의 코드가 여러 곳에 반복되어 사용되는 것을 `흩어진 관심사`라고 함

### 주요 키워드
* `Aspect` : 흩어진 관심사를 모듈화 한 것을 의미하며, 주로 부가기능들을 모듈화 함
* `Target` : `Aspect`를 적용하는 곳으로, 클래스나 메서드 등 여러 곳에 적용할 수 있음
* `Advice` : `Aspect`가 로직을 모듈화한 추상체라면, `Advice`는 각 모듈을 구현한 실질적인 구현체
* `JointPoint` : `Advice`가 적용되는 위치로, 메서드 진입, 생성자 호출, 필드 값 추출 시점에 적용 가능
* `PointCut` : `JointPoint`의 상세스펙을 정의한 것으로, `Advice`가 실행될 구체적인 진입 시점을 명시

### Spring AOP의 특징
* 스프링 내에서는 자체적으로 AOP를 지원하기 위해 프록시 패턴을 이용함
	* `Advice`를 프록시로 만든 뒤에 의존성 주입으로 연결되는 빈에 적용함
	* 이후에 `Target`에서 해당 빈의 메소드 호출 과정에 참여하여 부가기능을 제공함
* 모든 AOP 기능을 제공하는 것이 아닌, 스프링 IoC와 연동하여 흔한 문제에 대한 해결책을 제시해주기 위함