## Java의 문자열


자바의 Primitive Type과 Reference Type 

문자열은 Reference Type에 속하지만 다른 객체들과 차이점 존재

- 객체를 생성할 때는 new 연산자를 사용
- 문자열은 리터럴을 사용해 바로 값을 할당할 수 있음

```java
String name1 = "hyemin"; // 리터럴로 생성
String name2 = new String("hyemin"); // new 연산자로 생성
```

1. 리터럴 
    - String Constant Pool에 할당
    - 리터럴로 생성한 객체는 String Constant Pool 내의 동일한 문자열 객체를 참조
2. new 연산자 
    - Heap 영역에 할당
    - new 연산자로 생성한 객체는 Heap 영역에 항상 새로운 문자열 객체를 만들고 참조함

### String Constant Pool

- Java7부터 Heap 영역으로 옮겨짐
- Heap영역은 GC의 대상이기 때문에 상수풀에서 참조를 잃은 문자열은 메모리로 반환됨
- 리터럴로 선언시, 이미 존재하는 문자열을 찾아갈 수 있음
- String의 intern() 메소드로 String Constant Pool의 문자열 확인

### String의 intern() 메소드

- String Constant Pool에 생성하려는 문자열이 존재하면 주소값을 반환
- 문자열이 없으면 새로 문자열 객체 생성 후 주소값 반환

```java
public static void main(String[] args) {
    String s1 = "Hello World";
    String s2 = "Hello World";
    String s3 = new String("Hello World");
    System.out.println(s1 == s2); // true
    System.out.println(s2 == s3); // false
}
```

## String은 불변(Immutable)


```java
String name = "Eo";
name += "Hyemin";
```

문자열 리터럴은 불변이기 때문에 thread-safe

String으로 연산을 할 경우, 기존 문자열은 불변이기 때문에 새로운 객체를 만들고 참조하게 됨

기존 문자열은 참조가 사라지게 되어 GC의 대상이 됨

→ GC의 대상이 되는 객체가 많아지면 메모리가 낭비되고 GC의 응답 속도에 영향을 미침

→ GC의 대상이 되는 객체를 최소화 시켜야 함


## 가변성(mutable)을 가지는 StringBuilder, StringBuffer


StringBuffer와 StringBuilder는 내부적으로 동적배열 사용

내부 버퍼에 문자열을 저장해두고, 안에서 추가, 삭제등의 작업을 할 수 있음

자바에서 String연산을 위해 제공하는 객체

String으로 + 연산을 수행하면 내부적으로 StringBuilder를 수행하지만, + 연산만큼 자동으로 변환된 StringBuilder가 GC의 대상이 되기 때문에 메모리에 영향을 미치는것은 동일함

## StringBuilder vs StringBuffer


synchronized 동기화 여부

```java
// StringBuffer
@Override
public synchronized StringBuffer append(String str) {
    toStringCache = null;
    super.append(str);
    return this;
}

// StringBuilder
@Override
public StringBuilder append(String str) {
    super.append(str);
    return this;
}
```

StringBuffer

- 모든 메소드를 synchronized 사용해 동기화하기 때문에 thread-safe
- 속도가 느림

StringBuilder

- 단일 스레드 환경에서만 사용
- 멀티스레드 환경에서 안전하지 못함
