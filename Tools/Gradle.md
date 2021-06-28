## gradle이란

---

- JVM에서 동작하는 스크립트 언어인 Groovy언어를 기반으로 만들어진 빌드 도구
- 명령에 의해 task를 수행하는 프로그램
    - task는 사용자가 정의할 수 있으며 build.gradle에 task를 기술해두면 gradle 명령어로 호출해 실행 가능

## gradle init 명령과 type 종류

---

1. basic 
    - src가 제공되지 않는 기본 타입
    - build.gradle과 settings.gradle 빌드 파일만 생성됨
2. application - java
    - Java 애플리케이션 프로젝트 작성 타입
    - 기본적으로 [App.java](http://app.java)가 제공됨
3. library - java
4. gradle plugin

## gradle wrapper

---

gradle wrapper를 사용하면 해당 프로젝트에 종속된 gradle 버전으로 빌드할 수 있음

wrapper를 사용하지 않으면 프로젝트 버전이 다를때 프로젝트 버전과 일치하도록 수동으로 설치해주거나, 빌드 환경에 따른 다른 빌드 결과를 만들 수 있음

`gradlew` 명령어로 프로젝트를 빌드할 때 gradle-wrapper.jar 를 참조해 설정 파일 구성

```groovy
./gradlew build
```

- gradlew 란

    gradle wrapper를 사용하면 해당 프로젝트에 종속된 gradle 버전으로 빌드할 수 있음

    wrapper를 사용하지 않으면 프로젝트 버전이 다를때 프로젝트 버전과 일치하도록 수동으로 설치해주거나, 빌드 환경에 따른 다른 빌드 결과를 만들 수 있음

    `gradlew` 명령어로 프로젝트를 빌드할 때 gradle-wrapper.jar 를 참조해 설정 파일 구성

## gradle 프로젝트 기본 설정

---

build.gradle

- 프로젝트 빌드에 대한 기능 정의
- 프로젝트 의존성, task를 정의할 때 사용

```groovy
buildscript {
// gradle build 스크립트 자체를 의존성, task, plugin 등 지정
// build.gradle 자체를 실행하기 위한 설정
	
}

plugins {
	id 'java'
	// id 'org.springframework.boot' version ''
	// id 'io.spring.dependency-management' version '' - 스프링 의존성을 관리해주는 플러그인
}
dependencies { 
	/* 프로젝트 개발에 필요한 의존성 선언 */
	/* 특정 버전을 명시하지 않아야 위에서 설정한 버전을 따라감 -> 버전 충돌문제*/
	// implementation - 컴파일시 의존하는 라이브러리
	// testImplementation - 단위테스트에 사용하는 라이브러리
	// classpath - 컴파일부터 실행까지 의존하는 라이브러리
}

repositories { // 의존성(라이브러리)들을 어떤 원격 저장소에서 받을지 설정
	// jcenter()
	// mavenCentral()
}
```

init.gradle

- 초기화 스크립트
- build시 가장 먼저 실행되는 스크립트 파일 - 실행환경 초기화 등

settings.gradle

- 멀티 프로젝트의 경우, 빌드 대상 프로젝트를 설정하는 스크립트

gradle.properties

- 환경에 따라 값이 달라지는 파라미터들을 스크립트 밖에 선언할 때 사용

### Dependency configuration

---

- implementation
- testimplementation
- api
- runtime
- complieOnly

api/complile(deprecated) 대신 implementation을 사용하면 빌드 시스템이 재컴파일 해야 하는 프로젝트의 크기가 즐어들기 때문에 빌드시간이 상당히 개선 될수 있음

## 스프링부트 Gradle 플러그인

---

스프링부트는 spring-boot-dependencies를 의존성 관리를 위한 BOM(Bill Of Material)으로 사용하고 있음

스프링부트에서 지원하는 의존성 라이브러리는 버전을 명시하지 않아도 BOM에 등록되어 있는 버전으로 일괄 관리됨
