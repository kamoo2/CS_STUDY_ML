# 이재혁

진행 상태: In progress

# JAVA의 컴파일과정

- 자바는 OS에 독립적이다.
  - 그 이유는 JVM이 OS와 프로그램의 사이에서 기계어로 해석해주는 역할을 하기 때문.
  - 어떠한 OS든 JAVA가 설치 되어 있다면 JVM에 의해서 .java코드가 기계어로 해석될 수 있다.
    ![출처 : [https://d2.naver.com/helloworld/1230](https://d2.naver.com/helloworld/1230)](./img/lee5.png)
    출처 : [https://d2.naver.com/helloworld/1230](https://d2.naver.com/helloworld/1230)

1. 개발자가 IDE를 통해 .java파일을 생성한다.
2. Build를 하게되면 Java Compiler(javac)를 통해 .class파일을 생성하게된다.
   - 아직 컴퓨터가 읽을 수 없는 자바 바이트코드.
3. 자바 바이트코드(.class)는 클래스 로더에게 전달된다.
4. 클래스 로더는 동적로딩을 통해 필요한 클래스들을 로딩 및 링크하여 런타임 데이터 영역 즉, JVM 메모리에 올린다.
5. JVM메모리에 올라온 바이트코드는 실행엔진에 의해 기계어로 해석되어 실행된다.

## 런타임 데이터 영역

- JVM은 OS위에서 실행되면서 메모리 영역을 할당 받게되는데 이를 런타임 데이터 영역이라고 한다. 크게 5가지로 세분화 할 수 있다.

![출처 : [https://aljjabaegi.tistory.com/387](https://aljjabaegi.tistory.com/387)](./img/lee1.png)

출처 : [https://aljjabaegi.tistory.com/387](https://aljjabaegi.tistory.com/387)

이 중 PC Register, JVM Stack, Native Method Stack은 스레드 마다 하나씩 생성되고 Heap, Method Aread는 모든 스레드가 공유해서 사용된다.

- PC Register : PC 레지스터는 현재 수행 중인 명령의 주소를 가지며 스레드가 시작될 떄 생성되며 각 스레드마다 하나씩 존재한다.

- JVM 스택 : 스택 프레임이라는 구조체를 저장하는 스택이다. 예외 발생시 printStackTrace() 메서드로 보여주는 Stack Trace의 각 라인 하나가 스택 프레임을 표현한다. JVM 스택 역시 PC레지스터와 마찬가지로 스레드가 시작될 때 생성되며 각 스레드마다 하나씩 존재한다.

- 네이티브 메서드 스택 : JAVA 외의 언어로 작성된 네이티브 코드를 위한 스택이다. JNI를 통해 호출하는 C/C++ 등의 코드를 수행하기 위한 스택으로, 언어에 맞게 스택이 생성된다.

- 힙 : 인스턴스 또는 객체를 저장하는 공간으로 가비지 컬렉션 대상이다. JVM 성능 등의 이슈에서 가장 많이 언급되는 공간이다. 힙 구성 방식이나 가비지 컬렉션 방법 등은 JVM 벤더들의 재량이다.

- 메서드 영역 : 모든 스레드가 공유하는 영역으로 JVM이 시작될 때 생성된다. JVM이 읽어 들인 각각의 클래스와 인터페이스에 대한 런타임 상수 풀, 필드와 메서드에 대한 정보, Static 변수, 메서드의 바이트 코드 등을 보관한다.

- 런타임 상수 풀 : JVM 동작에서 가장 핵심적인 역할을 수행하는 곳으로 JVM 명세에서도 따로 중요하게 기술한다. 각 클래스와 인터페이스의 상수 뿐만 아니라, 메서드와 필드에 대한 모든 레퍼런스까지 담고 있는 테이블로 어떤 메서드나 필드를 참조할 때 JVM은 런타임 상수 풀을 통해 해당 메서드나 필드의 실제 메모리상 주소를 찾아서 참조한다.

## 실행 엔진(Excution Engine)

- 실행 엔진은 클래스 로더를 통해 런타임 데이터 영역에 배친된 바이트 코드를 명령어 단위로 읽어서 실행한다.
- 바이트 코드의 각 명령어는 1바이트 크기의 OpCode와 추가 피연산자로 이루어져 있다.
- 이 수행 과정에서 실행 엔진은 바이트 크드를 기계가 실행할 수 있는 형태로 변경하는데 다음 두 가지 방식으로 변경한다.
  - 인터프리터 : 바`이트 코드 명령어를 하나씩 읽어서 해석하고 실행한다. 하나하나 해석은 빠르지만 전체적인 실행 속도는 느리다는 단점이 있다. JVM안에서 바이트코드는 기본적으로 인터프리터 방식으로 동작한다.
  - JIT 컴파일러 : 인터프리터의 단점을 보완하기 위해 도입된 방식으로 바이트 코드 전체를 컴파일하여 네이티브 코드로 변경하고 이후에는 해당 메서드를 더 이상 인터프리팅 하지 않고 네이티브 코드로 직접 실행하는 방식이다. 하나씩 인터프리팅하여 실행하는 것이 아니라 바이트 코드 전체가 컴파일된 네이티브 코드를 실행하는 것이기 때문에 전체적인 실행 속도는 인터프리팅 방식보다 빠르다.

![출처 : [https://steady-snail.tistory.com/67](https://steady-snail.tistory.com/67)](./img/lee2.png)

출처 : [https://steady-snail.tistory.com/67](https://steady-snail.tistory.com/67)

# Call by value(값에 의한 호출)

- 함수가 호출될 때, 메모리 공간 안에서는 함수를 위한 별도의 임시공간이 생성된다.
- call by value호출 방식은 함수 호출 시 전달되는 변수 값을 복사해서 함수 인자로 전달한다.
- 이때 복사된 인자는 함수 안에서 지역적으로 사용되기 때문에 local value 속성을 가짐
- JAVA의 경우 함수에 전달되는 인자의 데이터 타입에 따라서 함수 호출 방식이 달라진다.
  - 기본 자료형 : call by value
  - 참조 자료형 : call by reference

# Call by reference(참조에 의한 호출)

- 함수가 호출될 때, 메모리 공간 안에서는 함수를 위한 별도의 임시 공간이 생성된다.
- 자바의 경우, 항상 **call by value**로 값을 넘긴다.
- C/C++와 같이 변수의 주소값 자체를 가져올 방법이 없으며, 이를 넘길 수 있는 방법 또한 있지 않다.
- reference type(참조 자료형)을 넘길 시에는 해당 객체의 주소값을 복사하여 이를 가지고 사용한다.
- 따라서 **원본 객체의 프로퍼티까지는 접근이 가능하나, 원본 객체 자체를 변경할 수는 없다.**

# \***\*Primitive type & Reference type\*\***

자바에는 기본형과 참조형이 있다. 일반적인 분류는 다음과 같다.

```
Java Data Type
ㄴ Primitive Type
    ㄴ Boolean Type(boolean)
    ㄴ Numeric Type
        ㄴ Integral Type
            ㄴ Integer Type(short, int, long)
            ㄴ Floating Point Type(float, double)
        ㄴ Character Type(char)
ㄴ Reference Type
    ㄴ Class Type
    ㄴ Interface Type
    ㄴ Array Type
    ㄴ Enum Type
    ㄴ etc.
```

## Primitive type(기본형 타입)

- 자바에서는 총 8가지의 Primitive type을 미리 정의하고 제공한다.
- 자바에서 기본 자료형은 반드시 사용하기 전에 선언 되어야 한다.
- os에 따라 자료형의 길이가 변하지 않는다.
- 비객체 타입이다. 따라서 null값을 가질 수 없다. 만약 Primitive type에 Null을 넣고 싶다면 Wrapper Class를 활용할 수 있다.
- Stack 메모리에 저장된다.

![[https://gyoogle.dev/blog/computer-language/Java/Primitive type & Reference type.html](https://gyoogle.dev/blog/computer-language/Java/Primitive%20type%20&%20Reference%20type.html)](./img/lee3.png)

[https://gyoogle.dev/blog/computer-language/Java/Primitive type & Reference type.html](https://gyoogle.dev/blog/computer-language/Java/Primitive%20type%20&%20Reference%20type.html)

- boolean
  - 논리형인 boolean 기본값은 false이며 참, 거짓을 저장하는 타입이다.
  - 실제로 1bit면 충분하지만, 데이터를 다루는 최소 단위가 1byte이므로 메모리 크기가 1byte이다.
- byte
  - byte는 주로 이진데이터를 다루는데 사용되는 타입이다.
- short
  - C언어와의 호환을 위해 사용되는 타입으로 잘 사용되지 않는다.
- int
  - int형은 자바에서 정수 연산을 하기 위한 기본 타입이다. 즉 byte 혹은 short의 변수가 연산을 하면 연산의 결과는 int형이 된다.
- long
  - 수치가 큰 데이터를 다루는 프로그램에서 주로 사용한다.
  - long 타입의 변수를 초기화 할 때는 정수값 뒤에 알파벳 L을 붙여서 long타입의 정수 데이터임을 알려주어야 한다. 만일 정수값이 int의 값의 저장 범위를 넘는 정수에서 L을 붙이지 않는다면 컴파일 에러가 발생한다.
- float, double
  - 실수를 가수와 지수 형식으로 저장하는 부동소수점 방식으로 저장된다.
  - 가수를 표현하는데 있어 double형이 float형보다 표현 가능 범위가 더 크므로 double형이 보다 정밀하게 표현할 수 있다.
  - 자바에서 실수의 기본 타입은 double형이므로 float형에는 알파벳 F를 붙여서 float 형임을 명시해주어야 한다.

## Reference type(참조형 타입)

- 자바에서 Primitive 타입을 제외한 모든 타입은 Reference 타입이다.
- Reference 타입은 JAVA에서 최상인 java.lang.Object클래스를 상속하는 모든 클래스들을 말한다. 물론 new로 인하여 생성하는 것들은 메모리 영역인 Heap 영역에 생성하게되고, GC가 돌면서 메모리를 해제한다.
- 클래스 타입, 인터페이스 타입, 배열 타입, 열거 타입이 있다.
- 빈 객체를 의미하는 NULL이 존재한다.
- 문법상으로는 에러가 없지만 실행시켰을 때 생기는 런타입 에러가 발생한다. 예를 들어 객체나 배열을 NULL 값으로 받으면 NullPointException이 발생하므로 변수값을 넣어야 한다.
- Heap 메모리에 생성된 인스턴스는 메서드나 각종 인터페이스에 접근하기 위해 JVM의 Stack 영역에 존재하는 Fram에 일종의 포인터인 참조값을 가지고 있어 이를 통해 인스턴스를 핸들링한다.

## String Class

- 클래스 형에서도 String클래스는 조금 특별하다. 이 클래스는 참조형에 속하지만 기본형 처럼 사용한다.
- 불변하는 객체이다.
- String 클래스에는 값을 변경해주는 메서드들이 존재하지만 해당 메서드를 통해 데이터를 바꾼다 해도 새로운 String 클래스 객체를 만들어내는 것이다. 일반적으로 기본형 비교는 == 연산자를 사용하지만 String 객체간의 비교는 .equals() 메서드를 사용해야 한다.

# Auto boxing & Auto unboxing

자바에는 기본 타입과 Wrapper 클래스가 존재한다.

- 기본 타입 : int, long, float, double, boolean 등
- Wrapper 클래스 : Integer, Long, Float, Double, Boolean 등

### Wrapper 클래스란?

- 자바의 자료형은 크게 기본타입과 참조타입으로 나누어짐
- 프로그래밍을 하다 보면 기본타입의 데이터를 객체로 표현해야 하는 경우가 종종 있다.
- 이럴 때에 기본타입을 객체로 다루기 위해 사용하는 클래스는 Wrapper 클래스라고한다.
- 래퍼 클래스의 주요 용도는 기본 타입의 값을 박싱 해서 포장 객체로 만드는 것이지만, 문자열을 기본 타입 값으로 변환할 때에도 사용된다.
  대부분의 래퍼 클래스에는 parse + 기본 타입명으로 되어있는 정적 메서드가 있다.
  이 메서드는 문자열을 매개 값으로 받아 기본 타입 값으로 변환한다.
- 래퍼 객체는 내부의 값을 비교하기 위해 == 연산자를 사용할 수 없다.
  이 연산자는 내부의 값을 비교하는 것이 아니라 래퍼 객체의 참조 주소를 비교하기 때문이다.
- 래퍼 클래스와 기본자료형과의 비교는 == 연산과 equals연산 모두 가능하다.
  그 이유는 컴파일러가 자동으로 오토박싱과 언박싱을 해주기 때문이다.

### Boxing 과 Unboxing

![[https://coding-factory.tistory.com/547](https://coding-factory.tistory.com/547)](./img/lee4.png)

[https://coding-factory.tistory.com/547](https://coding-factory.tistory.com/547)

- 기본 타입의 값을 포장 객체로 만드는 과정을 박싱이라고함.
- 반대로 포장객체에서 기본 타입의 값을 얻어내는 과정을 언박싱이라고한다.

### AutoBoxing 과 AutoUnboxing

- 기본타입 값을 직접 박싱, 언박싱 하지 않아도 자동으로 박싱과 언박싱이 일어나는 경우가 있다.
- 자동 박싱의 포장 클래스 타입에 기본값이 대입될 경우에 발생한다.
- 예를 들어 int타입의 값을 Integer 클래스 변수에 대입하면 자동 박싱이 일어나 힙 영역에 Integer객체가 생성된다.

# Serialization(직렬화)

- 자바 시스템 내부에서 사용되는 객체 또는 데이터를 외부의 자바 시스템에서도 사용할 수 있도록 바이트 형태로 데이터를 변환하는 기술

- 각자 PC의 OS마다 서로 다른 가상 메모리 주소 공간을 갖기 때문에, Reference Type의 데이터들은 인스턴스를 전달 할 수 없다.

- 따라서, 이런 문제를 해결하기 위해선 주소값이 아닌 Byte형태로 직렬화된 객체 데이터를 전달해야 한다.

- 직렬화된 데이터들은 모두 기본형이 되고, 이는 파일 저장이나 네트워크 전송 시 파싱이 가능한 유의미한 데이터가 된다. 따라서 전송 및 저장이 가능한 데이터로 만들어 주는 것이 바로 직렬화이다.

### 직렬화 조건

- 자바에서는 간단히 java.io.Serializable 인터페이스 구현으로 직렬화/역직렬화가 가능하다.
- 직렬화 대상
  - 인터페이스 상속 받은 객체,
  - Primitive 타입의 데이터
- Primitive 타입이 아닌 Reference 타입처럼 주소값을 지닌 객체들은 바이트로 변환하기 위해 Serializable 인터페이스를 구현해야 한다.
- 직렬화 대상에서 제외하고싶은 필드는 **transient** 예약어를 사용하여 제외할 수 있다.

### 직렬화 방법

- java.io.ObjectOutputStream 객체를 이용한다.

```java
Member member = new Member("홍길동", "hong@hong.com", 25);
    byte[] serializedMember;
    try (ByteArrayOutputStream baos = new ByteArrayOutputStream()) {
        try (ObjectOutputStream oos = new ObjectOutputStream(baos)) {
            oos.writeObject(member);
            // serializedMember -> 직렬화된 member 객체
            serializedMember = baos.toByteArray();
        }
    }
    // 바이트 배열로 생성된 직렬화 데이터를 base64로 변환
    System.out.println(Base64.getEncoder().encodeToString(serializedMember));
}
```

### 역직렬화 조건

- 직렬화 대상이 된 객체의 클래스가 클래스 패스에 존재해야 하며 import 되어 있어야 한다.
  - 중요한 점은 직렬화와 역직렬화를 진행하는 시스템이 서로 다를 수 있다는 것을 **반드시** 고려해야 한다.

<aside>
💡 자바 직렬화 대상 객체는 동일한 serialVersionUID를 가지고 있어야 한다.

- 필수는 아님!
</aside>

### 자바의 직렬화 왜 사용될까?

- CSV, JSON 프로토콜 버퍼 등은 시스템의 고유 특성과 상관없는 대부분의 시스템에서의 데이터 교환 시 많이 사용된다. 하지만 자바 직렬화 형태의 데이터 교환은 자바 시스템 간의 데이터 교환을 위해서 존재한다.

### 그렇다면 자바에서도 CSV, JSON을 사용하면 되지 자바 직렬화를 써야 되는 이유가 있을까?

- 정답은 없지만 목적에 따라 적절하게 써야한다.
- 직렬화의 장점
  - 자바 시스템에서 개발에 최적화 되어 있다.
  - 복잡한 데이터 구조의 클래스의 객체라도 직렬화 기본 조건만 지키면 큰 작업 없이 바로 직렬화가 가능하다.
  - 데이터 타입이 자동으로 맞춰진다.
- 직렬화의 단점
  - 변경에 취약하기 때문에 예외사항이 발생할 가능성이 높다.
  - 다른 포맷에 비해서 용량이 크다.

### 자바 직렬화는 언제 어디서 사용될까?

- 서블릿 세션
  - 서블릿 기반의 WAS들은 대부분 세션의 자바 직렬화를 지원하고 있다.
    물론 단순히 세션을 서블릿 메모리 위에서 운용한다면 직렬화를 필요로 하지 않지만
    파일로 저장하거나 세션 클러스터링, DB를 저장하는 옵션 등을 선택하게 되면 세션 자체가 직렬화 되어 저장되어 전달된다.
- 캐시
  - 자바 시스템에서 퍼포먼스를 위해 캐시 라이브러리 시스템을 많이 이용하게 된다.
    개발을 하다보면 상당수의 클래스가 만들어지게 된다
    예를들어 DB를 조회한 후 가져온 데이터 객체 같은 경우 실시간 형태로 요구하는 데이터가 아니라면
    메모리, 외부 저장소, 파일 등을 저장소를 이용해서 데이터 객체를 저장한 후 동일한 요청이 오면 DB를 다시 요청하는 것이 아니라 저장된 객체를 찾아서 응답하게 하는 형태를 보통 캐시를 사용한다고 한다.
  - 이렇게 캐시할 부분을 직렬화하여 저장해서 사용한다. 자바 직렬화만을 이용해서 캐시를 저장하지는 않지만 가장 간편하기 때문에 많이 사용된다.
