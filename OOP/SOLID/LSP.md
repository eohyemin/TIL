## LSP - 리스코프 치환 원칙


서브 타입은 언제나 Base Type으로 교체할 수 있어야 한다

- 하위 클래스 is a kind of 상위 클래스
- 구현 클래스 is able to 인터페이스

```java
public class LSP {
	static P p = new P();
	
	static class T{
		public void doSomething)_ {
			System.out.println("T doSomething");
		}
	}
	
	static class S extends T {
		public void doSomething)_ {
			System.out.println("S doSomething");
		}
	}
	
	static class P {
		public void doSomething(T t) {
			t.doSomething();
		}
	}

	public static void main(String[] args) {
		T t = new T();
		S s = new S();

		P.doSomething(t);
		P.doSomething(s);
		// s가 t를 대체하더라도 클래스P에 어떤 변경도 없다면
	}
}
```

public void doSomething(T t) 에서 다운캐스팅을 사용하지 않아도 돌아가야함

instanceOf, 다운캐스팅을 사용하면 타입에 대한 의존성이 있는 것

### OCP vs LSP

---

- OCP
    - abstraction, polymorphism 을 이용해서 구현
- LSP
    - OCP를 받쳐주는 다형성에 관한 원칙을 제공
    - LSP가 위반되면 OCP도 위반된다
    - LSP가 위반되면 SubType이 추가될 때 마다 클라이언트에 변경 발생
    - instanceOf, Downcasting 사용은 전형적인 LSP 위반

### Rectangle 예제

---

```java
public class Rectangle {
	private int width;
	private int height;

	public int area() {
		return width * height;
	}
	public void setWidth(int width) {
		this.width = width;
	}
	public void setHeight(int height) {
		this.height = height;
	}
}
```

```java
public class Square extends Rectangle {
	@Override
	public void setWidth(int width) {
		super.setHeight(width);
		super.setWidth(width);		
	}
	public void setHeight(int height) {
		super.setHeight(height);
		super.setWidth(height);
	}
}
```

Square IS-A Rectangle 관계 성립됨

```java
public class RectangleTest {
	private final int width = 3;
	private final int height = 5;

	@Test
	public void should_return_area_by_multiplying_width_and_height() {
		// given
		Rectangle rectangle = createRectangle(new Rectangle());

		// when
		int area = rectangle.area();
		
		// then
		asesrtThat(area, is(width * height));
	 }
	
	private Rectangle createRectangle(Rectangle rectangle) {
		rectangle.setWidth(width);
		rectangle.setHeight(height);
		return rectangle;
	}
}
```

```java
// Square 와 Rectangle AREA TEST
@Test
@Test
public void should_return_area_by_multiplying_width_and_height() {
	// given
	Rectangle rectangle = createRectangle(new Rectangle());
	Rectangle square = createRectangle(new Square());

	// when
	int area = rectangle.area();
	
	// then
	assertArea(rectangle);
	assertArea(square);
}

private void assertArea(Rectangle rectangle) {
	if(rectangle instanceOf Square) {
		assertThat(rectangle.area(), is(height * height);
	} else {
		assertThat(rectangle.area(), is(width * height);
	}
}
```

IS-A관계가 성립해도 LSP 위반되는 경우가 생길 떄

상속 대신 Composition 사용하는 방법 고려
