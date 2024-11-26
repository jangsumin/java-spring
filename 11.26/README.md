## 오늘의 공부 내용

### Q1. 데코레이터 패턴에 대해 알아보자.

실행 중에 확장하는 일의 위력을 보여주고, 객체를 꾸미는데 관심있는 데코레이터 패턴을 배워보자.

초대형 커피 전문점이 있다고 가정하고 Beverage라는 추상 클래스를 만들었다.

```java
public abstract class Beverage { // 추상 클래스
  private String description;

  public String getDescription() {
    return this.description;
  }

  public abstract int cost(); // 추상 메서드
}
```

이제 모든 서브 클래스(HouseBlend, DarkRoast, Decaf 등)에서 추상 메서드인 `cost()`메서드를 구현해야 한다.

> [!WARNING]
> 고객은 커피를 주문할 때 우유나 두유, 모카(초콜릿)를 추가하고 그 위에 휘핑크림을 얹기도 한다. 각각 추가할 때마다 커피 가격이 올라가야 해서 주문 시스템을 구현할 때 이런 점을 모두 고려해야 한다.

수백가지를 다루는 커피 전문점이라면 서브 클래스가 폭발적으로 증가하게 된다. 예를 들어 HouseBlendWithSteamedMilkandMocha, HouseBlendWithWhippingCream 등이 있다...❌

### 그럼 그냥 인스턴스 변수와 슈퍼클래스 상속을 써서 첨가물을 관리하면 안될까?

```java
public abstract class Beverage {
  private String description;
  private boolean milk;
  private boolean soy;
  private boolean mocha;
  private boolean ship;

  public String getDescription() {
    return this.description;
  }

  public int cost() {
    // 이제 추상 메서드로 정의하지 않는다.
    // 각 음료 인스턴스마다 첨가물에 해당하는 비용까지 포함할 수 있도록 하기 위해서다.
    return 5000;
  }

  public boolean hasMilk() {}
  public boolean setMilk() {}
  public boolean hasSoy() {}
  // 뒤는 생략
}

public class DarkRoast extends Beverage {
    public DarkRoast() {
      description = "최고의 다크 로스트 커피";
    }

    @Override
    public int cost() {
      int total = 5000;

      if (hasMilk()) total += 1000;
      if (hasSoy()) total += 500;
      // 뒤는 생략
    }
}
```

이런 식으로 바꾸면 HouseBlendWithSteamedMilkandMocha, HouseBlendWithWhippingCream같이 복잡한 클래스에서 HouseBlend라는 클래스만 추출해낼 수 있어서 편하다. 그리고 관리해야 할 커피 클래스는 줄어들게 된다.

### 그러나, 상속을 써서 음료 가격과 첨가물 가격을 합해서 총 가격을 산출하는 방법은 그리 좋지 않았다.

다음과 같은 문제가 야기된다.

- 첨가물 가격이 바뀔 때마다 기존 코드를 수정해야 한다.
- 첨가물의 종류가 많아지면 새로운 메소드를 추가해야 하고, 슈퍼클래스의 cost() 메소드도 고쳐야 한다.
- 새로운 음료가 출시될 수도 있다. 그중에는 특정 첨가물이 들어가면 안되는 음료도 있을 것이다.

👉 그렇다면, 특정 음료에서 시작해서 첨가물로 그 음료를 장식(decorate)해보자. 어떤 고객이 모카와 휘핑크림을 추가한 다크 로스트 커피를 주문한다면 다음과 같이 장식할 수 있다.

1. DarkRoast 객체를 가져온다.
2. Mocha 객체로 장식한다.
3. Whip 객체로 장식한다.
4. cost() 메소드를 호출한다. 이 때, 첨가물의 가격을 계산하는 일은 해당 객체에게 위임한다.

### 데코레이터 패턴 정리

- 데코레이터(Decorator)의 슈퍼클래스는 자신이 장식하고 있는 객체의 슈퍼클래스와 같다.

```java
// Decorator.java
// Decorator가 장식하고 있는 ConcreteComponent(원본 객체)의 슈퍼클래스는 Component이다.
public abstract class Decorator extends Component {
	protected Component component;

	public Decorator(Component component) {
		this.component = component;
	}
}
```

- 한 객체를 여러 개의 데코레이터로 감쌀 수 있다.

```java
// Client.java
// 여러 개의 데코레이터(BraceDecorator, ParenthesisDecorator)로 감싼 모습
public class Client {
	public static void main(String[] args) {
		System.out.println(new BraceDecorator(new ParenthesisDecorator(new ConcreteComponent("Hello World!"))).operation());
	}
}
```

- 데코레이터는 자신이 감싸고 있는 객체와 같은 슈퍼클래스를 가지고 있기에 원래 객체가 들어갈 자리에 데코레이터 객체를 넣어도 상관없다.
- 데코레이터는 자신이 장식하고 있는 객체에게 어떤 행동을 위임하는 일 말고도 추가 작업을 수행할 수 있다.
- 객체는 언제든지 감쌀 수 있으므로 실행 중에 필요한 데코레이터를 마음대로 적용할 수 있다.

### 커피 주문 시스템에 데코레이터 패턴 적용하기

```java
// 데코레이터와 원본 객체의 슈퍼클래스
public abstract class Beverage {
  String description = "제목 없음";

  public String getDescription() {
    return description;
  }

  public abstract double cost();
}

// 첨가물(condiment)을 나타내는 데코레이터 클래스
public abstract class CondimentDecorator extends Beverage {
  Beverage beverage;
  public abstract String getDescription(); // 상속하는 모든 첨가물 데코레이터에 getDescription() 메서드 구현을 강제할 계획
}

// 실제 음료 구현(원본 객체1)
public class Espresso extends Beverage {
  public Espresso() {
    description = "에스프레소";
  }

  public double cost() {
    return 1.99;
  }
}

// 실제 음료 구현(원본 객체2)
public class HouseBlend extends Beverage {
  public HouseBlend() {
    description = "하우스 블렌드 커피";
  }

  public double cost() {
    return .89;
  }
}

// 첨가물인 Mocha 데코레이터 클래스 구현
public class Mocha extends CondimentDecorator {
  public Mocha(Beverage beverage) {
    this.beverage = beverage;
  }

  public String getDescription() {
    return beverage.getDescription() + ", 모카";
  }

  public double cost() {
    return beverage.cost() + .20;
  }
}

// 커피 주문 시스템 코드 테스트
public class Client {
  public static void main(String[] args) {
    Beverage beverage = new Espresso(); // 아무것도 첨가하지 않은 에스프레소(원본 객체, ConcreteComponent)
    System.out.println(beverage.getDescription() + " $" + beverage.cost());

    Beverage beverage2 = new DarkRoast();
    beverage2 = new Mocha(beverage2); // 모카 추가
    beverage2 = new Mocha(beverage2); // 모카 또 추가
    beverage2 = new Whip(beverage2); // 휘핑크림 추가
    System.out.println(beverage2.getDescription() + " $" + beverage2.cost());
  }
}
```

> [!NOTE]
> 우리는 데코레이터 패턴을 사용함으로써 수많은 서브 클래스를 생성하고 관리해야 할 필요를 없애고, 런타임에 객체의 기능을 확장할 수 있다는 장점을 얻을 수 있다.
