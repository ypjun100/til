# Fork & Join 프레임워크

* 멀티쓰레드를 위해 한 작업을 여러 개로 나눠서 여러 쓰레드가 동시에 처리하는 것을 쉽게 하는 프레임워크
	* Fork는 작업을 나누는 것이고, Join은 일을 다시 합치는 의미로 사용됨
* 자바에만 국한된 용어는 아니며, **병렬 처리를 위해 분할 정복 알고리즘을 통해 재귀적으로 처리됨**
* 수행할 작업에 따라 두 개의 클래스 중에서 하나를 상속받아 구현할 수 있음
	* `RecursiveAction`: 반환값이 없는 작업을 구현할 때 사용
	* `RecursiveTask`: 반환값이 있는 작업을 구현할 때 사용

### Work Stealing
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FHalmi%2FbtrvMeBCEu8%2FtKTEnVVJRQGQC3kkPpLKmK%2Fimg.png)

* 작업을 위한 작업 큐 중에서 특정 큐에 작업이 많은 경우, 작업 중 일부를 다른 디큐에게 이관하는 행위
* Fork & Join 프레임워크에는 Work Stealing이 포함되어 있어, 자동으로 쓰레드 간 작업을 이관함
	* 해야할 작업이 많은 쓰레드의 작업 중 일부를 자동으로 다른 쓰레드의 작업 큐로 이관함

### 구현
```java
import java.util.*;
import java.util.stream.*;
import java.util.concurrent.*;

class HelloWorld {
    static final ForkJoinPool pool = new ForkJoinPool();
    
    static class Summation extends RecursiveTask<Long> {
        long from, to;
        
        public Summation(long from, long to) {
            this.from = from;
            this.to = to;
        }
        
        @Override
        public Long compute() {
	        // 작업이 3 이하인 경우 누적합 연산 수행
            if (to - from <= 3) {
                return LongStream.range(from, to + 1)
                                .reduce(0, (a, b) -> a + b);
            }
            
            // 작업이 많은 경우 분할 수행
            long middle = (from + to) / 2;
            Summation left = new Summation(from, middle);
            Summation right = new Summation(middle + 1, to);
            
            left.fork();
            
            // left.join()은 동기 메서드이기 때문에 호출 시 해당 쓰레드가 완료될 때까지 기다림
            // 따라서, 다른 쓰레드가 작업을 수행할 동안 right를 먼저 수행함
            return right.compute() + left.join();
        }
    }
    
    public static void main(String[] args) {
        Summation sum = new Summation(1, 10);
        
        System.out.println(pool.invoke(sum));
    }
}
```

* `from`부터 `to`까지의 누적합을 계산하는 예제
* 작업을 수행하기 위해서는 먼저 `ForkJoinPool` 인스턴스를 생성하여 쓰레드 풀을 생성해야 함
* `RecursiveTask`를 상속받은 후 `compute()` 메서드를 오버라이딩하여 작업을 명시함
* 쓰레드풀의 `invoke()` 메서드를 사용하여 작업을 시작할 수 있음
* `fork()` 메서드로 특정 작업을 쓰레드 풀의 작업 큐에 넣을 수 있음
* `join()` 메서드는 특정 작업의 수행이 끝날 때까지 대기한 후, 수행이 종료되면 그 결과를 반환함

### 정리
* 작업을 분할하고 합치는 데 걸리는 시간이 있기 때문에 무조건 멀티쓰레드로 처리하는 것이 좋은게 아님
* 하지만 내부적으로 Fork & Join으로 구현되어 있는 클래스들이 많기 때문에 알아두는 것이 좋음
	* 그 예시로 `parallel()` 메서드를 사용하는 병렬 스트림이 있음
