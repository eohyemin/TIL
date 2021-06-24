# Lambda와 Functional Interface

Java 8의 핵심은 함수형 프로그래밍이라는 패러다임을 Java 개발에 접목시키는 것. 

## 함수형 프로그래밍

함수형 프로그래밍이란 함수의 입력만을 의존하여 출력을 만드는 구조로 외부의 상태를 변경하는 것을 지양하는 패러다임

*다음 3가지 조건을 만족해야함*

1. 순수 함수 (Pure Function)

   Side Effect가 없는 함수로, 함수의 실행이 외부 상태를 변경시키지 않는 함수

   오직 입력에 의해서만 출력이 정해지고, 환경에 영향을 받지 않아야 함

2. 익명 함수 (Annonymous Function)

   이름이 없는 함수를 정의할 수 있어야 함. 람다 표현식으로 구현 가능

3. 고차 함수 (Higher-Order Function)

   → 함수를 일급 객체(First Class Object)로 사용 가능

   함수를 매개변수로 받을 수 있고, 함수를 리턴할 수 있음

   - 일급 객체

Java 8에서 함수형 인터페이스라는 개념을 도입하면서, 함수형 인터페이스의 경우 람다식으로 표현 가능하도록 제공되었으며 함수형 프로그래밍 언어의 조건을 만족하게 됨

## 람다 표현식 (Lambda Expressions)

* 함수형 인터페이스의 인스턴스를 만드는 방법으로 사용 가능

- 익명함수를 간단하게 구현해서, 코드가 간결해짐
- 메소드 파라미터, 리턴값, 변수로 사용가능함

```java
// interface DoSomething
@FunctionalInterface
public interface DoSomething {
	void doIt();
}

```

```java
// class Foo
public class Foo {
	public static void main(String[] args) {
		// 익명클래스
		RunSomething runSomething = new RunSomething() {
			@Override
			public void doIt() {
				System.out.println("DO~~");
			}
		};
		// 람다
		RunSomething runSomething2 = () -> System.out.println("DO~~");
		runSomething2.doIt(); //호출
	}
}
```

() -> System.out.println("DO") 자체를 메소드 파라미터로 전달하거나 리턴값 가능 → 일급객체

```java
/*
* 람다 사용할 수 없는 경우 
* 1. 함수 안에서 함수 밖의 값을 참조할때
* 2. 외부의 값을 변경하려고 할 경우
* 같은 input에 대해 같은 결과를 보장할 수 없을때는 사용 XX
*/
// class Foo
public class Foo {
	public static void main(String[] args) {
		int baseNumber = 10;
		RunSomething runSomething = new RunSomething() {
			// int baseNumber = 10;
			@Override
			public int doIt(int number) {
				return number + baseNumber;
			}
		};
	}
}
```

## 함수형 인터페이스 (Functional Interface)

함수형 인터페이스란 추상 메소드를 딱 하나만 가지고 있는 인터페이스를 말한다

람다 표현식으로 구현이 가능한 인터페이스는 오직 추상메소드가 1개인 인터페이스뿐

```java
@FunctionalInterface
public interface Operator {
	public int operate(int x, int y);
}
```

### @FunctionalInterface

@FunctionalInterface를 사용해 해당 인터페이스가 함수형 인터페이스라는 것을 명시함

다른 메소드를 추가하거나 다른 인터페이스를 상속시 컴파일 에러 발생시켜 미연에 방지

함수형 인터페이스를 따로 구현하지 않더라도, java.util.function 패키지의 Java에서 기본 제공하는 함수형 인터페이스 사용하는 것 권장

### Java에서 기본으로 제공하는 함수형 인터페이스

java.util.function 패키지

- Function<T, R> - R apply(T t)

  T 값을 받아서 R 값을 리턴함 (두개의 타입은 다를 수 있음)

  ```java
  import java.util.function.Function;
  public class Foo {
  	public static void main(String[] args) {
  		Function<Integer, Integer> plus10 = (i) -> i + 10;
  		Function<Integer, Integer> multiply2 = (i) -> i * 2;
  		System.out.println(plus10.apply(1));
  
  		// 조합할 수 있는 default 메소드 제공
  		plus10.compose(multiply2); // 입력값을 가지고 먼저 적용 후 그 결과값을 적용
  		plus10.andThen(multiply2); // plus10 이후 multiply2
  	}
  }
  ```

- Consumer<T> - void accept(T t)

  T 타입의 값을 받아서 리턴값은 없음

  ```java
  import java.util.function.Consumer;
  public class Foo {
  	public static void main(String[] args) {
  		Consumer<Integer> printT = (i) -> System.out.println(i);
  		printT.accept(10);
  	}
  }
  ```

- Supplier<T> - T get()

  T 타입의 값을 제공하는 함수 인터페이스

  ```java
  import java.util.function.Supplier;
  public class Foo {
  	public static void main(String[] args) {
  		Supplier<Integer> get10 = () -> 10;
  		System.out.println(get10);
  	}
  }
  ```

- Predicate<T t>

  인자를 하나 받아서 true/false 리턴함, and or 로 조합 가능

  ```java
  import java.util.function.Predicate;
  public class Foo {
  	public static void main(String[] args) {
  		Predicate<String> startWithHyemin = (s) -> s.startsWith("hyemin");
  		startWithHyemin.test("");
  	}
  }
  ```
