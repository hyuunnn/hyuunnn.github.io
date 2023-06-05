---
layout: post
title: "Factory Method Pattern"
description: ""
date: 2023-06-04
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

이를 별도의 Factory 클래스를 만들어서 코드를 개선할 수 있다.

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

<a href="http://www.yes24.com/Product/Goods/12501269">자바 객체지향 디자인 패턴</a>

ㅁㄴㅇㅁㄴㅇ

<a href="http://www.yes24.com/Product/Goods/108192370">헤드 퍼스트 디자인 패턴</a>
