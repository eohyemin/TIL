## ISP - 인터페이스 분리 원칙


Interface Segregation Principle

클라이언트는 자신이 사용하지 않는 메소드에 의존관계를 맺으면 안된다

- 사용하는 기능만 제공하도록 인터페이스 분리
- 클라이언트 입장에서 인터페이스를 분리

### SRP vs ISP

SRP와 ISP는 같은 문제에 대한 두 가지 다른 해결책

### 인터페이스 최소주의 원칙

인터페이스를 통해 외부에 메소드를 제공할 때, 최소한의 메소드만 제공하라

상위클래스는 풍성할수록 좋고, 인터페이스는 작을수록 좋다

----

![image](https://user-images.githubusercontent.com/24310798/124375655-50b7b800-dcde-11eb-9197-0a7dfd77d6d9.png)


![image](https://user-images.githubusercontent.com/24310798/124375702-82c91a00-dcde-11eb-8381-5a8d3b5d0bfc.png)


**One interface for a sub system**

어떤 특정 인터페이스에 변경 발생

→ Job 클래스와 해당 인터페이스를 사용하는 클라이언트만 변경

Job이 변경되어도, 나머지 클라이언트들은 인터페이스에 의해 보호되고 있어 변경이 일어나지 않음

Interface는 클라이언트와 논리적으로 결합되므로, 클라이언트가 호출하는 메소드만 Interface에 정의되었다는 것을 확신할 수 있음
