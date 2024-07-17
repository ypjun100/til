# Java의 상수 풀
### 상수 풀이 필요한 이유
* Java 코드는 컴파일 과정에서 JVM이 실행할 수 있는 형태인 바이트 코드로 변환됨
* 바이트 코드 내에는 실제 데이터가 저장되기도 하는데, 만약 데이터가 너무 큰 경우 저장이 불가능함
* 따라서, 이런 경우 해당 **데이터는 바이트 코드에 저장하는 것이 아닌 별개의 공간인 `상수 풀`에 저장하게 됨**
* 바이트 코드는 이 상수 풀 내의 데이터를 참조하여 해당 데이터를 이용하게 됨

### constant_pool 테이블
* `.class` 파일의 내부에는 해당 클래스의 데이터를 저장하는 `constant_pool` 테이블이 존재함
* **해당 영역은 클래스마다 별개로 존재하며, 이 테이블을 이용해 런타임 때 실제 상수 풀을 구성하게 됨**
* 테이블 내부에는 숫사 상수, 리터럴 문자열, 클래스 및 메서드 이름 등의 정보가 저장됨
```java
public class ConstantPool {
    public void sayHello() {
        System.out.println("Hello World");
    }
}
```
* 해당 클래스를 컴파일한 뒤에 `javap -v 클래스이름` 명령으로 constant_pool 테이블 출력
```
   #1 = Methodref          #6.#14         // java/lang/Object."<init>":()V
   #2 = Fieldref           #15.#16        // java/lang/System.out:Ljava/io/PrintStream;
   #3 = String             #17            // Hello World
   #4 = Methodref          #18.#19        // java/io/PrintStream.println:(Ljava/lang/String;)V
   #5 = Class              #20            // com/baeldung/jvm/ConstantPool
   #6 = Class              #21            // java/lang/Object
   #7 = Utf8               <init>
   #8 = Utf8               ()V
   #9 = Utf8               Code
  #10 = Utf8               LineNumberTable
  #11 = Utf8               sayHello
  #12 = Utf8               SourceFile
  #13 = Utf8               ConstantPool.java
  #14 = NameAndType        #7:#8          // "<init>":()V
  #15 = Class              #22            // java/lang/System
  #16 = NameAndType        #23:#24        // out:Ljava/io/PrintStream;
  #17 = Utf8               Hello World
  #18 = Class              #25            // java/io/PrintStream
  #19 = NameAndType        #26:#27        // println:(Ljava/lang/String;)V
  #20 = Utf8               com/baeldung/jvm/ConstantPool
  #21 = Utf8               java/lang/Object
  #22 = Utf8               java/lang/System
  #23 = Utf8               out
  #24 = Utf8               Ljava/io/PrintStream;
  #25 = Utf8               java/io/PrintStream
  #26 = Utf8               println
  #27 = Utf8               (Ljava/lang/String;)V
```

* `#n`은 레퍼런스 인덱스로, 이 인덱스를 통해 특정 레퍼런스가 다른 레퍼런스를 참조하고 있는지 확인 가능
* 위 코드에서는 'Hello World!' 문자열이 `#17`에 저장되어 있고, 이를 `#3`에서 참조하고 있음
* 이 테이블을 기반으로 런타임 시점에 `Runtime Constant Pool`을 구성하게 됨

> `#n`의 번호가 1부터 시작하는 이유는 0번 인덱스는 유효하지 않은 인덱스로 간주하기 때문이다.