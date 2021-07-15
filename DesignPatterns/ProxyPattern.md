### Proxy Pattern

Proxy Pattern의 목적은 객체에 대한 접근 제어

- Proxy는 대리자,대변인이라는 의미
- Proxy객체를 사용해 실제 객체 메소드를 변경하지 않고 Proxy객체를 사용
- 흐름을 제어할 뿐, 결과값을 조작하거나 변경하지 않음
- Client에서 Proxy객체를 사용하는지 모르게 처리됨

패턴을 도입함으로 인해 이미 짜여져서 작동하고 있는 코드는 변경하지 않는 것


![Untitled](https://user-images.githubusercontent.com/24310798/125798110-c340c428-a165-4c6e-a0b5-9f55b3b8e4bf.png)

Proxy객체

- Proxy는 인터페이스를 구현하고, 실제 객체와 같은 이름의 메소드를 구현함
- Proxy객체는 실제 클래스를 참조 변수로 가짐

Flow

- Client가 해당 객체의 함수를 직접 호출하지 않고 Proxy객체의 메소드를 호출
- Proxy객체에서 실제 사용할 객체를 호출
- 실제 객체에서 반환 받은 값을 Client에게 전달

```java
public interface IService {
	void runSomething();
}
```

```java
public class Service implements IService {
	@Override
	public void runSomething() {
		System.out.println("Service doIt()");
	}
}
```

```java
public class Proxy implements IService {
	public IService service = new Service();

	@Override
	public void runSomething() {
		System.out.println("===========================");
		service.runSomething();
		System.out.println("===========================");
	}
}
```

```java
public class Main {
	public static void main(String[] args) {
		IService service = new Proxy();
		service.runSomething();
	}
}
```

### Proxy usage in Spring

- Transactional
- Caching
