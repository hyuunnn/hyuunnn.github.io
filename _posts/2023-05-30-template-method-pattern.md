---
layout: post
title: "Template Method Pattern"
description: ""
date: 2023-05-30
tags: [pattern]
---

<a href="http://www.yes24.com/Product/Goods/12501269">자바 객체지향 디자인 패턴</a>

```java
public enum DoorStatus {
  CLOSED, OPENED
}

public enum Direction {
  UP, DOWN
}

public enum MotorStatus {
  MOVING, STOPPED
}

public class Door {
  private DoorStatus doorStatus;

  public Door() {
    this.doorStatus = DoorStatus.CLOSED;
  }

  public DoorStatus getDoorStatus() {
    return doorStatus;
  }

  public void close() {
    doorStatus = DoorStatus.CLOSED;
  }

  public void open() {
    doorStatus = DoorStatus.OPENED;
  }
}

public class HyundaiMotor {
  private Door door;
  private MotorStatus motorStatus;

  public HyundaiMotor(Door door) {
    this.door = door;
    motorStatus = MotorStatus.STOPPED;
  }

  private void moveHyundaiMotor(Direction direction) {
    // Hyundai Motor를 구동시킴
  }

  public MotorStatus getMotorStatus() {
    return motorStatus;
  }

  private void setMotorStatus(MotorStatus motorStatus) {
    this.motorStatus = motorStatus;
  }

  public void move(Direction direction) {
    MotorStatus motorStatus = getMotorStatus();
    if (motorStatus == MotorStatus.MOVING)
      return;

    DoorStatus doorStatus = door.getDoorStatus();
    if (doorStatus == DoorStatus.OPENED)
      door.close();

    moveHyundaiMotor(direction);
    setMotorStatus(MotorStatus.MOVING);
  }
}

public class Client {

  public static void main(String[] args) {
    Door door = new Door();
    HyundaiMotor hyundaiMotor = new HyundaiMotor(door);
    hyundaiMotor.move(Direction.UP);
  }
}
```

위와 같은 코드를 작성했을 때 LG에서 만든 모터를 구현한다고 생각해보자.

```java
public class LGMotor {
  private Door door;
  private MotorStatus motorStatus;

  public LGMotor(Door door) {
    this.door = door;
    this.motorStatus = MotorStatus.STOPPED;
  }

  private void moveLGMotor(Direction direction) {
    // LG Motor를 구동시킴
  }

  public MotorStatus getMotorStatus() {
    return motorStatus;
  }

  private void setMotorStatus(MotorStatus motorStatus) {
    this.motorStatus = motorStatus;
  }

  public void move(Direction direction) {
    MotorStatus motorStatus = getMotorStatus();
    if (motorStatus == MotorStatus.MOVING) return;

    DoorStatus doorStatus = door.getDoorStatus();
    if (doorStatus == DoorStatus.OPENED) door.close();

    moveLGMotor(direction);
    setMotorStatus(MotorStatus.MOVING);
  }
}
```

LGMotor와 HyundaiMotor 클래스의 기능을 보면 매우 유사한 것을 볼 수 있다. 특히 move 메서드는 모터가 움직이고 있다면 종료, 문이 열려있다면 닫은 후 모터의 기능을 수행한다.

공통적으로 수행하는 모터의 기능을 클래스로 구현하고 세부적인 기능은 상속하여 구현한다면 중복 코드를 줄일 수 있겠다.

```java
public abstract class Motor {
  protected Door door;
  private MotorStatus motorStatus;

  public Motor(Door door) {
    this.door = door;
    this.motorStatus = MotorStatus.STOPPED;
  }

  public MotorStatus getMotorStatus() {
    return motorStatus;
  }

  protected void setMotorStatus(MotorStatus motorStatus) {
    this.motorStatus = motorStatus;
  }
}

public class HyundaiMotor extends Motor {
  private Door door;
  private MotorStatus motorStatus;

  public HyundaiMotor(Door door) {
    super(door);
  }

  private void moveHyundaiMotor(Direction direction) {
    // Hyundai Motor를 구동시킴
  }

  public void move(Direction direction) {
    MotorStatus motorStatus = getMotorStatus();
    if (motorStatus == MotorStatus.MOVING) return;

    DoorStatus doorStatus = door.getDoorStatus();
    if (doorStatus == DoorStatus.OPENED) door.close();

    moveHyundaiMotor(direction);
    setMotorStatus(MotorStatus.MOVING);
  }
}

public class LGMotor extends Motor {
  private Door door;
  private MotorStatus motorStatus;

  public LGMotor(Door door) {
    super(door);
  }

  private void moveLGMotor(Direction direction) {
    // LG Motor를 구동시킴
  }

  public void move(Direction direction) {
    MotorStatus motorStatus = getMotorStatus();
    if (motorStatus == MotorStatus.MOVING) return;

    DoorStatus doorStatus = door.getDoorStatus();
    if (doorStatus == DoorStatus.OPENED) door.close();

    moveLGMotor(direction);
    setMotorStatus(MotorStatus.MOVING);
  }
}
```

하지만 아직 중복 코드가 존재한다. 바로 move 메서드이다. 완전히 중복되진 않지만 부분적으로 중복되어 있다.

LG 모터로 동작할 것인지, Hyundai 모터로 동작할 것인지를 제외하면 move 메서드의 실행 구조는 같다. (모터의 제조사에 따라 구현 방법은 다르지만 기능 면에서는 동일하다.)

이때 template method pattern을 사용하면 부분적인 중복 코드도 제거할 수 있다.

```java
public abstract class Motor {
  protected Door door;
  private MotorStatus motorStatus;

  public Motor(Door door) {
    this.door = door;
    this.motorStatus = MotorStatus.STOPPED;
  }

  public MotorStatus getMotorStatus() {
    return motorStatus;
  }

  protected void setMotorStatus(MotorStatus motorStatus) {
    this.motorStatus = motorStatus;
  }

  public void move(Direction direction) {
    MotorStatus motorStatus = getMotorStatus();
    if (motorStatus == MotorStatus.MOVING) return;

    DoorStatus doorStatus = door.getDoorStatus();
    if (doorStatus == DoorStatus.OPENED) door.close();

    moveMotor(direction);
    setMotorStatus(MotorStatus.MOVING);
  }

  protected abstract void moveMotor(Direction direction);
}

public class HyundaiMotor extends Motor {
  public HyundaiMotor(Door door) {
    super(door);
  }

  protected void moveMotor(Direction direction) {
    // Hyundai Motor를 구동시킴
  }
}

public class LGMotor extends Motor {
  public LGMotor(Door door) {
    super(door);
  }

  protected void moveMotor(Direction direction) {
    // LG Motor를 구동시킴
  }
}
```

<a href="http://www.yes24.com/Product/Goods/108192370">헤드 퍼스트 디자인 패턴</a>
