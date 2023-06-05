---
layout: post
title: "Facade Pattern"
description: ""
date: 2023-06-04
tags: [pattern]
---

클라이언트가 여러 클래스들과 복잡한 interaction을 하고 있으며, 중복되어 재사용되지 않는 복잡한 상황이라면 복잡도가 올라간다.

이때 중간 수준의 Facade 클래스를 만들어서 클라이언트와의 관계를 이전보다 단순하게 설계할 수 있다.

예를 들어 엘리베이터의 문을 닫고 여는 각각의 동작을 Facade 클래스로 묶어서 사용한다면 관계를 단순하게 하고 재사용, 확장에도 이점을 준다.

![0](/assets/images/facade-pattern/0.png)

```java
public interface IDoor {
  public void open();
  public void close();
}

public class FloorDoor implements IDoor {

  private int floor;

  public FloorDoor(int floor) {
    this.floor = floor;
  }

  @Override
  public void open() {
    System.out.println(floor + "th Floor Door Open");
  }

  @Override
  public void close() {
    System.out.println(floor + "th Floor Door Close");
  }
}

public class ElevatorDoor implements IDoor {

  @Override
  public void open() {
    System.out.println("Elevator Door Open");
  }

  @Override
  public void close() {
    System.out.println("Elevator Door Close");
  }
}

public interface IMotor {
  public void stop();
  public void move(int direction);
}

public class ElevatorMotor implements IMotor {

  @Override
  public void stop() {
    System.out.println("Elevator Motor Stop");
  }

  @Override
  public void move(int direction) {
    System.out.println("Elevator Motor Move with direction " + direction);
  }
}

public class DoorTimerTask extends TimerTask {

  private ElevatorController controller;

  public DoorTimerTask(ElevatorController controller) {
    this.controller = controller;
  }

  @Override
  public void run() {
    controller.doorTimeout();
  }
}

public class DoorTimer {

  private Timer timer = new Timer();

  public void start(ElevatorController controller) {
    DoorTimerTask task = new DoorTimerTask(controller);
    timer.schedule(task, 5000);
  }

  public void stop() {
    timer.cancel();
  }
}

public class ElevatorController {

  private int curFloor = 1;
  private List<Integer> destinations = new ArrayList<>();

  private IMotor motor;
  private boolean isMoving = false;
  private ElevatorDoor elevatorDoor;
  private List<FloorDoor> floorDoors;
  private DoorTimer doorTimer;

  public ElevatorController(IMotor motor, ElevatorDoor elevatorDoor, List<FloorDoor> floorDoors,
      DoorTimer doorTimer) {
    this.motor = motor;
    this.elevatorDoor = elevatorDoor;
    this.floorDoors = floorDoors;
    this.doorTimer = doorTimer;
  }

  public void goTo(int destination) {
    if (!destinations.contains(destination)) destinations.add(destination);

    if (isMoving == false) {
      int direction = determineMovingDirection();
      if (direction != 0) {
        motor.move(direction);
        isMoving = true;
      }
    }
  }

  public void approaching(int floor) {
    System.out.println("\nApproaching " + floor + "th floor");
    curFloor = floor;

    if (needToStop(floor)) {
      motor.stop();
      isMoving = false;
      elevatorDoor.open(); //
      floorDoors.get(curFloor - 1).open(); //
      doorTimer.start(this); //
      destinations.remove(Integer.valueOf(floor));
    }
  }
  private boolean needToStop(int floor) { return destinations.contains(floor); }

  public void doorTimeout() {
    elevatorDoor.close(); //
    floorDoors.get(curFloor - 1).close(); //
    doorTimer.stop(); //
    int direction = determineMovingDirection();
    if (direction != 0) {
      motor.move(direction);
      isMoving = true;
    }
  }

  private int determineMovingDirection() {
    if (destinations.size() == 0) return 0;
    int destination = destinations.get(0);
    if (destination > curFloor) return 1;
    return -1;
  }

  public void openDoor() {
    if (isMoving) return;
    elevatorDoor.open(); //
    floorDoors.get(curFloor - 1).open(); //
    doorTimer.start(this); //
  }

  public void closeDoor() {
    if (isMoving) return;
    elevatorDoor.close(); //
    floorDoors.get(curFloor - 1).close(); //
    doorTimer.stop(); //
  }
}

public class Client {

  public static void main(String[] args) {
    final int MAX_FLOOR = 10;
    ElevatorMotor elevatorMotor = new ElevatorMotor();
    ElevatorDoor elevatorDoor = new ElevatorDoor();
    List<FloorDoor> floorDoors = new ArrayList<>(MAX_FLOOR);
    for (int i = 0; i < MAX_FLOOR; i++)
      floorDoors.add(new FloorDoor(i+1));
    DoorTimer timer = new DoorTimer();
    ElevatorController controller = new ElevatorController(
        elevatorMotor, elevatorDoor, floorDoors, timer);

    controller.goTo(4);
    controller.approaching(2);
    controller.approaching(3);
    controller.approaching(4);
  }
}
```

위 예제 코드를 보면 주석으로 표시한 부분이 중복되어 사용됨을 확인할 수 있다.

또한 ElevatorController 클래스가 엘리베이터의 문을 닫고(elevatorDoor) 이전 층의 문을 닫고(floorDoors) 타이머를 종료하는(doorTimer) 행위와 이와 반대되는 행위(시작) 각각을 제어하고 있다.

하지만 이를 Facade 클래스로 만들어서 사용한다면 구조가 단순해진다.

```java
public interface IDoorTimeout {
  public void doorTimeout();
}

public class DoorController {
  private ElevatorDoor elevatorDoor;
  private List<FloorDoor> floorDoors;
  private DoorTimer doorTimer;
  public DoorController(ElevatorDoor elevatorDoor, List<FloorDoor> floorDoors, DoorTimer doorTimer) {
    this.elevatorDoor = elevatorDoor;
    this.floorDoors = floorDoors;
    this.doorTimer = doorTimer;
  }

  public void setDoorTimeout(IDoorTimeout doorTimeout) {
    doorTimer.setDoorTimeout(doorTimeout);
  }

  public void openDoor(int floor) {
    elevatorDoor.open();
    floorDoors.get(floor - 1).open();
    doorTimer.start();
  }

  public void closeDoor(int floor) {
    elevatorDoor.close();
    floorDoors.get(floor - 1).close();
    doorTimer.stop();
  }
}

public class ElevatorController implements IDoorTimeout {

  private int curFloor = 1;
  private List<Integer> destinations = new ArrayList<>();
  private IMotor motor;
  private boolean isMoving = false;

  private DoorController doorController;

  public ElevatorController(IMotor motor, DoorController doorController) {
    this.motor = motor;
    this.doorController = doorController;
  }

  public void goTo(int destination) {
    // 이전 코드와 동일
  }

  public void approaching(int floor) {
    System.out.println("\nApproaching " + floor + "th floor");
    curFloor = floor;

    if (needToStop(floor)) {
      motor.stop();
      isMoving = false;

      doorController.openDoor(curFloor);
      destinations.remove(Integer.valueOf(floor));
    }
  }
  private boolean needToStop(int floor) { return destinations.contains(floor); }

  public void doorTimeout() {
    doorController.closeDoor(curFloor);

    int direction = determineMovingDirection();
    if (direction != 0) {
      motor.move(direction);
      isMoving = true;
    }
  }

  private int determineMovingDirection() {
    // 이전 코드와 동일
  }

  public void openDoor() {
    if (isMoving) return;

    doorController.openDoor(curFloor);
  }

  public void closeDoor() {
    if (isMoving) return;

    doorController.closeDoor(curFloor);
  }
}

public class DoorTimer {

  private Timer timer = new Timer();
  private DoorTimerTask task;

  public DoorTimer() {
    this.task = new DoorTimerTask();
  }

  public void setDoorTimeout(IDoorTimeout doorTimeout) {
    task.setDoorTimeout(doorTimeout);
  }

  public void start() { timer.schedule(task, 5000); }
  public void stop() { timer.cancel(); }
}

public class DoorTimerTask extends TimerTask {

  private IDoorTimeout doorTimeout;

  public void setDoorTimeout(IDoorTimeout doorTimeout) {
    this.doorTimeout = doorTimeout;
  }

  @Override
  public void run() {
    doorTimeout.doorTimeout();
  }
}
```

DoorController라는 별도의 컨트롤러 클래스를 만들어서 Door를 관리하기 때문에 ElevatorController 입장에서는 DoorController만 사용하면 되므로 구조를 간단하게 변경할 수 있다.

또한 이전 코드는 DoorTimer와 DoorTimerTask 클래스가 ElevatorController 클래스에 직접 접근하고 있었는데 (ElevatorController의 다양한 메서드들을 호출할 수 있었다.)

IDoorTimeout 인터페이스를 사용하면서 DIP, ISP 원칙을 지키게 된다. (ElevatorController의 다른 메서드는 사용하지 못하며, 인터페이스의 구현체인 doorTimeout 메서드만 사용 가능하다. )

<a href="http://www.yes24.com/Product/Goods/108192370">헤드 퍼스트 디자인 패턴</a>
