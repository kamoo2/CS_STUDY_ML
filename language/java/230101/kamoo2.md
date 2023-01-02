# 문석환

## 목차

- 문자열 클래스
- 오브젝트 클래스
- 캐스팅
- 스레드

## 1. 문자열 클래스

| 분류   | String    | StringBuffer                     | StringBuilder       |
| ------ | --------- | -------------------------------- | ------------------- |
| 변경   | Immutable | Mutable                          | Mutable             |
| 동기화 |           | Synchronized 가능 (Thread -safe) | Synchronized 불가능 |

### String 특징

- new 연산을 통해 생성된 인스턴스의 메모리 공간은 변하지 않는다. (Immutable)
- Garbage Collector로 제거되어야 함
- 문자열 연산시 새로 객체를 만드는 Overhead 발생
- 객체가 불변하므로, Multithread에서 동기화를 신경 쓸 필요가 없음 (조회 연산에 매우 큰 장점)

String 클래스 : 문자열 연산이 적고, 조회가 많은 멀티쓰레드 환경에서 좋음

### StringBuffer, StringBuilder 특징

- 공통점
  - new 연산으로 클래스는 한 번만 만듬 (Mutable)
  - 문자열 연산시 새로 객체를 만들지 않고, 크기를 변경시킴
  - StringBuffer 와 StringBuilder 클래스의 메서드가 동일
- 차이점
  - StringBuffer는 Thread-Safe / StringBuilder는 Thread-Safe 하지 않음 (불가능)

StringBuffer 클래스 : 문자열 연산이 많은 Multi-Thread 환경

StringBuilder 클래스 : 문자열 연산이 많은 Single-Thread 또는 Thread를 신경 쓰지 않는 환경

### Thread-Safe

쓰레드 안전은 멀티 쓰레드 프로개르맹에서 일반적으로 어떤 함수나 변수, 혹은 객체가 여러 쓰레드로부터 동시에 접근이 이루어져도 프로그램의 실행에 문제가 없음을 뜻한다.

예를들어 하나의 함수가 한 쓰레드로부터 호출되어 실행 중일 때, 다른 쓰레드가 그 함수를 호출하여 동시에 함께 실행되더라도 각 쓰레드에서의 함수의 수행 결과가 올바르게 나오는 것

### Thread-Safe의 여부를 판단하는 방법

- 전역 변수나 힙, 파일과 같이 여러 스레드가 동시에 접근할 수 있는 자원을 사용하는가 ?
- 핸들과 포인터를 통한 데이터의 간접 접근이 가능한가 ?
- 부수 효과를 가져오는 코드가 있는가 ?

### Thread-Safe를 지키기 위한 방법 4가지

1. Mutual Exclustion ( 상호 배제)
   - 공유자원에 하나의 Thread만 접근 할 수 있또록, 세마포어/뮤텍스로 락을 통제하는 방법
     - 일반적으로 많이 사용되는 방식
   - 적용 예제
     - Python은 Thread Safe하게 메모리를 관리 하지 않으므로 GIL(Global Interpreter Lock)을 사용해 Thread Safe를 보장
2. Atomic operation (원자 연산)
   - 공유자원에 원자적으로 접근하는 방법
   - Atomic
     - 공유 자원 변경에 필요한 연산을 원자적으로 분리한 뒤,
     - 실제로 데이터의 변경이 이루어지는 시점에 Lock을 걸고,
     - 데이터를 변경하는 시간 동안, 다른 쓰레드의 접근이 불가능 하도록 하는 방법입니다.
3. Thread-local storage (쓰레드 지역 저장소)
   - 공유 자원의 사용을 최대한 줄이고, 각각의 쓰레드에서만 접근 가능한 저장소들을 사용함으로써 동시 접근을 막는 방법
   - 일반적으로 공유 상태를 피할 수 없을때 사용하는 방식
4. Re-entrancy (재진입성)
   - 쓰레드 호출과 상관 없이 프로그램에 문제가 없도록 작성하는 방법

## 2. Object 클래스 wait, notify, notifyAll

### Object Class가 갖고 있는 메서드

- toString()
- hashCode()
- wait()
  - 갖고 있는 고유 lock 해제, Thread를 잠들게 함
- notify()
  - 잠들던 Thread 중 임의의 하나를 깨움
- notifyAll()
  - 잠들어 있는 Thread를 모두 깨움

wait, notify, notifyAll : 호출하는 스레드가 반드시 고유 락을 가지고 있어야 함

- Synchronized 블록 내에서 실행되어야 함
- 그 블록 안에서 호출하는 경우 IllegalMonitorStateException 발생

## 3. Casting (업캐스팅 & 다운 캐스팅)

### 캐스팅이란 ?

변수가 원하는 정보를 다 갖고 있는 것

```java
int a = 0.1; // 에러 발생 X
int b = (int) true; // 에러 발생 O, boolean은 int로 캐스트 불가
```

- 0.1이 double형이지만 , int가 될 정보 또한 가지고 있다.
- true는 int형이 될 정보를 가지고 있지 않다.

### 캐스팅이 필요한 이유

1. 다형성 : 오버라이딩된 함수를 분리해서 활용할 수 있다.
2. 상속 : 캐스팅을 통해 범용적인 프로그래밍이 가능하다.

### 형변환의 종류

1. 묵시적 형변환 : 캐스팅이 자동으로 발생 (업캐스팅)

```java
Parent p = new Child(); // (Parent) new Child() 할 필요가 없다.
```

→ Parent를 상속받은 Child를 Parent의 속성을 포함하고 있기 때문

1. 명시적 형변환 : 캐스팅할 내용을 적어줘야 하는 경우 (다운캐스팅)

```java
Parent p = new Child();
Child c = (Child) p;
```

→ 다운 캐스팅은 업 캐스팅이 발생한 이후에 작용한다.

### 예시 문제

```java
class Parent {
 int age;

 Parent() {}

 Parent(int age) {
  this.age = age;
 }

 void printInfo() {
  System.out.println("Parent Call!!!!");
 }
}

class Child extends Parent {
 String name;

 Child() {}

 Child(int age, String name) {
  super(age);
  this.name = name;
 }

 @Override
 void printInfo() {
  System.out.println("Child Call!!!!");
 }

}

public class test {
    public static void main(String[] args) {
        Parent p = new Child();

        p.printInfo(); // 문제1 : 출력 결과는?
        Child c = (Child) new Parent(); //문제2 : 에러 종류는?
    }
}
```

### 문제 1 : Child Call

자바에서는 오버라이딩된 함수를 동적 바인딩 하기 때문에, Parent에 담겼어도 Child의 printInfo() 함수를 불러오게 된다.

### 문제 2 : Runtime Error

컴파일 과정에서는 데이터형의 일치만 따진다.

프로그래머가 따로 (Child)로 형변환을 해줬기 때문에 컴파일러는 문법이 맞다고 생각해서 넘어간다.

하지만 런타임 과정에서 Child 클래스에 Parent 클래스를 넣을 수 없다는 것을 알게 되고
런타임 에러가 발생 하게 된다.

## 4. Thread

- 요즘 OS는 모두 멀티태스킹 지원

### 🐯 멀티태스킹이란

- 컴퓨터로 음악을 들으면서 웹서핑을 하는 것
- 쉽게 말해 두 가지 이상의 작업을 동시에 하는 것

실제로 동시에 처리될 수 있는 프로세스의 개수는 CPU 코어의 개수와 동일한데, 이보다 많은 개수의 프로세스가 존재하기 때문에 모두 함께 동시에 처리할 수 없다.

각 코어들은 아주 짧은 시간동안 여러 프로세스를 번갈아가며 처리하는 방식을 통해 동시에 동작하는 것처럼 보이게 할 뿐이다.

이와 같이 멀티스레딩이란 하나의 프로세스 안에 여러개의 스레드가 동시에 작업을 수행하는 것을 말한다.

스레드는 하나의 작업 단위라고 생각하면 편하다.

### 스레드 구현

1. Runnable 인터페이스 구현
2. Thread 클래스 상속

→ 둘다 run() 메소드를 오버라이딩 하는 방식이다.

```java
public class MyThread implements Runnable {
    @Override
    public void run() {
        // 수행 코드
    }
}
```

```java
public class MyThread extends Thread {
    @Override
    public void run() {
        // 수행 코드
    }
}
```

### 스레드 생성

두 방법도 차이점이 있는데 인스턴스 생성 방법에 차이가 있다.

Runnable 인터페이스를 구현한 경우는, 해당 클래스를 인스턴스화해서 Thread 생성자에 argument로 넘겨줘야 한다.

그리고 run()을 호출하면 Runnable 인터페이스에서 구현한 run()이 호출되므로 따로 오버라이딩 하지 않아도 되는 장점이 있다.

```java
public static void main(String[] args) {
    Runnable r = new MyThread();
    Thread t = new Thread(r, "mythread");
}
```

Thread 클래스를 상속받은 경우는, 상속받은 클래스 자체를 스레드로 사용할 수 있다.

또, Thread 클래스를 상속받으면 스레드 클래스 메소드(getName())를 바로 사용 할 수 있지만, Runnable의 경우 Thread 클래스의 static 메소드인 currentThread()를 호출하여 현재 스레드에 대한 참조를 얻어와야만 호출 가능

```java
public class ThreadTest implements Runnable {
    public ThreadTest() {}

    public ThreadTest(String name){
        Thread t = new Thread(this, name);
        t.start();
    }

    @Override
    public void run() {
        for(int i = 0; i <= 50; i++) {
            System.out.print(i + ":" + Thread.currentThread().getName() + " ");
            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}
```

### 스레드 실행

스레드의 실행은 run() 호출이 아닌 start() 호출로 해야한다.

WHY ?

- 우리는 run으로 메소드를 정의했지만, 실제 스레드 작업을 시키려면 start()로 작업해야 한다.
- run()으로 작업 지시를 하면 스레드가 일을 하지 않을까 ?
  - ❌ 두 메소드 모두 같은 작업을 한다.
  - 하지만 run() 메소드를 사용하면, 이건 스레드를 사용하는 것이 아니다.
- Java에는 콜스택이 존재 , 이 영역이 실질적인 명령어들을 담고 있는 메모리로, 하나씩 꺼내서 실행시킨다.
- 만약 동시에 두 가지 작업을 한다면, 두 개 이상의 콜 스택이 필요함
- 스레드를 이용한다는 건, JVM이 다수의 콜 스택을 번갈아 가며 일처리를 하고 사용자는 동시에 작업하는 것처럼 보여준다.
- 즉, run() 메소드를 이용하는 것은 main()의 콜스택 하나만 이용 하는 것으로 스레드 활용이 아니다.
- start() 메소드를 호출하면, JVM은 알아서 스레드를 위한 콜 스텍을 새로 만들어 주고 context switching을 통해 스레드답게 동작하도록 해준다.
- 우리는 새로운 콜스택을 만들어 작업을 해야 스레드 일처리가 되는 것이기 때문에 start() 메소드를 써야한다.

> start()는 스레드가 작업을 실행하는데 필요한 콜 스택을 생성한 다음 run()을 호출해서 그 스택 안에 run()을 저장 한다.
