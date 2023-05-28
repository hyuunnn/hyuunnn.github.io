---
layout: post
title: "State Pattern"
description: ""
date: 2023-05-27
tags: [pattern]
---

<a href="http://www.yes24.com/Product/Goods/12501269">자바 객체지향 디자인 패턴</a>

상태에 따라서 다르게 동작하는 코드를 작성할 때 조건문으로 어떤 상태인지 하나씩 확인하는 코드를 작성한다면, 복잡한 조건식이 만들어지며 결과적으로 코드를 이해하거나 수정하기 어렵게 만든다.

```java
public class Light {
  private static int ON = 0;
  private static int OFF = 1;
  private static int SLEEPING = 2;
  private int state;

  public Light() {
    state = OFF;
  }

  public void on_button_pushed() {
    if (state == ON) {
      System.out.println("반응 없음");
      state = SLEEPING;
    }
    else if (state == SLEEPING) {
      System.out.println("Light On");
      state = ON;
    }
    else {
      System.out.println("Light On");
      state = ON;
    }
  }

  public void off_button_pushed() {
    if (state == OFF) {
      System.out.println("반응 없음");
    }
    else if (state == SLEEPING) {
      System.out.println("Light Off");
      state = OFF;
    }
    else {
      System.out.println("Light Off");
      state = OFF;
    }
  }
}

public class Client {
    public static void main(String[] args) {
        Light light = new Light();
        light.off(); // 반응 없음
        light.on();
        light.off();
    }
}
```

위 코드는 형광등의 ON, OFF 기능을 가지고 있다. 그리고 형광등이 켜져있을 때 ON 버튼을 누르면 취침등(SLEEPING) 모드로 전환된다.

만약에 새로운 기능이 추가된다면 복잡한 조건 사이에 상태 변화를 추가해야 한다.

그렇게 만들어진 코드는 어떻게 동작하는지 이해하기 어렵고 새로운 상태를 추가할 때 연관된 모든 메서드에 반영하기 위해 코드를 수정해야 한다.

**변하는 부분을 찾아서 캡슐화하고, 상태가 변경되어도 상관없는 독립적인 코드로 수정해야 한다.**

![0](/assets/images/state-pattern/0.png)

```java
interface State {
  public void on_button_pushed(Light light);
  public void off_button_pushed(Light light);
}

public class Light {
  private State state;

  public Light() {
    state = OFF.getInstance();
  }

  public void setState(State state) {
    this.state = state;
  }

  public void on_button_pushed() {
    state.on_button_pushed(this);
  }

  public void off_button_pushed() {
    state.off_button_pushed(this);
  }
}

public class ON implements State {

  private static ON on = new ON();
  private ON() {}

  public static ON getInstance() {
    return on;
  }

  @Override
  public void on_button_pushed(Light light) {
    System.out.println("반응 없음");
  }

  @Override
  public void off_button_pushed(Light light) {
    System.out.println("Light Off!");
    light.setState(OFF.getInstance());
  }
}

public class OFF implements State {

  private static OFF off = new OFF();
  private OFF() {}

  public static OFF getInstance() {
    return off;
  }

  @Override
  public void on_button_pushed(Light light) {
    System.out.println("Light On!");
    light.setState(ON.getInstance());
  }

  @Override
  public void off_button_pushed(Light light) {
    System.out.println("반응 없음");
  }
}
```

if 문으로 각 경우를 처리하지 않고, 상태에 따른 행위를 각각의 클래스로 분리하였기 때문에 상태 변화를 if, switch 등으로 나타낼 필요가 없다. (상태 변경도 각 상태(setState)에서 처리하기 때문이다.)

이제 Light 클래스는 state 변수를 통해 상태를 처리하기 때문에 상태가 변경되어도 전혀 영향을 받지 않으며, 변경 여부도 알 필요가 없다.

<a href="http://www.yes24.com/Product/Goods/108192370">헤드 퍼스트 디자인 패턴</a>
