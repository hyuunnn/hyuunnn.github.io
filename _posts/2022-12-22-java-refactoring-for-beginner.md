---
layout: post
title: "자바로 배우는 리팩토링 입문"
description: ""
date: 2022-12-22
tags: ["book"]
---

## null 객체 도입

"하루 건너 먹어야 하는 약이 있는데, 하루 건너서 먹다보면 까먹을 수 있다. 약을 매일 먹는 대신 가짜 약과 진짜 약을 교대로 먹는게 어떤가? 그저 꾸준히 먹으면 된다." - p108

```java
// 리팩토링 전
public class Person {

    private final Label _name;
    private final Label _mail;

    public Person(Label name, Label mail) {
        _name = name;
        _mail = mail;
    }

    public Person(Label name) {
        this(name, null);
    }

    public void display() {
        if (_name != null) {
            _name.display();
        }
        if (_mail != null) {
            _mail.display();
        }
    }

    public String toString() {
        String result = "[ Persion: ";

        result += " name=";
        if (_name == null) {
            result += "\"(none)\"";
        } else {
            result += _name;
        }

        result += " mail=";
        if (_mail == null) {
            result += "\"(none)\"";
        } else {
            result += _mail;
        }

        return result;
    }
}

public class Label {

    private final String _label;

    public Label(String label) {
        _label = label;
    }

    public void display() {
        System.out.println("display: " + _label);
    }

    public String toString() {
        return "\"" + _label + "\"";
    }
}
```

```java
// 리팩토링 후
public class Person {

    private final Label _name;
    private final Label _mail;

    public Person(Label name, Label mail) {
        _name = name;
        _mail = mail;
    }

    public Person(Label name) {
        this(name, new NullLabel());
    }

    public void display() {
        _name.display();
        _mail.display();
    }

    public String toString() {
        return "[ Person: name=" + _name + " mail=" + _mail + " ]";
    }
}

public class Label {
    
    private final String _label;
    
    public Label(String label) {
        _label = label;
    }

    public void display() {
        System.out.println("display: " + _label);
    }

    public String toString() {
        return "\"" + _label + "\"";
    }

    public boolean isNull() {
        return false;
    }
}

public class NullLabel extends Label { 

    public NullLabel() {
        super("(none)");
    }

    @Override
    public void display() {

    }

    @Override
    public boolean isNull() {
        return true;
    }
}
```

```java
// 팩터리 메서드 패턴, 싱글톤 패턴 적용
public class Label {
    
    private final String _label;
    
    public Label(String label) {
        _label = label;
    }

    public void display() {
        System.out.println("display: " + _label);
    }

    public String toString() {
        return "\"" + _label + "\"";
    }

    public boolean isNull() {
        return false;
    }

    public static Label newNull() {  // 팩터리 메서드 패턴 (NullLabel 클래스를 숨겼다.)
        return NullLabel.getInstance();
    }

    private static class NullLabel extends Label { 

        private static final NullLabel singleton = new NullLabel(); // 싱글톤 패턴 (1번 생성한 객체를 재사용한다.)

        private static NullLabel getInstance() {
            return singleton;
        }

        public NullLabel() {
            super("(none)");
        }

        @Override
        public void display() {

        }

        @Override
        public boolean isNull() {
            return true;
        }
    }
}

public class Person {
    
    ...
    public Person(Label name) {
        this(name, Label.newNull());
    }
    ...
}
```


### 패턴 중독에 빠지지 않기

`null` 확인이 많다면 `null` 객체 도입도 좋고, 클래스명을 은폐하기 위해 팩토리 메서드 패턴, 불필요한 생성을 막기 위한 싱글톤 패턴 등은 다 좋은 생각이다.

하지만 `new`가 있다면 무조건 팩토리 메서드로 만든다던지 모든 코드를 싱글톤으로 만든다던지 이런 접근 방법은 옳지않다.

trade off를 고려하여 리팩토링을 수행해야 한다.

<a href="http://www.yes24.com/Product/Goods/14752528">패턴을 위한 리팩터링</a>에서 이런 부류를 패턴 중독이라고 부른다고 한다.

### 기존 클래스를 수정할 수 없을 떄

`Null`이라는 인터페이스를 만들고 `NullLabel` 클래스가 인터페이스를 구현하여 `obj instanceof Null` 표현식을 사용하면 된다.

OOP에서 `instanceof`는 smell이 있다고 한다. 이런 인터페이스를 마커 인터페이스라고 하는데, 상위 클래스를 변경할 수 없는 제약이 있다면 이러한 방법이 해결책이라고 한다.

`java.io.Serializable`가 마커 인터페이스라고 한다.

## 메서드 추출

메서드 이름은 동사 + 명사 (`printBorder`)

"무엇을 하는지" 알 수 있게 지어야한다. "어떻게 하는가"는 구현 코드가 변경됨에 따라 메서드명도 변경해야 하기 때문이다.

괜찮은 메서드명이 떠오르지 않는다면 코드를 제대로 파악하지 못했다는 증거이다. 코드가 긴 경우에도 떠오르지 않고, 주석을 작성하고 싶어지곤 한다.

네이밍을 잘하는건 프로그래머의 숙명이다. 참조 문서나 소스 코드 등을 읽을 때도 나라면 어떤 이름을 붙일까? 고민하는 습관이 중요하다.


