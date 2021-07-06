## OCP - 개방 폐쇄 원칙

---

소프트웨어 개체는 확장에는 열려 있어야 하고, 변경에는 닫혀 있어야 한다.

→ 자신의 확장에는 열려 있고, 주변의 변경에 대해서는 닫혀 있어야 함

요구사항의 변경이나 추가사항이 발생하더라도, 기존 구성요소는 수정이 일어나지 말아야 하며, 기존 구성요소를 쉽게 확장해서 재사용할 수 있어햐 함

OCP를 가능하게 하는 중요 메커니즘은 추상화와 다형성

OCP의 목표
- 확장하기 쉽게 만든다
- 변경으로 인해 시스템이 너무 많은 영향을 받지 않게 만든다

어떻게
- 시스템을 컴포넌트 단위로 분리
- 저수준 모듈에서 발생한 변경으로부터 고수준 모듈을 보호할 수 있는 형태의 **의존성 계층구조**를 만든다


### Open and Closed

![image](https://user-images.githubusercontent.com/24310798/124349780-e8a89980-dc2b-11eb-9bdf-a6e0388d4cb3.png)

- Open for extension
    - Reader, Writer의 구현체를 추가하는 일에는 열려있음
- Closed for modificatoin
    - 확장이 일어날 때, 상위 레벨이 영향을 받지 않아야 함
- 상위 레벨의 모듈이 하위 레벨에 의존성을 가지는 경우, 사이에 Abstract Interface 추가
- Spring Framework
    - DI 컨테이너
    - 하위레벨의 구현체를 관리

### Pos Example

```java
void checkOut(Receipt receipt) {
	Money total = Money.zero;
	for(Item item: items) {
		total += item.getPrice();
			receipt.addItem(item);
	}
	Payment p = acceptCash(total);
	receipt.addPayment(p);
}
```

신용카드를 받고자 할때는 확장이 필요함

```java
void checkOut(Receipt receipt) {
	Money total = Money.zero;
	for(Item item: items) {
		total += item.getPrice();
			receipt.addItem(item);
	}
	Payment p; // if else 추가되어야 함
	if(isCredit) {
		p = acceptCredit(total);
	} else {
		p = acceptCash(total);
	}
	receipt.addPayment(p);
}
```

확장을 위해 소스를 수정해야하기 때문에 OCP 위반

→ 확장이 필요한 행위를 Abstraction

```java
// 인터페이스로 분리해 인터페이스를 외부에서 주입할 수 있도록 변경
void checkOut(Receipt receipt, PaymentMethod pm) { 
	Money total = Money.zero;
	for(Item item: items) {
		total += item.getPrice();
			receipt.addItem(item);
	}
	Payment p = pm.acceptPayment(total);
	receipt.addPayment(p);
}
```

![image](https://user-images.githubusercontent.com/24310798/124349806-17267480-dc2c-11eb-9788-d8a9239b5e6c.png)

- CheckOut은 인터페이스에만 의존하고, 하위레벨 구현체에 의존하지 않음
- CheckOut 모듈의 수정 없이, PaymentMethod를 확장할 수 있다


### Is This Possible?

- main partition
    - 프레임워크에서 관리로 해결 가능
- Crystal ball problem
    - 미래의 변경으로부터 안전하도록 Abstract을 적용하는 것은 쉽지만, 미리 알기 어려움

