---
layout: post
title: "Factory Method Pattern"
description: ""
date: 2023-06-05
tags: [pattern]
---
```java
public abstract class Namer {
  protected String last;
  protected String first;

  public String getFirst() {
    return first;
  }

  public String getLast() {
    return last;
  }
}

public class FirstFirst extends Namer {
  public FirstFirst(String name) {
    int i = name.lastIndexOf(" ");
    if (i > 0) {
      first = name.substring(0, i).trim();
      last = name.substring(i+1).trim();
    }
    else {
      first = name;
      last = "";
    }
  }
}

public class LastFirst extends Namer {
  public LastFirst(String name) {
    int i = name.lastIndexOf(",");
    if (i > 0) {
      last = name.substring(0, i).trim();
      first = name.substring(i+1).trim();
    }
    else {
      first = "";
      last = name;
    }
  }
}

public class Client {

  public static void main(String[] args) {
    String name = "Hong, Kildong";
    Namer namer;
    int i = name.indexOf(",");
    if (i > 0)
      namer = new LastFirst(name);
    else
      namer = new FirstFirst(name);

    System.out.println("Last name: " + namer.getLast() +
        ", First name: " + namer.getFirst());
  }
}
```

위 코드는 문자에 `,` 유무를 체크하여 객체를 생성하고 있다.

하지만 동작 방식이 바뀐다면 사용 중인 코드를 수정해야 하므로 OCP를 위반한다고 볼 수 있다.

그리고 namer 변수나 출력문은 인터페이스에 의존하고 있지만, name 변수는 LastFirst, FirstFirst 클래스의 존재를 알아야하므로(의존하므로) DIP도 위반한다고 볼 수 있다.

이를 별도의 Factory 메서드를 만들어서 코드를 개선할 수 있다.

```java
public class NameFactory {
  public Namer getInstance(String name) {
    int i = name.indexOf(",");
    if (i > 0)
      return new LastFirst(name);
    else
      return new FirstFirst(name);
  }
}

public class Client {

  public static void main(String[] args) {
    NameFactory nameFactory = new NameFactory();

    Namer lastNamer = nameFactory.getInstance("Hong, Kildong");
    System.out.println("Last name: " + lastNamer.getLast() +
        ", First name: " + lastNamer.getFirst());

    Namer firstNamer = nameFactory.getInstance("Kildong Hong");
    System.out.println("Last name: " + firstNamer.getLast() +
        ", First name: " + firstNamer.getFirst());
  }
}
```

이제 클라이언트 입장에서는 FirstFirst, LastFirst 클래스의 존재유무와 구분하는 기준이 무엇인지 알필요 없이 그저 getInstance 메서드를 호출하여 nameFactory로 넘겨주면 되는 것이다.

<a href="https://hyuunnn.github.io/2023/06/04/template-method-pattern/">template-method-pattern</a> 예제 활용

```java
public class ElevatorController {
  private int id;
  private int curFloor = 1;
  private Motor motor;

  public ElevatorController(int id, int curFloor, Motor motor) {
    this.id = id;
    this.curFloor = curFloor;
    this.motor = motor;
  }

  public void gotoFloor(int destination) {
    if (destination == curFloor) return;
    Direction direction;
    if (destination > curFloor) direction = Direction.UP;
    else direction = Direction.DOWN;
    motor.move(direction);
    System.out.println("Elevator [" + id + "] Floor: " + curFloor);
    curFloor = destination;
    System.out.println(" ==> " + curFloor + " with" +
        motor.getClass().getSimpleName());
    motor.stop();
  }
}

public class Client {

  public static void main(String[] args) {
    Motor lgMotor = new LGMotor();
    ElevatorController controller1 = new ElevatorController(1, lgMotor);
    controller1.gotoFloor(5);
    controller1.gotoFloor(3);

    Motor hyundaiMotor = new HyundaiMotor();
    ElevatorController controller2 = new ElevatorController(2, hyundaiMotor);
    controller2.gotoFloor(4);
    controller2.gotoFloor(6);
  }
}
```

모터를 컨트롤하는 ElevatorController 클래스를 만들어서 사용한다고 했을 때 Client의 코드처럼 사용될 수 있겠다.

ElevatorController 자체는 Motor 타입을 파라미터로 받기 때문에 DIP, OCP를 충족한다고 볼 수 있지만,

Client는 LGMotor, HyundaiMotor의 존재를 알고 생성하여 넘겨줘야하기 때문에 DIP, OCP를 위반한다고 볼 수 있다.

이때 Factory 메서드를 만들면 하나의 클래스가 Vendor에 따라서 다른 객체를 반환하는 방식으로 코드를 수정할 수 있다.

```java
public class MotorFactory {
  public static Motor getMotor(MotorVendorID, vendorID) {
    Motor motor = null;
    switch (vendorID) {
      case LG: motor = new LGMotor(); break;
      case HYUNDAI: motor = new HyundaiMotor(); break;
    }
    return motor;
  }
}

public class Client {

  public static void main(String[] args) {
    Motor lgMotor = MotorFactory.getMotor(MotorVendorID.LG);
    ElevatorController controller1 = new ElevatorController(1, lgMotor);
    controller1.gotoFloor(5);
    controller1.gotoFloor(3);

    Motor hyundaiMotor = MotorFactory.getMotor(MotorVendorID.HYUNDAI);
    ElevatorController controller2 = new ElevatorController(2, hyundaiMotor);
    controller2.gotoFloor(4);
    controller2.gotoFloor(6);
  }
}
```

하지만 위 방법도 VendorID라는 enum 값을 명시해야 하기 때문에 개선의 여지가 있지만 이전보다는 개선된 코드라고 볼 수 있다. (MotorFactory가 상황에 따라 다른 클래스를 반환한다는 점에서)

switch 코드는 <a href="https://hyuunnn.github.io/2023/05/21/solid/">SOLID</a> 글에서 언급했던 enum을 활용하여 제거할 수 있겠다.

![0](/assets/images/factory-method-pattern/0.png)

위 UML도 보면 상황에 따라서 GeneralStatistics, JavaStatistics 클래스를 사용하는데 Factory 메서드를 사용하면 

```java
public enum StatID {GENERAL, JAVA}

public class StatisticsFactory {
  public static Statistics getStatistics(StatID statID) {
    Statistics stattistics = null;
    switch (statID) {
      case GENERAL: statistics = new GeneralStatistics(); break;
      case JAVA: statistics = new JavaStatistics(); break;
    }
    return statistics;
  }
}

public class Client {

  public static void main(String[] args) {
    int[] data = {0, 50, 10, 30, 70};
    Statistics generalStatistics = StatisticsFactory.getStatistics(StatID.GENERAL);
    ScoreProcessing proc1 = new ScoreProcessing(generalStatistics);
    proc1.analyze(data);

    Statistics javaStatistics = StatisticsFactory.getStatistics(StatID.JAVA);
    ScoreProcessing proc2 = new ScoreProcessing(javaStatistics);
    proc2.analyze(data);
  }
}
```
위와 같이 코드를 개선할 수 있다.

하지만 아까 말했듯이 특정 방법에 해당하는 StatID 값을 넣어야 하기 때문에 기능을 변경할 때 내부 코드를 수정해야 한다는 점이다.

```java
public class StatisticsFactory {
  public static <T> Statistics geStatistics(List<T> data) {
    Statistics statistics = null;
    if (data.size() >= 100)
      statistics = new GeneralStatistics();
    else
      statistics = new JavStatistics();
    return statistics;
  }
}
```

위와 같이 Factory 메서드 안에서 판단하여 상황에 따라 다른 값을 반환하는게 더욱 바람직한 코드 설계이다. (Factory 메서드를 이용할 때 메서드의 인자가 내부에서 return 하는 클래스를 암시하지 않도록 하는 것이 중요하다.)

<a href="http://www.yes24.com/Product/Goods/12501269">자바 객체지향 디자인 패턴</a>

<a href="http://www.yes24.com/Product/Goods/108192370">헤드 퍼스트 디자인 패턴</a>
