# IO(Input/Output) Connector

* Connector는 클라이언트와 소켓으로 연결되어 요청 데이터를 서블릿 컨테이너로 전달하는 역할을 수행함
	* 클라이언트가 전송한 데이터 패킷으로 `ServletRequest` 객체를 생성한 뒤 서블릿 컨테이너로 전달
* Connector에는 Blocking IO Connector와 Non-Blocking IO Connector가 있음
	* 더이상 Blocking IO Connector는 사용되지 않음

### BIO(Blocking I/O) Connector

* BIO는 HTTP 요청을 처리하여 응답한 뒤에 연결이 종료될 때까지 쓰레드를 점유한 상태로 대기하는 방식
* 클라이언트 하나당 하나의 쓰레드를 할당하며, 커넥션이 종료될 때까지 쓰레드가 커넥션에 할당되어 있음
* 단순한 방식이지만, 다수의 클라이언트 요청을 동시에 처리할 수 없기 때문에 성능이 저하될 수 있음

### NIO(Non-Blocking I/O) Connector

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FmJTm2%2FbtssfRPiyxC%2Fa3XuzahhOKfukvNXmlSSp0%2Fimg.png)

* 톰캣 5부터 지원하며, BIO의 단점인 성능에 대한 이슈를 Poller를 통해 해결한 방식
* Poller는 소켓들을 캐시로 들고 있다가 해당 소켓에서 데이터에 대한 처리가 필요할 때만 쓰레드를 할당함
* 이 방식을 통해 쓰레드가 점유 상태인 채로 아이들(Idle) 상태가 되어 낭비되는 시간을 줄일 수 있음
#### Acceptor
* 소켓 커넥션을 직접적으로 받는 요소로, 소켓에서 채널 객체를 얻어 톰캣의 `NioChannel` 객체로 변환시킴
	* 채널이란 기존의 스트림이 단방향 통신인 것과 반대로 양방향 통신이 가능한 데이터 구조
	* 그래서 스트림의 대체제라고 볼 수 있으며, File Channel, Socket Channel 등 다양하게 존재함
* 그리고 `NioChannel` 객체를 `PollerEvent` 객체로 한번 더 캡슐화하여 `PollerEvent Queue`에 삽입

#### Poller
* `PollerEvent Queue`로부터 `PollerEvent` 객체를 받으며, 단 하나의 쓰레드로 여러 커넥션을 관리함
* NIO Connector는 Selector를 기반하며, Poller 쓰레드 내에 별도의 Selector 객체가 존재함
	* 셀렉터는 이벤트 리스너로, 요청이 들어오면 셀렉터는 메소드를 실행시켜 논블로킹 작업을 처리함
	* 따라서, 단 하나의 쓰레드 만으로도 여러 커넥션을 관리할 수 있음
* 작업 요청을 받은 Poller는 쓰레드 풀에서 이용할 수 있는 Worker 쓰레드를 찾아 소켓을 넘김