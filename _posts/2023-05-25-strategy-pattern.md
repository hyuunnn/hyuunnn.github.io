---
layout: post
title: "Strategy Pattern"
description: ""
date: 2023-05-25
tags: [pattern]
---

<a href="http://www.yes24.com/Product/Goods/12501269">자바 객체지향 디자인 패턴</a>

Strategy Pattern은 전략을 쉽게 바꿀 수 있도록 해주는 디자인 패턴이다. (전략이란 어떤 목적을 달성하기 위해 일을 수행하는 방식이나 해결 방법 등을 의미한다.)

예를 들어 게임 캐릭터가 상황에 따라서 공격이나 행동을 바꿔야 할 때 전략 패턴은 유용하다.

![0](/assets/images/strategy-pattern/0.png)

```java
public abstract class Robot {
    private String name;

    public Robot(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public abstract void attack();
    public abstract void move();
}

public class TaekwonV extends Robot {
    public TaekwonV(String name) {
        super(name);
    }

    @Override
    public void attack() {
        System.out.println("I have Missile and can attack with it.");
    }

    @Override
    public void move() {
        System.out.println("I can only walk.");
    }
}

public class Atom extends Robot {
    public Atom(String name) {
        super(name);
    }

    @Override
    public void attack() {
        System.out.println("I have strong punch and can attack with it.");
    }

    @Override
    public void move() {
        System.out.println("I can fly.");
    }
}
```

`TaekwonV`와 `Atom`은 둘 다 로봇이기 때문에 `Robot` 클래스를 상속하여 구현하였다. 또한 두 로봇은 공격과 이동 방법이 다르기 때문에 추상 메서드로 선언되어 있다.

하지만 `TaekwonV`의 공격이나 이동 방법을 수정하려면 내부 코드를 수정해야 하기 때문에 OCP를 위반하고 있다.

또한 새로운 로봇을 추가할 때 `TaekwonV`와 같은 공격 방식을 사용한다면 기능을 중복 구현해야 하는 상황이 발생한다. (공격 방식에 문제가 있다면 해당 방식을 사용하는 모든 로봇을 수정해야 한다.)

이를 해결하는 방법은 공격과 이동 방법을 인터페이스로 분리하고, 구현하는 클래스를 만들어야 한다.

![1](/assets/images/strategy-pattern/1.png)

위와 같이 구현하면 로봇의 입장에서는 그저 `attack()`과 `move()` 메서드를 호출하기만 하면 된다. (구현된 코드의 수정 여부를 알 필요가 없다.) 

수정할 때는 `setMovingStrategy`, `setAttackStrategy` 메서드를 사용하여 외부에서 변경하기 때문에 OCP를 만족하는 코드가 된다.

```java
public abstract class Robot {
    private String name;
    private MovingStrategy movingStrategy;
    private AttackStrategy attackStrategy;

    public Robot(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public void move() {
        movingStrategy.move();
    }

    public void attack() {
        attackStrategy.attack();
    }

    public void setMovingStrategy(MovingStrategy movingStrategy) {
        this.movingStrategy = movingStrategy;
    }

    public void setAttackStrategy(AttackStrategy attackStrategy) {
        this.attackStrategy = attackStrategy;
    }
}
```

```java
public class Atom extends Robot {
    public Atom(String name) {
        super(name);
    }
}

public class TaekwonV extends Robot {
    public TaekwonV(String name) {
        super(name);
    }
}

interface MovingStrategy {
    void move();
}

public class FlyingStrategy implements MovingStrategy {
    @Override
    public void move() {
        System.out.println("I can fly.");
    }
}

public class WalkingStrategy implements MovingStrategy {
    @Override
    public void move() {
        System.out.println("I can only walk.");
    }
}

interface AttackStrategy {
    void attack();
}

public class PunchStrategy implements AttackStrategy {
    @Override
    public void attack() {
        System.out.println("I have strong punch and can attack with it.");
    }
}

public class MissileStrategy implements AttackStrategy {
    @Override
    public void attack() {
        System.out.println("I have Missile and can attack with it.");
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        Robot taekwonV = new TaekwonV("TaekwonV");
        Robot atom = new Atom("Atom");

        taekwonV.setMovingStrategy(new WalkingStrategy());
        taekwonV.setAttackStrategy(new MissileStrategy());

        atom.setMovingStrategy(new FlyingStrategy());
        atom.setAttackStrategy(new PunchStrategy());

        taekwonV.move();
        taekwonV.attack();

        atom.move();
        atom.attack();
    }
}
```

## 좀 더 고민해보기

위와 같은 코드는 `setMovingStrategy`, `setAttackStrategy` 메서드를 사용할 때마다 새로운 객체가 생성된다.

하지만 움직이거나 공격하는 방법은 항상 같기 때문에 싱글톤 패턴을 적용할 수 있다.

```java
public class PunchStrategy implements AttackStrategy {

  private static final PunchStrategy PUNCH = new PunchStrategy();

  private PunchStrategy() {
  }

  public static Punch getInstance() {
    return PUNCH;
  }

  @Override
  public void attack() {
    System.out.println("I have strong punch and can attack with it.");
  }
}

public class MissileStrategy implements AttackStrategy {

  private static final MissileStrategy MISSILE = new MissileStrategy();

  private MissileStrategy() {
  }

  public static Missile getInstance() {
    return MISSILE;
  }

  @Override
  public void attack() {
    System.out.println("I have Missile and can attack with it.");
  }
}
```

`WalkingStrategy`와 `FlyingStrategy`도 위와 유사한 코드 진행이기 때문에 생략한다.

```java
public class Main {

  public static void main(String[] args) {
    Robot taekwonV = new TaekwonV("taekwon");
    Robot atom = new Atom("atom");

    taekwonV.setMoving(Walking.getInstance());
    taekwonV.setAttack(Missile.getInstance());

    atom.setMoving(Flying.getInstance());
    atom.setAttack(Punch.getInstance());

    taekwonV.move();
    taekwonV.attack();

    atom.move();
    atom.attack();
  }
}
```

이제 교체할 때마다 새로운 객체를 생성하지 않고 `getInstance()`를 사용하여 재활용하고 있다.

이를 싱글톤 패턴이라고 한다.
