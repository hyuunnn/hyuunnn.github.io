---
layout: post
title: "Abstract Factory Pattern"
description: ""
date: 2023-06-05
tags: [pattern]
---

```java
public abstract class Button {
  public abstract void clicked();
}

public class MotifButton extends Button {

  @Override
  public void clicked() {
    System.out.println("MitifButton Clicked");
  }
}

public class WindowsButton extends Button {

  @Override
  public void clicked() {
    System.out.println("WindowsButton Clicked");
  }
}

public abstract class Menu {
  public abstract void draw();
}

public class MotifMenu extends Menu {

  @Override
  public void draw() {
    System.out.println("MotifMenu");
  }
}

public class WindowsMenu extends Menu {

  @Override
  public void draw() {
    System.out.println("WindowsMenu");
  }
}

public enum UIType {
  MOTIF, WINDOWS
}

public class UI {
  private Button _button;
  private Menu _menu;
  private UIType _uiType;
  public UI(UIType type) { _uiType = type; }

  public void create() {
    switch (_uiType) {
      case MOTIF: {
        _button = new MotifButton();
        _menu = new MotifMenu();
        break;
      }
      case WINDOWS: {
        _button = new WindowsButton();
        _menu = new WindowsMenu();
        break;
      }
    }
  }
  public void draw() { _menu.draw(); }
  public void click() { _button.clicked(); }
}

public class UIMain {

  public static void main(String[] args) {
    UI ui = new UI(UIType.MOTIF);
    ui.create();
    ui.draw();
    ui.click();
  }
}
```

버튼을 클릭했을 때 메뉴를 보여주는 기능을 가지고 있는 클래스이다.

Button, Menu는 abstract 클래스로 되어있으며 이를 상속하여 구현하는 각각의 세부 클래스들이 존재한다.

UI 클래스에서 _button, _menu 변수는 abstract 클래스의 타입으로 받고 있으며, draw와 click 메서드는 해당 변수의 메서드를 호출하고 있기 때문에 DIP를 위반하지 않고 있다. 

하지만 create 메서드는 enum 값에 따라서 다른 객체를 반환하는데 implementation 클래스를 사용하고 있으므로 DIP를 위반하고 있으며, 또 다른 타입이 들어온다면 switch 코드를 수정해야하므로 OCP를 위반하고 있다.

**Factory Method Pattern을 사용하는 방법**

```java
public class UI {
  private Button _button;
  private Menu _menu;
  private UIType _uiType;
  public UI(UIType type) { _uiType = type; }

  public void create() {
    _button = ButtonFactory.getButton(_uiType);
    _menu = MenuFactory.getButton(_uiType);
  }
  public void draw() { _menu.draw(); }
  public void click() { _button.clicked(); }
}
```

하지만 <a href="https://hyuunnn.github.io/2023/06/05/factory-method-pattern/">factory method pattern</a> 글에서 봤듯이 결국 UIType에 의존하여 값에 따라서 Button과 Menu를 반환하기 때문에 바람직한 코드라고 볼 수는 없다.

또한 Factory Method에 새로운 UIType을 처리하지 않아서 _button은 MOTIF인데 _menu는 WINDOWS라면 의도하지 않는 동작을 하게 된다. (각 Factory Method는 독립적으로 떨어져서 동작하고 있음)

**Abstract Factory Pattern 사용하기**

![0](/assets/images/abstract-factory-pattern/0.png)

UML을 보면 패턴명 그대로 세부 Factory 클래스들을 통틀어서 Factory라는 abstract 형태의 클래스로 만들어서 사용하고 있다.

UI 클래스의 생성자는 Factory 타입으로 주입받기 때문에 WindowsFactory인지 MotifFactory인지 알필요가 없다.

```java
public abstract class Factory {
  public abstract Button CreateButton();
  public abstract Menu CreateMenu();
}

public class MotifFactory extends Factory {
  public Button CreateButton() {
    return new MotifButton();
  }

  public Menu CreateMenu() {
    return new MotifMenu();
  }
}

public class WindowsFactory extends Factory {
  public Button CreateButton() {
    return new WindowsButton();
  }
  public Menu CreateMenu() {
    return new WindowsMenu();
  }
}

public class UI {
    private Button _button;
    private Menu _menu;
    private Factory _factory;

    public UI(Factory factory) { _factory = factory; }
    public void create() {
        _button = _factory.CreateButton();
        _menu = _factory.CreateMenu();
    }
    public void draw() { _menu.draw(); }
    public void click() { _button.clicked(); }
}

public class UIMain {

  public static void main(String[] args) {
    UIType type = UIType.MOTIF;
    Factory factory = null;
    switch (type) {
      case MOTIF: factory = new MotifFactory(); break;
      case WINDOWS: factory = new WindowsFactory(); break;
    }

    UI ui = new UI(factory);
    ui.create();
    ui.draw();
    ui.click()
  }
}
```
<a href="http://www.yes24.com/Product/Goods/12501269">자바 객체지향 디자인 패턴</a>


