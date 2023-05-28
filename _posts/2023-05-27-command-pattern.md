---
layout: post
title: "Command Pattern"
description: ""
date: 2023-05-27
tags: [pattern]
---

<a href="http://www.yes24.com/Product/Goods/12501269">자바 객체지향 디자인 패턴</a>

```java
public class Alarm {
  public void start() {
    System.out.println("Alarming...");
  }
}

public class Lamp {
  public void turnOn() {
    System.out.println("Lamp On");
  }
}

enum Mode { LAMP, ALARM };

public class Button {
  private Lamp theLamp;
  private Alarm theAlarm;
  private Mode theMode;

  public Button(Lamp theLamp, Alarm theAlarm) {
    this.theLamp = theLamp;
    this.theAlarm = theAlarm;
  }

  public void setMode(Mode mode) {
    this.theMode = mode;
  }

  public void pressed() {
    switch (theMode) {
      case LAMP -> theLamp.turnOn();
      case ALARM -> theAlarm.start();
    }
  }
}

public class Client {

  public static void main(String[] args) {
    Lamp lamp = new Lamp();
    Alarm alarm = new Alarm();
    Button button = new Button(lamp, alarm);

    button.setMode(Mode.LAMP);
    button.pressed();

    button.setMode(Mode.ALARM);
    button.pressed();
  }
}
```

![0](/assets/images/command-pattern/0.png)

버튼을 누르는 동작에 따라 다른 기능을 실행하게 하려면 기능이 실행되는 시점에 필요한 프로그램(메서드)을 선택할 수 있어야 한다. - p236

새로운 기능을 추가할 때마다 Button 클래스를 수정해야 한다면 OCP를 위반한다. (내부 코드를 수정하는 것이 아닌 확장해야 한다.)

Button 클래스에서 구현하는 것이 아닌 외부로부터 제공받아 호출하는 방법으로 해결할 수 있다. (생성자 주입)

```java
public interface Command {
  public abstract void execute();
}

public class Alarm {
  public void start() {
    System.out.println("Alarming...");
  }
}

public class AlarmOnCommand implements Command {
  private Alarm theAlarm;

  public AlarmOnCommand(Alarm theAlarm) {
    this.theAlarm = theAlarm;
  }

  @Override
  public void execute() {
    theAlarm.start();
  }
}

public class Lamp {
  public void turnOn() {
    System.out.println("Lamp On");
  }
}

public class LampOnCommand implements Command {
  private Lamp theLamp;

  public LampOnCommand(Lamp theLamp) {
    this.theLamp = theLamp;
  }

  @Override
  public void execute() {
    theLamp.turnOn();
  }
}

public class Button {
  private Command theCommand;

  public Button(Command theCommand) {
    this.theCommand = theCommand;
  }

  public void setCommand(Command newCommand) {
    this.theCommand = newCommand;
  }

  public void pressed() {
    theCommand.execute();
  }
}

public class Client {

  public static void main(String[] args) {
    Lamp lamp = new Lamp();
    Command lampOnCommand = new LampOnCommand(lamp);

    Button button1 = new Button(lampOnCommand);
    button1.pressed();

    Alarm alarm = new Alarm();
    Command alarmOnCommand = new AlarmOnCommand(alarm);

    Button button2 = new Button(alarmOnCommand);
    button2.pressed();

    button2.setCommand(lampOnCommand);
    button2.pressed();
  }
}
```

![1](/assets/images/command-pattern/1.png)

위와 같이 코드를 작성하면 사용자 입장에서는 갈아끼우기만 하면 되며 Button 클래스 입장에서는 버튼을 눌렀을 때 어떤 기능을 수행해야 하는지, 어떻게 구현되었는지 알 필요 없이 Command 인터페이스의 execute() 메서드만 호출하면 된다.

즉 Lamp와 Alarm 클래스는 각 역할에 맞는 기능이 구현되어 있으며, LampOnCommand, AlarmOnCommand 클래스에서 execute() 메서드를 구현하여 어떻게 수행할 것인지 컨트롤한다.

더 나아가서 Lamp나 Alarm을 끄는 기능도 별도의 Command 클래스를 만들어서 setCommand 메서드를 통해 기능을 변경한 후 pressed 메서드를 호출하면 된다.

### 더 알아보기

Command Pattern은 <a href="https://hyuunnn.github.io/2023/05/27/state-pattern/">State Pattern</a>과 유사한 형태를 띄고 있다. (OCP를 해결하기 위해 각 기능을 클래스로 구현하여 갈아끼운다는 점에서)

하지만 State Pattern은 예를 들어 버튼을 눌렀을 때 `ON -> OFF` 혹은 `OFF -> ON`과 같이 상태가 수시로 바뀌어야 하는데 이러한 기능이 메서드(`button_pushed`)로 추상화되어 있다.

Command Pattern은 수시로 변하는게 아닌 다른 기능을 수행하고 싶을 때 변경하는 개념이라고 볼 수 있겠다.

<a href="http://www.yes24.com/Product/Goods/108192370">헤드 퍼스트 디자인 패턴</a>
