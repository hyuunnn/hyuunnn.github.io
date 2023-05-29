---
layout: post
title: "Template Method Pattern"
description: ""
date: 2023-05-28
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

<a href="http://www.yes24.com/Product/Goods/108192370">헤드 퍼스트 디자인 패턴</a>
