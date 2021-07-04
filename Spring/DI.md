### 스프링 IoC 컨테이너

---

IoC는 Spring에서만 사용되는 개념이 아니다. 프로그래밍 패턴이고 작게는 객체간의 디자인 패턴, 크게는 컨테이너 프레임워크 역할에 적합한 구조이다.

Spring은 프레임워크로 흐름을 직접 핸들링하기 위한 적합한 패턴을 적용하였고 이게 IoC 와 DI 개념

IoC는 역할과 책임의 분리와 관련이 있다.

객체마다 자신의 책임을 다하며 협력하여 변경에 유연한 프로그래밍을 할 수 있도록 하는 것이 객체지향 프로그래밍이고, 높은 응집도와 낮은 결합도가 핵심이다.

> DI는 오브젝트 레퍼런스를 외부로부터 주입받고 이를 통해 다른 오브젝트와 다이나믹하게 의존 관계가 만들어지는 것이 핵심이다.
주입받는 메소드 파라미터가 특정 클래스 타입으로 고정되어 있다면 DI가 일어날 수 없다.
DI에서 말하는 주입은 다이나믹하게 구현 클래스를 결정해서 제공받을 수 있도록
인터페이스 타입의 파라미터를 통해 이뤄져야 한다.

스프링 IoC 컨테이너

ApplicationContext, BeanFactory

Bean 설정 정보로부터 Bean을 읽어들이고, 제공해주며 각각의 의존 관계를 설정해둔다.

### DI를 통한 IoC 구현 방법

---

- 각 클래스 사이 필요한 의존 관계를 컨테이너가 자동으로 연결해주는 것
- 각 클래스 사이의 의존 관계를 Bean설정 정보를 바탕으로 컨테이너가 자동으로 연결해주는 것

### Constructor Injection

```java
@Service
@Transactional
public class OwnerService {
	private final OwnerRepository ownerRepository; // final

	public OwnerService(OwnerRepository ownerRepository) {
		this.ownerRepository = ownerRepository;
	}
}
```

Bean의 생성자가 하나이며, 생성자의 파라미터 타입이 Bean Container에 등록되어 있다면 

Autowired 어노테이션 없이 의존성 주입이 가능하다.

```java
@Service
@Transactional
@RequiredArgsConstructor
public class OwnerService {
	private final OwnerRepository ownerRepository;
}
```

@RequiredArgsConstructor

- 초기화 되지 않은 final 필드나, @NotNull이 붙은 필드에 대한 생성자 제공
- 의존성 주입 시 편리성 제공

### Setter Injection

```java
@Service
@Transactional
public class OwnerService {
	private OwnerRepository ownerRepository;

	@Autowired
	public void setOwnerRepository(OwnerRepository ownerRepository) {
		this.ownerRepository = ownerRepository;
	}
}
```

### 필드 Injection

```java
@Service
@Transactional
public class OwnerService {
	@Autowired
	private OwnerRepository ownerRepository;
}
```

### 빈 (Bean)

스프링 IoC컨테이너가 관리하는 객체

Bean으로 등록된 객체들만 의존성 주입을 해줌 (@Autowired)

- Component-Scan 을 통해 등록
    1. xml 파일에서 component-scan을 사용하면 base-package에 작성된 패키지에서 빈으로 등록할 수 있는 어노테이션을 찾아서 빈으로 등록해줌
    2. @Component
        - @Repository
        - @Service
        - @Controller
        - @Configuration
- xml이나 자바 설정파일에 직접 등록

### Spring-Boot에서의 Bean 등록

---

Spring Boot에서는 어노테이션을 통해 빈을 설정하고 주입받는 것이 표준이다.

- @Bean은 개발자가 컨트롤 할 수 없는 외부 라이브러리를 빈으로 등록하고 싶은 경우
- @Component는 개발자가 직접 만든 클래스를 빈으로 등록하는 경우
- @Controller, @Service, @Repository는 비즈니스 레이어에 따라 명칭을 다르게 지정한 것
    - Component 어노테이션 사용해서 정의되어 있음

@SpringBootApplication은 ComponentScan 어노테이션을 사용해 정의되어 있다.

따라서 @SpringBootApplication 를 사용한 클래스부터 하위 모든 패키지에 Component 어노테이션을 활용한 클래스들을 빈으로 등록해준다.
