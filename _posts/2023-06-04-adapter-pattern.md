---
layout: post
title: "Adapter Pattern"
description: ""
date: 2023-06-04
tags: [pattern]
---

![0](/assets/images/adapter-pattern/0.png)

```java
public class Point {
  private int x;
  private int y;

  public Point(int x, int y) {
    this.x = x;
    this.y = y;
  }

  public Point() {
    this.x = 0;
    this.y = 0;
  }

  public void set(Point point) {
    this.x = point.getX();
    this.y = point.getY();
  }

  public int getX() {
    return x;
  }

  public int getY() {
    return y;
  }
}

public abstract class Shape {
  public abstract void getBoundingBox(Point leftTop, Point rightBottom);
}

public class TextShape extends Shape {
  public void getBoundingBox(Point leftTop, Point rightBottom) {
    // use the existing class TextViewWithDifferentName
  }
}

public class TextViewWithDifferentName {
  private Point leftTop;
  private Point rightBottom;

  public TextViewWithDifferentName(Point leftTop, Point rightBottom) {
    this.leftTop = leftTop;
    this.rightBottom = rightBottom;
  }

  public void getBoundary(Point leftTop, Point rightBottom) {
    leftTop.set(this.leftTop);
    rightBottom.set(this.rightbottom);
  }
}
```

기존에 존재하는 레거시 코드인 TextViewWithDifferentName 클래스를 사용해야 하는데 Shape 클래스를 상속받아서 만들고 싶을 수 있다.

기존의 클래스가 수정이 가능하다면 getBoundary를 getBoundaryBox로 변경을 고려하면 되겠지만 

클래스를 수정할 수 없는 경우에는 TextShape을 구현하면서 TextViewWithDifferentName 클래스를 이용하는 방법이 필요할 수 있다. (이미 사용 중이거나 수정하기 곤란한 상황)

새로운 기능을 구현하는게 아닌 인터페이스의 불일치를 해결하기 위한 패턴이 Adapter Pattern이다.

```java
public class TextShape extends Shape {
  private TextViewWithDifferentName textView;

  public TextShape(TextViewWithDifferentName textView) {
    super();
    this.textView = textView;
  }

  public void getBoundingBox(Point leftTop, Point rightBottom) {
    Point lt = new Point();
    Point rb = new Point();
    textView.getBoundary(lt, rb);
    leftTop.set(lt);
    rightBottom.set(rb);
  }
}

public class Client {

  public static void main(String[] args) {
    int leftTopX = 0, leftTopY = 0;
    int rightBottomX = 20, rightBottomY = 30;

    TextViewWithDifferentName textView =
        new TextViewWithDifferentName(
            new Point(leftTopX, leftTopY),
            new Point(rightBottomX, rightBottomY)
        );

    Shape textShape = new TextShape(textView);
    Point leftTop = new Point();
    Point rightBottom = new Point();
    textShape.getBoundingBox(leftTop, rightBottom);

    System.out.printf("%d %d %d %d\n", leftTop.getX(), leftTop.getY(), rightBottom.getX(),
        rightBottom.getY());
  }
}
```

다음은 레거시 코드인 LEDLight 클래스를 Lamp 클래스에서 사용하는 예제이다. 

![1](/assets/images/adapter-pattern/1.png)

ILamp를 상속하는 Lamp 클래스의 turnOn, turnOff 메서드에서 switchOn, switchOff 메서드를 사용해야 한다. 

또한 switchOn 메서드는 int 타입을 요구하기 때문에 BrightnessLevel에 따라서 적절한 값을 반환해야 한다.

```java
public enum BrightnessLevel {MINIMUM, MEDIUM, MAXIMUM}

public abstract class ILamp {
  public abstract void turnOn(BrightnessLevel brightnessLevel);
  public abstract void turnOff();
}

public class Lamp extends ILamp {

  @Override
  public void turnOn(BrightnessLevel brightnessLevel) {
    // 레거시 코드인 LEDLight 클래스의 switchOn 메서드를 사용해야 한다.
  }

  @Override
  public void turnOff() {
    // 레거시 코드인 LEDLight 클래스의 switchOff 메서드를 사용해야 한다.
  }
}

public class LEDLight {
  public void switchOn(int brightness) {
    System.out.println("LED Light switches on: " + brightness);
  }

  public void switchOff() {
    System.out.println("LED Light switches off");
  }
}

public class LampClient {

  public static void main(String[] args) {
    ILamp lamp = new Lamp();
    lamp.turnOn(BrightnessLevel.MINIMUM);
    lamp.turnOn(BrightnessLevel.MEDIUM);
    lamp.turnOn(BrightnessLevel.MAXIMUM);
    lamp.turnOff();
  }
}
```

Adapter Pattern을 적용하여 개선한 코드는 아래와 같다.

```java
public class Lamp extends ILamp {
  private LEDLight ledLight;

  public Lamp(LEDLight ledLight) {
    this.ledLight = ledLight;
  }

  @Override
  public void turnOn(BrightnessLevel brightnessLevel) {
    int brightness;
    if (brightnessLevel == BrightnessLevel.MINIMUM) brightness = 20;
    else if (brightnessLevel == BrightnessLevel.MEDIUM) brightness = 50;
    else brightness = 100;
    ledLight.switchOn(brightness);
  }

  @Override
  public void turnOff() {
    ledLight.switchOff();
  }
}

public class LampClient {

  public static void main(String[] args) {
    LEDLight ledLight = new LEDLight();
    ILamp lamp = new Lamp(ledLight);
    lamp.turnOn(BrightnessLevel.MINIMUM);
    lamp.turnOn(BrightnessLevel.MEDIUM);
    lamp.turnOn(BrightnessLevel.MAXIMUM);
    lamp.turnOff();
  }
}
```

레거시 코드에 해당하는 LEDLight 클래스를 객체로 선언하고, 이를 Adapter에 전달하여 사용할 수 있다.

<a href="http://www.yes24.com/Product/Goods/108192370">헤드 퍼스트 디자인 패턴</a>

<a href="https://johngrib.github.io/wiki/pattern/adapter/">어댑터 패턴 (Adapter Pattern) - johngrib</a>
