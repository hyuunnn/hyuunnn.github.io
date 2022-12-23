---
layout: post
title: "자바 퍼즐러"
description: ""
date: 2022-12-19
tags: ["book"]
---

개발자가 실수 할 수 있는 다양한 유형들을 퍼즐로 비유하여 설명하는 책이다. <a href="http://www.yes24.com/Product/Goods/65551284">이펙티브 자바</a>의 저자가 쓴 책이기도 해서 읽어보고 싶다는 생각이 들었다. 책의 내용이 전체적으로 흥미있었고, <a href="http://www.yes24.com/Product/Goods/17926925">유지보수하기 어렵게 코딩하는 방법</a> 느낌의 내용도 있었다. 그리고 생각보다 어려웠다..

자바를 계속 써보면서 문제가 생길만한 상황을 아직 접해보지 않았는데, 책을 통해 다양한 퍼즐들을 경험하면서 신중해야겠다는 생각이 들었다. 책을 읽으면서 인상 깊었던 퍼즐들을 정리하였다.

8번째 퍼즐: 3항 연산자에서 2번째 3번째 값의 자료형이 다르면 형변환이 일어나는 경우가 있다.

9, 10번째 퍼즐: `=`연산자와 `+=`연산자는 다르다. 
```java
short x = 0;
int i = 123456;
```
위 코드에서 `x = x + i`는 단순 할당이므로 범위를 초과하여 에러가 발생하지만, `x += i`은 복합 할당이므로 타입 변환이 된 후에 오버플로우가 발생한다.

타입을 동일하게 두지 않으면 위와 같은 문제가 발생할 수 있다.

```java
Object x = "Buy";
String i = "Effective Java!";
```
위 코드에서 `x = x + i`는 `x + i`로 합쳐진 문자가 x에 저장되므로 문제가 없지만, `x += i`는 왼쪽 타입이 String이 아닌 참조형(Object)이므로 오류가 발생한다.

11번째 퍼즐: `+` 연산자는 문자열이 있을 때만 문자열 연결을 진행하며, 없으면 숫자 덧셈 연산을 진행한다.

13번째 퍼즐: 연산자 우선순위의 중요성, 우선 순위를 명확하게 표현하기 위해 괄호를 사용하자.
```java
System.out.println("Animals are equal: " + pig == dog);
// 위 코드는 아래와 같은 결과를 만든다. + 연산자가 == 연산자보다 우선순위가 높기 때문
System.out.println(("Animals are equal: " + pig) == dog);
```
```java
// `==`와 `equals()`의 차이점
String a = "animal";
String b = "animal";
System.out.println(a == b);
// 문자열을 인턴하기 때문에 true가 나온다. 하지만 비교 대상이 인턴되는 문자열인지 아닌지 알 수 없다.
// 객체의 참조를 비교할 떄는 ==, 내용을 비교할 때는 equals()
```

26, 28, 29번째 퍼즐: 반복문에서 발생할 수 있는 문제들
```java
// 26번째 퍼즐
// i++을 하는 과정에서 i가 MAX_VALUE가 되어 무한 루프가 발생한다.
// 최댓값 근처에 가는 값이 만들어진다면, 주의 깊게 확인하거나 long 타입 사용하기
int count = 0;
final int END = Integer.MAX_VALUE;
final int START = END - 100;
for (int i = START; i <= END; i++)
    count++;
System.out.println(count);
```
```java
// 28번째 퍼즐
// while의 조건은 말이 안되지만, 무한을 사용하면 1을 더해도 무한이기 때문에 true를 반환한다.
// 매우 큰 부동소수점 숫자를 사용하면 1을 더해도 영향을 주지 못해 true를 반환한다.

double i = 1.0 / 0.0;
double i = Double.POSITIVE_INFINITY;
double i = 1.0e40;

while (i == i + 1) {}
```
```java
// 29번째 퍼즐
// NaN(Not a Number)을 생성하면 자신을 비교했을 때도 false를 반환하여 while문을 실행할 수 있다.
double i = 0.0 / 0.0;
double i = Double.NaN;

while (i != i) {}

System.out.println(i == 0); // false
System.out.println(Double.NaN == Double.NaN); // false
```

36, 41번째 퍼즐: try, catch에서 return을 하더라도, finally에 return이 있다면 이를 수행한다.

```java
// 36번째 퍼즐
static boolean a() {
    try {
        return true;
    } finally {
        return false;
    }
}

System.out.println(a()); // false
```
```java
// 41번째 퍼즐
// finally의 특징에 적합한 상황이지만 아래와 같은 문제가 발생할 수 있다.
// in.close();에서 예외가 발생했을 때 out.close();를 실행하지 못하고 코드가 종료된다.
// Closeable을 받아와서 처리하는 메서드를 생성하여 이를 해결할 수 있다.
InputStream in = null;
OutputStream out = null;
try {
    in = new FileInputStream(src);
    out = new FileOutputStream(dest);
} finally {
    if (in != null) in.close(); // closeIgnoringException(in);
    if (out != null) out.close(); // closeIgnoringException(out);
}

private static void closeIgnoringException(Closeable c) {
    if (c != null) {
        try {
            c.close();
        } catch (IOException ex) {}
    }
}
```
46번째 퍼즐: 오버로딩을 했을 때 생길 수 있는 문제점
```java
// Object가 출력될 것 같지만, double array가 출력된다.
// null을 생각해보면 Object이지만, doublle[]에서도 사용 가능하다.
// Object보다 double이 작은 형태의 자료형이므로 double array가 출력되는 것이다.
// 이처럼 코드가 복잡해지므로, 오버로딩은 가능하면 하지 말기 (다른 이름 사용하기)
public class a {
    private a(Object o) {
        System.out.println("Object");
    }

    private a(double[] arr) {
        System.out.println("double array");
    }

    public static void main(String[] args) {
        new a(null);
    }
}
```
48번째 퍼즐

51번째 퍼즐: 오버라이딩 가능한 메서드 생성자에서 호출하지 말기 (호출 순서가 꼬이게 된다.)
```java
// [4,2]:null이 나오는데, ColorPoint의 생성자에서 super에 의해 부모 클래스의 생성자가 실행된다.
// Point의 makeName(), ColorPoint의 makeName()이 실행되는데, this.color = color를 하기 전에 실행되어서 null이 나온다.
class Point {
    protected final int x, y;
    private final String name;

    Point(int x, int y) {
        this.x = x;
        this.y = y;
        name = makeName();
    }

    private String makeName() {
        return "[" + x + "," + y + "]";
    }

    public final String toString() {
        return name;
    }
}

public class ColorPoint extends Point {
    private final String color;
    
    ColorPoint(int x, int y, String color) {
        super(x, y);
        this.color = color;
    }

    protected String makeName() {
        return super.makeName() + ":" + color;
    }

    public static void main(String[] args) {
        System.out.println(new ColorPoint(4, 2, "purple"));
    }
}
```
52번째 퍼즐: static을 사용할 때 주의할 점
```java
// 4950이 나와야하는데 9900이 나온다.
// 그 이유는 static으로 초기화할 때 순서대로 실행되기 때문에 static 블록이 먼저 실행되는데, 그 이후에 initialized 값이 false로 다시 변경되므로 이후에 getSum() 메서드에 의해 다시 실행된다.
// static 블록을 initialized 초기화되는 부분 이후로 옮기면 해결된다.
// 또한 이 코드는 무분별하게 메서드를 남용하고 있는데, sum 변수에 이를 계산해주는 헬퍼 메서드 하나면 충분하다.
class Cache {
  static {
    initializeIfNecessary();
  }

  private static int sum;
  public static int getSum() {
    initializeIfNecessary();
    return sum;
  }

  private static boolean initialized = false;
  private static synchronized void initializeIfNecessary() {
    if (!initialized) {
      for (int i = 0; i < 100; i++)
        sum += i;
      initialized = true;
    }
  }
}

public class Main {

  public static void main(String[] args) {
    System.out.println(Cache.getSum()); // 9900
  }
}
```
53번째 퍼즐: 

57번째 퍼즐: equals() 메서드를 오버라이딩할 때 hashCode()도 오버라이딩하자. <a href="https://tecoble.techcourse.co.kr/post/2020-07-29-equals-and-hashCode/">equals와 hashCode는 왜 같이 재정의해야 할까?</a>

60번째 퍼즐: 배열을 문자열로 변환해주는 메서드 Arrays.deepToString()
```java
Object[] array = {1, 2, 3, new int[] {1, 2, 3}, new int[] {1, 2, 3}};
System.out.println(Arrays.deepToString(array)); // [1, 2, 3, [1, 2, 3], [1, 2, 3]]

Object[] array = {1, 2, 3, new int[] {1, 2, 3}, new int[] {1, 2, 3}};
arrays[0] = array;
System.out.println(Arrays.deepToString(array)); // [[...], 2, 3, [1, 2, 3], [1, 2, 3]] 
```