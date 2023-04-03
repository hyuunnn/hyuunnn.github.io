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

    public static Label newNull() {  // 팩터리 메서드 패턴 (NullLabel 클래스의 존재를 숨겼다.)
        return NullLabel.getInstance();
    }

    private static class NullLabel extends Label { // Static Nested Class

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

## 클래스 추출

### 불변 인터페이스

```java
public interface ImmutableAuthor {
    public String getName();
    public String getMail();
}

public class Author implements ImmutableAuthor {
    private String _name;
    private Strign _mail;
    public Author(String name, String mail) {
        _name = name;
        _mail = mail;
    }
    public String getName() { return _name; }
    public String getMail() { return _mail; }
    public void setName(String name) { _name = name; }
    public void setMail(Strign mail) { _mail = mail; }
    // ...
}

public class Book {
    ...
    private Author _author;
    ...
    public ImmutableAuthor getAuthor() {
        return _author;
    }
}
```

인터페이스에 getter 메서드만 있는 것이 핵심이다. `book.getAuthor().setName("authorName")` 코드처럼 `setName`을 호출했을 때 `ImmutableAuthor`는 `setName`을 모르기 때문에 컴파일 에러가 발생한다.

```
소프트웨어 개발은 정원 관리와 비슷합니다. 나무와 화초의 균형, 잎과 꽃의 균형을 맞추는 것처럼 소프트웨어 전체를 주기적으로 정리할 필요가 있습니다. 한쪽으로 치우치지 않고 골고루 관리해야 합니다. - p164
```

## 분류 코드를 하위 클래스로 치환

```java
public class Shape {
    public static final int TYPECODE_LINE = 0;
    public static final int TYPECODE_RECTANGLE = 1;
    public static final int TYPECODE_OVAL = 2;
    
    private final int _typecode;
    private final int _startx;
    private final int _starty;
    private final int _endx;
    private final int _endy;

    protected Shape(int typecode, int startx, int starty, int endx, int endy) {
        _typecode = typecode;
        _startx = startx;
        _endx = endx;
        _starty = starty;
        _endy = endy;
    }

    public int getTypecode() { return _typecode; }

    public String getName() {
        switch (_typecode) {
            case TYPECODE_LINE:
                return "LINE";
            case TYPECODE_RECTANGLE:
                return "RECTANGLE";
            case TYPECODE_OVAL:
                return "OVAL";
            default:
                return null;
        }
    }

    public void draw() {
        switch (_typecode) {
            case TYPECODE_LINE:
                drawLine();
                break;
            case TYPECODE_RECTANGLE:
                drawRectangle();
                break;
            case TYPECODE_OVAL:
                drawOval();
                break;
            default:
                ;
        }
    }
}
```

위 코드를 보면 기능이 추가될 때마다 switch 코드를 수정해야하므로 OCP를 위반하는 코드이다. Shape는 `draw()`, `getName()`을 호출할 때 어떻게 동작해야 하는지 알 필요 없이 단순히 메서드를 호출하면 된다.

```java
public abstract class Shape {
    public static final int TYPECODE_LINE = 0;
    public static final int TYPECODE_RECTANGLE = 1;
    public static final int TYPECODE_OVAL = 2;
    
    private final int _startx;
    private final int _starty;
    private final int _endx;
    private final int _endy;

    public static Shape create(int typecode, int startx, int starty, int endx, int endy) {
        switch (typecode) {
            case TYPECODE_LINE:
                return new ShapeLine(startx, starty, endx, endy);
            case TYPECODE_RECTANGLE:
                return new ShapeRectangle(statx, starty, endx, endy);
            case TYPECODE_OVAL:
                return new ShapeOval(startx, starty, endx, endy);
            default:
                throw new IllegalArgumentException("typecode = " + typecode);
        }
    }

    protected Shape(int startx, int starty, int endx, int endy) {
        _startx = startx;
        _endx = endx;
        _starty = starty;
        _endy = endy;
    }

    public abstract int getTypecode();
    public abstract String getName();
    public abstract void draw();
}
```

```java
public class ShapeLine extends Shape {
    protected ShapeLine(int startx, int starty, int endx, int endy) {
        super(startx, starty, endx, endy);
    }

    @Override public int getTypecode() { return Shape.TYPECODE_LINE; }
    @Override public String getName() { return "LINE"; }
    @Override public void draw() { drawLine(); }
    
    private void drawLine() {
        System.out.println("drawLine");
    }
}

public class ShapeRectangle extends Shape {
    protected ShapeRectangle(int startx, int starty, int endx, int endy) {
        super(startx, starty, endx, endy);
    }

    @Override public int getTypecode() { return Shape.TYPECODE_RECTANGLE; }
    @Override public String getName() { return "RECTANGLE"; }
    @Override public void draw() { drawRectangle(); }
    
    private void drawRectangle() {
        System.out.println("drawRectangle");
    }
}

public class ShapeOval extends Shape { ... }
```

위와 같이 모양에 해당하는 각각의 클래스가 Shape 클래스를 상속하여 구현한 후에 이를 호출하면 되는 것이다. 이전 코드처럼 모양이 추가될 때마다 `draw()`와 `getName()`에 switch case를 추가할 필요가 없다.

하지만 `create`에서 switch를 사용하기 때문에 이것도 결국 내부 코드를 수정해야 하므로 OCP를 위반하고 있다. 

그러나 Strategy Pattern을 사용하면 어떤 클래스의 객체를 반환해야 하는지 알 필요 없이 `factory` 필드와 `setFactory` 메서드를 만들어서 `setFactory` 메서드를 통해 받은 객체로 `facotry` 필드를 단순히 갈아끼우면 된다. (그저 `factory.create(~~)` 메서드를 호출하면 된다.)

책에서는 아래와 같은 코드를 작성했다.

```java
public abstract class Shape {
    ...
    public static Shape createShape(ShapeFactory factory, int startx, int starty, int endx, int endy) {
        return factory.create(startx, starty, endx, endy);
    }
    ...
}
```

```java
public abstract class ShapeFactory {
    public abstract Shape create(int startx, int starty, int endx, int endy);

    public static class LineFactory extends ShapeFactory {
        private static final ShapeFactory factory = new LineFactory();
        
        private LineFactory() {
        }

        public static ShapeFactory getInstance() {
            return factory;
        }

        public Shape create(int startx, int starty, int endx, int endy) {
            return new ShapeLine(startx, starty, endx, endy);
        }
    }

    public static class RectangleFactory extends ShapeFactory { ... }
    public static class OvalFactory extends ShapeFactory { ... }
}
```

```java
Shape line = Shape.createShape(ShapeFactory.LineFactory.getInstance(), 0, 0, 100, 200);
Shape rectangle = Shape.createShape(ShapeFactory.RectangleFactory.getInstance(), 0, 0, 100, 200);
```

그러나 위와 같은 코드는 가독성이나 사용하는 입장에서 봤을 때 괜찮은 코드는 아니다. (너무 길다.)

`ShapeFactory`를 구현하는 클래스들을 각각의 파일로 만들어서 하나의 패키지에 넣는 것도 괜찮은 방법이겠다. (`Shape` 패키지 안에 `ShapeFactory`, `LineFactory`, `RectangleFactory` 등이 있는 형태)

```java
Shape line = Shape.createShape(LineFactory.getInstance(), 0, 0, 100, 200);
Shape rectangle = Shape.createShape(RectangleFactory.getInstance(), 0, 0, 100, 200);
```

`create`라는 메서드 하나로 해결하지 않고, `createShapeLine`, `createShapeRectangle` 처럼 팩토리 메서드를 여러개 만들어서 사용하는 방법도 있다.

```java
public Shape createShapeLine(int startx, int starty, int endx, int endy) {
    return new ShapeLine(startx, starty, endx, endy);
}

public Shape createShapeRectangle(int startx, int starty, int endx, int endy) {
    return new ShapeRectangle(startx, starty, endx, endy);
}

public Shape createShapeOval(int startx, int start,y int endx, int endy) { ... }
```

지금까지 다양한 방법으로 리팩토링을 할 수 있었다. 이는 규모나 기능 추가 예정 등 trade off를 판단하여 적절한 리팩토링을 진행해야 한다. (절대적인 정답은 없다.)
