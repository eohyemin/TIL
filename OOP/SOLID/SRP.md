## SRP - 단일 책임 원칙


어떤 클래스를 변경해야 하는 이유는 오직 하나뿐이어야 한다

하나의 모듈은 하나의 변경사유를 가져야 한다

하나의 모듈은 하나의 액터에 대해서만 책임져야 한다

- 모듈은 함수와 데이터 구조로 구성된 응집된 집합
- 액터란 이해 관계를 가진, 변경을 요청하는 집단
- 응집성 Cohesion
    - 단일 액터를 책임지는 코드를 함께 묶어주는 것

-----

클래스는 하나의 기능만 가지며, 클래스가 제공하는 모든 서비스는 하나의 책임을 수행하는 데 집중되어 있어야 함

One and only one responsibility

클래스를 역할과 책임에 따라 분리

클래스 뿐 아니라 속성, 메소드, 패키지, 모듈, 컴포넌트, 프레임워크에도 적용 가능

SRP를 지키지 못한 예시

- 하나의 속성이 여러 의미를 가지는 경우
- 메소드에서 분기 처리 진행

---

### Responsibility

![image](https://user-images.githubusercontent.com/24310798/124345658-3fef3f80-dc15-11eb-8b36-f371036b5f92.png)

- 위 클래스가 몇개의 책임을 가지는지
- 같은 부류로 나눠 책임이 몇개인지 판단
    - save와 findById는 같은 부류 - 둘 다 디비와 연관되어 있음
        - save, findById, update, delete 메소드
        - 메소드가 추가된다 해도 책임이 증가하는 것은 아님
    - calculatePay, calculateSalary
- 부류
    - 메소드 카테고리 구분
    - 나누는 기준
    - SRP에서는 클라이언트에 의해 구분 - 메소드를 누가 사용하는지
    - 누가 해당 메소드의 변경을 유발하는 사용자인지
    - 위의 예시
        - calculate
        - describe
        - 디비
- SRP에서 책임은 사용자에 관한 것
- 변경의 근원

### It's about Users

![image](https://user-images.githubusercontent.com/24310798/124345721-8775cb80-dc15-11eb-94f5-d6e283877560.png)

위의 그림처럼 사용자그룹을 나눌 수 있음 - 3개의 Actor

- Policy
- Architect
- Operations

사용자그룹 : Actor

- 서로 다른 needs, expectation을 가짐
- Actor들로 책임을 나눈다

### It's about Roles

User들을 그들이 수행하는 역할(Role) 에 따라 나눠야함

User가 특정 역할을 수행할 때 Actor라고 부름

User한명이 3개의 역할을 수행할때 책임이 하나가 아닌 3개

> 책임→ 특정 Actor의 요구사항을 만족시키기 위한 메소드집합
Actor의 요구사항이 변경되면 Actor가 사용하는 메소드집합이 변경

### Two Values of SW

- Primary Value of SW
    - 지속적으로 변하는 요구사항을 수용하는 것
- Secondary value of SW
    - 현재 SW가 현재 사용자의 현재 요구사항을 만족하는지

1. Collision

    3가지 책임을 한 클래스에 두면 충돌이 일어남

2. Fan Out

    Employee는 너무 많은 것을 안다

    - Business rules
    - DB
    - Reports, formatting

    많은 책임을 갖는다

    - CalculateionAPI, DatabaseAPI, StringAPI
    - 3개의 API중 하나만 변경이 일어나도 Employee에 변경이 일어남
    - Employee를 사용하는 클라이언트에도 영향

    Employee가 하나의 책임을 가지도록 해서 Fan Out 최소화 → SRP

3. Collocation is Coupling

    Operations Actor의 새로운 요구사항 

    → Policy, Architecture에는 변경이 없음에도 Employee가 변경됨

### SRP

하나의 모듈은 하나의 변경 사유를 가져야 한다

- 변경 사유: Actor, 그 클래스를 사용하는 사람
- 하나의 클래스는 한 종류의 클라이언트만 서빙해야 한다.

### Solutions

- 3개의 Actor, 3개의 Responsibility가 하나의 클래스
- 어떻게 책임을 분리할지?

### 1.  Inverted Dependencies

![image](https://user-images.githubusercontent.com/24310798/124345756-affdc580-dc15-11eb-875a-6e8a6cc9bf63.png)

- 클래스를 클래스와 인터페이스로 분리
- 하나의 Actor의 요구사항 변경으로 Impl클래스가 변경되더라도, Employee Interface가 변경되지 않으면 다른 Actor들을 영향받지 않는다
- Employee인터페이스가 변경되면 영향을 받긴함

### 2. Extract Classes

![image](https://user-images.githubusercontent.com/24310798/124345776-d0c61b00-dc15-11eb-8644-0f999dae9bf2.png)

- 3개의 클래스로 분리하는 방법
    - Actor들은 불리된 3개의 클래스에 의존
    - 3개의 책임에 대한 구현 분리
    - 하나의 책임에 대한 변경이 다른 책임에 영향을 미치지 않음
- 문제점
    - transitive dependency
        - Employee의 변경이 Gateway와 Reporter에 영향줄 수 있음
        - Employee의 개념이 3조각으로 분리?

### 3. Facade

![image](https://user-images.githubusercontent.com/24310798/124345798-f6532480-dc15-11eb-81b7-8439da60fe39.png)

- 어디에 구현되어있는지 찾기 쉽게
- Actor들은 여전히 Coupling 되어있음

### 4. Interface Segregation

![image](https://user-images.githubusercontent.com/24310798/124345808-0b2fb800-dc16-11eb-9967-d800595de695.png)

- 책임에 따라 인터페이스를 3개로 분리
- 3개의 인터페이스를 하나의 클래스가 구현
    - Actor들은 decoupled
    - 하나의 클래스에 구현되어 구현은 coupled → Impl도 3개로 추출하자
