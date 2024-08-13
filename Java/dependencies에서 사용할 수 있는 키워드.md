#  dependencies에서 사용할 수 있는 키워드

* Java의 Classpath는 클래스 혹은 JAR 파일이 존재하는 위치임
* JVM이 프로그램을 실행할 때 클래스 혹은 라이브러리를 찾기 위한 기준이 되는 경로
* 컴파일 혹은 런타임에 필요한 파일을 정의하기 위해 `compileClasspath`, `runtimeClasspath`로 구분
* 이를 Gradle에서 구분짓기 위해 `compileOnly`, `runtimeOnly`, `implementation` 옵션을 사용함
* 라이브러리마다 옵션을 적절히 사용하여 빌드 속도와 빌드 크기를 최적화할 수 있음

### compileOnly
```
dependencies {
	compileOnly 'junit:junit' // 코드 테스트를 위한 라이브러리이므로, 컴파일 타임에만 필요함
}
```

* 라이브러리가 컴파일 타임에만 필요하다는 것을 나타내며 `compileClasspath`에 라이브러리가 추가됨
* 컴파일 이후에 생성된 빌드 결과물에는 해당 라이브러리를 포함하지 않아 용량이 줄어드는 장점이 존재함
* 런타임 시점에 필요없는 라이브러리이거나, 이미 런타임 환경에서 라이브러리가 제공되는 경우에 이용함

### runtimeOnly
```
dependencies {
	runtimeOnly 'mysql:mysql-connector-java' // JDBC 드라이버는 런타임 시점에만 필요함
}
```

* 라이브러리가 런타임 시점에만 필요하다는 것을 나타내며, `runtimeClasspath`에 라이브러리가 추가됨
* 프로젝트를 빌드할 때에는 라이브러리를 추가하지 않고, 런타임에 필요한 경우에만 프로젝트에 포함시킴
* 임포트하는 라이브러리가 변경되어도 프로젝트 내에 포함되지 않았기 때문에 컴파일을 다시 할 필요 없음

### implementation
```
dependencies {
	implementation 'org.springframework.boot:spring-boot-starter-web' // Spring Boot 라이브러리로, 컴파일 타임과 런타임 시점에 필요함
}
```

* 라이브러리가 컴파일 및 런타임 모두 필요하다는 것을 나타냄
* 프로젝트 빌드 시점에 라이브러리를 컴파일에 사용하고, 빌드된 결과물에도 해당 라이브러리를 포함시킴
* 라이브러리와 관련된 클래스 및 메서드를 프로젝트 내에서 직접 참조할 수 있게 됨