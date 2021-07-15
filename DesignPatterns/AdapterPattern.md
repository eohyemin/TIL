한 클래스의 인터페이스를 Client에서 사용하고자 하는 다른 인터페이스로 변환하는 것

어댑터 패턴의 목적은 인터페이스 호환성 문제를 해결하는 것

- 클라이언트와 구현된 인터페이스를 분리할 수 있음
- 인터페이스가 변경되더라도, 변경 내역은 어댑터에 캡슐화되기 때문에 클라이언트는 변경할 필요가 없음

![image](https://user-images.githubusercontent.com/24310798/125798650-09c310e3-848b-4fae-96f0-ade75eddd9e9.png)



Flow

- Client는 Target 인터페이스만 볼 수 있다
- Adapter에서 Target인터페이스를 구현한다
- Adapter는 Adaptee로 구성되어 있다 (Composition 사용)
    - Adaptee의 서브클래스에 대해서도 Adapter를 사용할 수 있다
- 모든 요청은 Adaptee에게 위임된다
