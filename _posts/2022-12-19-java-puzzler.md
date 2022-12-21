---
layout: post
title: "자바 퍼즐러"
description: ""
date: 2022-12-19
tags: ["book"]
---

개발자가 실수 할 수 있는 다양한 유형들을 퍼즐로 비유하여 설명하는 책이다. <a href="http://www.yes24.com/Product/Goods/65551284">이펙티브 자바</a>의 저자가 쓴 책이기도 해서 읽어보고 싶다는 생각이 들었다. 책의 내용이 전체적으로 흥미있었고, <a href="http://www.yes24.com/Product/Goods/17926925">유지보수하기 어렵게 코딩하는 방법</a> 느낌의 내용도 있었다.

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
// 
```