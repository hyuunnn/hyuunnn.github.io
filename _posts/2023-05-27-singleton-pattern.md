---
layout: post
title: "Singleton Pattern"
description: ""
date: 2023-05-27
tags: [pattern]
---

<a href="http://www.yes24.com/Product/Goods/12501269">자바 객체지향 디자인 패턴</a>

Singleton Pattern은 인스턴스가 오직 하나만 생성되어야 하는 경우에 사용하는 디자인 패턴이다. (불필요한 객체 생성을 방지할 수 있다.)

```java
public class Printer {
    private static Printer printer = new Printer();
    private Printer() { }

    public static Printer getPrinter() {
        return printer;
    }

    public void print(String str) {
        System.out.println(str);
    }
}
```

정적 변수를 사용하여 `Printer` 클래스의 인스턴스를 하나만 생성하고, 생성자를 private으로 선언하여 외부에서 인스턴스를 생성할 수 없도록 한다. (정적 변수는 객체 생성되기 전 클래스가 메모리에 로딩될 때 초기화가 한 번만 실행되며, 종료될 때까지 메모리에 상주하고 모든 객체에서 참조할 수 있다. - p198)

`Printer` 클래스를 사용할 때는 `getPrinter()` 메서드를 사용하여 인스턴스를 얻어온다.

하지만 멀티 쓰레드에서는 여러 개의 인스턴스가 생성될 수 있다. (동기화 처리를 하지 않았기 때문에)

TODO: 이 책에도 싱글톤의 멀티 쓰레드 내용이 있는데 이펙티브 자바랑 같이 보면서 공부해보기

<a href="http://www.yes24.com/Product/Goods/65551284">Effective Java</a>

### Item 1 - 생성자 대신 정적 팩터리 메서드를 고려하라

**호출될 때마다 인스턴스를 새로 생성하지 않아도 된다.**

생성한 인스턴스를 캐싱하여 재활용하기 때문에 불필요한 객체 생성을 피할 수 있다. (반복되는 요청에도 같은 객체를 반환하여 인스턴스를 통제할 수 있고, 인스턴스가 하나임을 보장할 수 있다.)

**정적 팩터리 메서드는 프로그래머가 찾기 어렵다.**

생성자는 객체를 생성했을 때 자동으로 동작하지만 정적 팩터리 메서드는 어떤 메서드들이 구현되어 있는지 찾아야 한다. 그래서 API 문서화를 꼼꼼히 하고 메서드명을 정의할 때 규약을 따르는 것을 권장하고 있다. 

* `instance` 혹은 `getInstance`
    * 매개변수를 받는다면 매개변수로 명시한 인터페이스를 반환하지만, 같은 인스턴스임을 보장하지는 않는다.
    * 매개변수를 받지 않는다면 항상 같은 인스턴스임을 보장한다.

```java
StackWalker luke = StackWalker.getInstance(options);
Printer printer = Printer.getInstance();
```

### Item 3 - private 생성자나 열거 타입으로 싱글턴임을 보증하라

**클래스를 싱글턴으로 만들면 이를 사용하는 클라이언트를 테스트하기가 어려워질 수 있다.**

인터페이스를 구현해서 만든 싱글턴이 아니라면 싱글턴 인스턴스를 가짜(mock) 구현으로 대체할 수 없기 때문이다.

<a href="https://velog.io/@skyepodium/%EC%8B%B1%EA%B8%80%ED%86%A4-%EC%A0%95%EB%A7%90-%ED%95%9C%EA%B0%9C%EB%A7%8C-%EB%A7%8C%EB%93%A4%EC%96%B4%EB%B4%85%EC%8B%9C%EB%8B%A4">[자바] 싱글톤 정말 한개만 만들어봅시다.</a>

TODO: 시험 끝나고 딥하게 공부해보기

<a href="http://www.yes24.com/Product/Goods/108192370">헤드 퍼스트 디자인 패턴</a>
