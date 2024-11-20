## 오늘의 개인 공부

### Q1. 추상 클래스와 인터페이스의 차이?

먼저 📌 추상 클래스에 대한 설명을 한다.

우리가 보통 구현하는 일반적인 클래스는 구체적(concrete)으로 데이터를 담아서 인스턴스화하여 직접 다루는 클래스다. 반대로 추상 클래스는 구체적이지 않은 추상적인(abstract) 데이터를 담고 있는 클래스이다. 그렇기 때문에 일반 클래스와 달리 인스턴스화가 불가능한 클래스이며, 추상 클래스를 선언할 때는 `abstract` 키워드를 사용한다.

중요한 것은 _문법적인 특징이나 객체 생성이 되고 안되고가 중요한 게 아니다._ 우리가 늘 사용하는 반복문이나 문자열 처리 메서드는 모두 추상화의 개념이 들어있다. 추상화라는 개념이 바로 머릿 속에 들어오지 않았던 이유는 너무 당연하게 생각하고 사용해왔기 때문이다.

정리하자면, 추상 클래스는 '추상화'를 클래스에 접목시킨 것이다. 추상 클래스는 클래스에 추상화를 접목시켜서 보다 구조적이게 객체를 설계하고 프로그램의 유지보수성을 올려주며, 만약 프로그램에 어떠한 기능을 업그레이드한다고 하면 수정과 추가를 유연적으로 수행할 수 있게 해준다.

사실 추상 클래스는 추상 메서드를 포함하고 있다는 것을 제외하고는 일반 클래스와 전혀 다른 점이 없다. 단, new 생성자를 통한 추상 클래스의 인스턴스는 만들 수 없으니 유의해야 한다.

```java
// 자바에서는 abstract 키워드를 클래스명과 메서드명 앞에 붙임으로써 컴파일러에게 추상 클래스/메서드임을 알려준다.

public abstract class Shape {
    private String type;

    public Shape(String type) {
      this.type = type;
    }

    // 추상 클래스 안에 메서드를 미완성으로 남겨놓는 이유는 추상 클래스를 상속받는 자식 클래스의 주제에 따라서 상속 받는 메서드의 내용이 달라질 수 있기 때문이다.
    public abstract void draw(); // 추상 메서드
}
```

```java
abstract class Pet {
    abstract public void walk();
    abstract public void eat();

    public int health; // 인스턴스 필드
    public void run() { // 인스턴스 메서드
        System.out.println("run run");
    }
}

class Dog extends Pet {
    public void walk() {
      System.out.println("Dog walk");
    }
    public void eat() {
      System.out.println("Dog eat");
    }
}

public class main {
  public static void main(String[] args) {
    Dog d = new Dog();
    d.eat();
    d.walk();
    d.run();
  }
}
```

#### 공통 멤버의 통합으로 중복 제거

다음과 같이 Marine, Tank, DropShip 클래스가 정의되어 있다고 하자. 자세히 살펴보면 이름이 겹치는 필드와 메서드들이 몇몇 보인다.

```java
class Marine {
  int x, y;
  void move(int x, int y) {}
  void stop() {}
  void stimPack() {}
}

class Tank {
  int x, y;
  void move(int x, int y) {}
  void stop() {}
  void siegeMode() {}
}

class DropShip {
  int x, y;
  void move(int x, int y) {}
  void stop() {}
  void loadUnload() {}
}
```

만약 상속(extends) 기능을 이용해서 3개의 클래스를 대표할 수 있는 부모 추상 클래스로 묶으면 상위 클래스의 특징을 하위 클래스에서 그대로 물려 받아 사용할 수 있는 상속 특징을 이용하여 코드의 중복 제거, 코드의 재사용성 증대 효과를 누릴 수 있게 된다.

```java
abstract class Unit {
  int x, y;
  abstract void move(int x, int y); // move 명령어는 자식 클래스마다 이동하는 로직이 달라서 오버라이딩을 통해 재정의하도록 함
  void stop() {} // 어떤 유닛이건 간에 명령이 동일하므로 인스턴스 메서드로 생성

class Marine extends Unit {
  void move(int x, int y) {
    System.out.println("걸어서 이동");
  }
  void stimPack() {}
}

class Tank extends Unit {
  void move(int x, int y) {
    System.out.println("굴러서 이동");
  }
  void siegeMode() {}
}

class DropShip extends Unit {
  void move(int x, int y) {
    System.out.println("날아서 이동");
  }
  void loadUnload() {}
}
```

> [!NOTE]
> 사실 공통된 필드와 메서드를 통일하는 목적으로는 일반 클래스로도 가능하여 꼭 추상 클래스만의 고유 용도라고 보기는 어렵다. 하지만 이 예제는 인터페이스(interface)와의 차이점을 보여준다. Java8의 인터페이스도 똑같이 안에 필드를 선언해줄 수 있지만, 자동으로 `public static final`처리가 되기 때문에 이른바 공통 상수가 되어 버린다. 따라서 자식 클래스에서 중복되는 변수들을 상속으로 묶어 통합시키는 기능 자체는 인터페이스로는 구현할 수 없고 오로지 추상 클래스로만 가능하다.

#### 구현의 강제성을 통한 기능 보장

강제적으로 추상 메서드에 대한 구현을 자식 클래스에서 하게끔 강제하기 때문에 개발자가 실수해도 바로 고칠 수 있다. 그런데, 일반 클래스로 상속 관계를 맺는 상황에서 위와 같은 실수가 일어나면 아무 오류도 일어나지 않는다.

> [!NOTE]
> 위 특성은 인터페이스에도 똑같이 적용된다.

---

### Q1의 또 다른 정리

📌 추상 클래스

- abstract
- 사용 가능 변수에 제한 없음
- 사용 가능 접근 제어자에 제한 없음
- 사용 가능 메서드에 제한 없음
- 상속 키워드: extends
- 다중 상속 가능 여부: 불가능

📌 인터페이스

- interface
- 변수는 모두 static final(상수) 취급됨
- 사용 가능한 접근 제어자는 public으로 유일
- 사용 가능 메서드는 abstract method, default method, static method, private method
- 상속 키워드: implements
- 다중 상속 가능 여부: 가능

📌 서로의 공통점

1. 추상 메서드를 가지고 있어야 한다.
2. 인스턴스화 할 수 없다.(new 생성자 사용 안됨)
3. 인터페이스 혹은 추상 클래스를 상속받아 구현한 구현체의 인스턴스를 사용해야 한다.
4. 인터페이스와 추상클래스를 구현, 상속한 클래스는 추상 메서드를 반드시 구현해야 한다.

인터페이스는!

- 부모 자식 관계인 상속에 얽매이지 않고, 공통 기능이 필요할 때마다 추상 메서드를 정의해놓고 구현(implement)하는 식으로 추상클래스보다 자유롭게 붙였다 떼는 식으로 사용 가능하다.
- **인터페이스는 클래스와 별도로 구현 객체가 같은 동작을 한다는 것을 보장하기 위해 사용하는 것에 초점을 둔다.**
- 보통 ~able 접미사를 붙이는 인터페이스 네이밍 규칙을 따른다.

추상 클래스는!

- 추상클래스는 다중 상속이 불가능하여 단일 상속만 허용한다.
- 추상클래스는 추상화(추상 메서드)를 하면서 중복되는 클래스 멤버들을 통합 및 확장할 수 있다.
- **같은 추상화인 인터페이스와 다른 점은, 추상 클래스는 클래스간의 연관 관계를 구축하는 것에 초점을 둔다.**
