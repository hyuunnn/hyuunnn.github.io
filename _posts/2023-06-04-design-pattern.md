---
layout: post
title: "Design Pattern"
description: ""
date: 2023-06-04
tags: [pattern]
---

시스템은 규모가 크기 때문에 일반적으로 3가지의 설계 스텝이 존재한다.

Top Level Design은 시스템 전체에 걸친 이슈들, 컴포넌트 간의 커뮤니케이션은 어떻게 할 것인지 등을 정리한다.

Component Level Design은 클래스간의 관계를 정리한다

Detailed Design은 데이터 멤버, 함수들의 정의를 의미한다.

|디자인|패턴|
|-----|---|
|Top Level Design|Architectural Pattern|
|Component Level Design|Design Pattern|
|Detailed Design|Code Pattern|

디자인 패턴이라는 용어가 처음 나왔을 때 Component Level Design에 관한 패턴을 의미했었다.

하지만 Top level, Component, Detailed 모두 디자인 활동이라고 볼 수 있기 때문에 위 3개를 통틀어서 말하기도 한다.

* <a href="http://www.yes24.com/Product/Goods/9027376">패턴 랭귀지</a> - 크리스토퍼 알렉산더
    * 패턴을 사용하면 똑같은 일을 반복하는 문제를 해결할 수 있다. (효율적인 설계가 가능하다.)

### GoF Design Pattern

<a href="https://en.wikipedia.org/wiki/Design_Patterns">GoF Pattern</a>에서는 3가지의 디자인 패턴으로 분류하고 있다.

|패턴|설명|
|----|---|
|Creational Patterns|클래스를 바탕으로 object를 어떻게 만드는게 바람직한가?|
|Structural Patterns|좀 더 큰 규모의 시스템을 개발하기 위해 클래스, 오브젝트를 어떻게 구성하는게 바람직한가?|
|Behavioral Patterns|object 간의 커뮤니케이션을 어떻게 하는 것이 필요한가?|

||Creational|Structural|Behavioral|
|-|----------|----------|----------|
|Class-level|Factory Method|Adapter (class)|Interpreter, Template Method|
|Object-level|Abstract Factory, Builder, Prototype, Singleton|Adapter (object), Bridge, Composite, Decorator, Facade, Flyweight, Proxy|Chain of Responsibility, Command, Iterator, Mediator, Memento, Observer, State, Strategy, Visitor|

패턴 자체를 이해하기 쉽진 않다. 또한 일반적인 상황에서 사용되는 패턴도 있지만 특수한 상황에서 사용되는 패턴도 있기 때문에 범용적인 패턴을 먼저 학습하는게 도움이 될 수 있다.

### 디자인 패턴의 장단점

* 전문가들로부터 검증된 설계를 활용하여 객체 지향 설계에 도움을 줄 수 있다. (다른 사람의 성공으로부터 배우는 것)

* 특히 패턴은 개발자간 의사소통의 용어로 사용될 수 있다.
    * 패턴의 수행 단계를 설명할 필요 없이 패턴 이름만 언급하면 된다. (간결하고 명확한 의사소통이 가능)

* 패턴의 목적은 성능을 높이거나 신뢰성을 높이는게 아닌 재사용성을 높일 수 있는 방법을 제시하는 것이다.

* 패턴을 사용하게 되면 오히려 시스템 구조가 복잡해질 수 있다.

* 패턴을 모르는 사람의 입장에서는 코드를 이해하기 어려울 수 있다. (적절한 패턴 사용, 과용하지 않기)

### 링크 모음

<a href="https://en.wikipedia.org/wiki/Design_pattern">design pattern - wikipedia</a>

<a href="https://en.wikipedia.org/wiki/Software_design_pattern">software design pattern - wikipedia</a>

<a href="https://github.com/iluwatar/java-design-patterns">java-design-patterns</a>

<a href="https://github.com/kamranahmedse/design-patterns-for-humans">design-patterns-for-humans</a>

<a href="https://github.com/DovAmir/awesome-design-patterns">awesome-design-patterns</a>

<a href="https://refactoring.guru/">refactoring guru</a>

<a href="https://johngrib.github.io/wiki/pattern/quotes/">디자인 패턴 인용문 모음 - johngrib</a>
