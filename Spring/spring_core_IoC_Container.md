<https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#spring-core>

# IoC Container and Beans


IoC(Inversion of Control) 이라고 알려진 스프링 프레임워크의 구현방법에 대해 다룬다.

IoC는 DI(Dependency Injection)이라고도 알려져 있다.

스프링 컨테이너는 Bean을 생성할 때 필요한 의존성을 모두 주입한다.

이 과정에서 Bean은 자신의 생성이나 의존성에 대한 컨트롤을 하지 않으므로 제어의 역전인 것이다.

`org.springframework.beans`

`org.springframework.context`

두 패키지는 스프링 프레임워크의 IoC컨테이너의 기본 패키지이다.

BeanFactory 인터페이스는 모든 유형의 객체를 관리할 수 있는 매커니즘을 제공하고, ApplicationContext은 BeanFactory의 서브타입으로, 다음의 기능을 포함한다.

- Spring AOP 기능과의 통합
- Message Resource 핸들링
- 이벤트 발행
- `WebApplicationContext`와 같이, 웹 어플리케이션에서 사용하기 위한 특정 계층 컨텍스트

스프링 IoC 컨테이너에 의해 생성되고 관리되는 객체를 Bean이라고 부른다. Bean과 그들의 의존성은 컨테이너에서 사용하는 설정 메타데이터에 반영된다.

# Container


`org.springframework.context.ApplicationContext` 인터페이스는 스프링 IoC 컨테이너를 나타내며, Bean의 생성과 설정 및 조립을 담당한다.

컨테이너는 설정 메타데이터를 읽어 객체 생성, 구성, 조립에 대한 정보를 얻는다.

설정 메타데이터는 XML, Java annotaions, Java 코드로 표현될 수 있다. 

애플리케이션은 컨텍스트가 생성 및 초기화된 후, 설정 메타데이터와 결합하여 실행 가능한 시스템이 된다.

![image](https://user-images.githubusercontent.com/24310798/127174266-13bf1785-1f0a-496b-9bcc-63174ca1c979.png)

## Configuration Metadata


설정 메타데이터는 애플리케이션 개발자로서 스프링 컨테이너에게 객체를 생성하고 구성하는 방법을 나타낸다.

- XML
- annotation-based (Spring 2.5)
- Java-based (Spring 3.0)
    - `@Configuration` `@Bean` `@Import` `@DependsOn`

## Bean


스프링 컨테이너는 하나 이상의 빈을 관리한다. `@Bean`, `<bean>` 당 각각 하나의 메타 정보가 생성되는데, 스프링 컨테이너는 이 메타정보를 기반으로 빈을 생성한다.

컨테이너 내에서 빈의 정의는 `BeanDefinition` 객체로 표현되며 다음과 같은 메타데이터를 포함한다.

- 패키지 기반 클래스 이름
- Bean 동작 구성 요소 (scope, lifecycle callbacks 등)
- 다른 Bean에 대한 참조, 의존성
- 새로 생성할 객체에 설정할 값

# Dependencies


엔터프라이즈급 애플리케이션은 하나의 객체로만 구성되지 않는다. 간단한 애플리케이션도 여러 객체들의 협업을 통해 이루어진다. 

## Dependency Injection

의존성 주입(DI)는 생성자, 팩토리 메소드, 인스턴스가 생성된 후 설정되는 속성에 의해서만 종속성을 정의하는 프로세스다.

컨테이너는 빈을 생성할 때 이러한 의존성을 주입해준다.

- Constructor-based Dependency Injection
- Setter-based Dependency Injection
