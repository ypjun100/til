# Thread의 interrupt()와 interrupted()

* Thread는 쓰레드를 바로 종료하기 위한 `stop()` 메서드를 제공하지만, deprecated 상태임
	* 쓰레드를 종료할 때 쓰레드가 사용 중인 자원들이 불안전한 상태로 남아있을 수 있기 때문
* 쓰레드를 안전하게 종료하는 방법은 **`interrupt()` 메서드를 사용하여 종료되도록 유도하는 것**
* Thread의 `interrupt()`를 호출하면 해당 스레드에 `InterruptedException` 예외를 발생시킴
	* 이때 중요한 점은 **해당 스레드가 미래에 일시 정지 상태일 때 예외를 발생시킨다는 것**
	* 쓰레드가 현재 일시 정지 상태라도 예외는 발생하지 않고, 미래에 일시 정지 상태가 되면 예외가 발생됨

```java
new Thread() {
	public void run() {
		try {
			while (true)
				System.out.println("실행 중");
		} catch (InterruptedException e) {}

		System.out.println("실행 종료");
	}
}
```

* 만약 위와 같이 작성한다면 `interrupt()`를 실행해도 정상적으로 종료되지 않음
	* 쓰레드가 대기 상태에 들어가지 않고, 계속 실행 중인 상태이기 때문
* 이를 해결하기 위해 while문 안에 `Thread.sleep(1);`를 추가하여 대기 상태로 들어갈 수 있도록 해야 함
* 일시 정지 상태를 만들지 않고도 `interrupt()` 호출 여부를 확인하기 위해서 `interrupted()`를 사용함

```java
new Thread() {
	public void run() {
		try {
			while (!interrupted()) {
				System.out.println("실행 중");
			}
		} catch (InterruptedException e) {}

		System.out.println("실행 종료");
	}
}
```

* `interrupted()`를 사용하여 실행 상태라도 중지 여부를 확인할 수 있음