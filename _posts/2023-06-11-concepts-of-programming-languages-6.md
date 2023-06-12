---
layout: post
title: "프로그래밍 언어론 - Chapter 06"
description: "Concepts of Programming Languages - Robert W. Sebesta"
date: 2023-06-11
tags: [PL]
---

## 타입 (Type)

타입이 왜 중요할까? - 좋은 언어의 척도가 될 수 있기 때문이다.

예를 들어 Python은 list라는 기능이 built-in되어 사용할 수 있다. 하지만 구현되어 있지 않은 언어라면 직접 만들어야 한다.

## 기본 데이터 타입 (Primitive Data Type)

다른 타입으로 정의되지 않는 데이터 타입

대부분 하드웨어에서 지원하는 타입이지만 (정수 타입이 그렇다.), 항상 지원되는 것은 아니다 (이때 소프트웨어적인 지원)

### 정수 (Integer)

2's complement (2의 보수)가 majority이긴 하지만 standard는 아니기 때문에 모든 프로그래밍 언어가 2의 보수로 구현했다고 가정하면 안된다.

언어를 2의 보수로 구현하라는 내용은 없다. 하지만 2의 보수를 많이 채택하고 있는 것이다.

### 부동 소수점 (Floating Point)

floating point는 real number가 아닌 실수 값에 대한 근사 값이며, IEEE-754에 의해 표준으로 정의되어 있다.

일반적으로 하드웨어와 구현을 일치시키지만, 항상 그런 것은 아니다. (Ada 언어는 국방에서 사용될만큼 특정 영역만큼은 정밀하게 계산하라는 룰이 있기도 했다.)

### Others

* Decimal
    * fixed point라고 생각하면 된다. (ex: 99.999라고 할 때 숫자 각각을 인코딩하는 것이다. (문자 스트링과 매우 흡사하게 저장된다.) - 고정된 10진수 숫자(coded digit)들의 모임)
    * 장점은 제한된 범위 내에서 정확하다. (부동 소수점은 오차 존재)
    * 단점은 제한된 범위, 메모리 낭비가 있다. (ex: 1282라는 수 각각을 인코딩하면 2^4 = 16바이트 필요)
* Boolean
    * 불리언 값은 한 개의 비트로 표현될 수 있지만 대부분의 컴퓨터는 비트 단위를 효율적으로 접근할 수 없기 때문에 바이트 단위로 구현하는 경우가 많다.
* Character
    * ASCII: 8비트
    * UNICODE: 16비트

## 문자 스트링 타입 (Character String Type)

**스트링이 단순히 문자 배열의 특수한 유형인가 또는 기본 타입인가?**

* 대입 (assignment)
    * Python에서는 `a = "hihi"`, Java에서는 `String a = "hihi";` 등
* 비교 (comparison)
    * `==`, Java에서는 `equals` 등
* 문자 연결
    * `NAME1 := NAME1 & NAME2;` - Ada
* 슬라이스(slice) - 부분 스트링 참조(substring reference)
    * `NAME(2:4);` - Ada
* 패턴 매칭 (Pattern Matching)
    * `NAME ~= /\d+/` - Perl

슬라이스, 패턴 매칭 등 언어 자체에서 지원하는 경우도 있고 함수 혹은 클래스 라이브러리 형태로 지원하는 경우도 있다.

**스트링이 정적 길이 또는 동적 길이를 갖는가?**

* 정적 길이 스트링 (static length string)
    * Python의 스트링, Java의 String 클래스, Fortran, Ada, COBOL 등  (수정의 개념이 아닌 항상 새롭게 만든다.)
* 제한된 동적 길이 스트링 (limited dynamic length string)
    * 고정된 최대 길이까지의 가변적인 길이를 갖는다. (null의 유무로 문자의 끝을 판단한다.) - C, C++, PL/I 등
* 동적 길이 스트링 (dynamic length string)
    * 최대 길이의 제한 없이 가변 길이를 갖는다. - Javascript, Perl, 표준 C++ 라이브러리 등

### 문자 스트링 타입의 구현

위 설명처럼 정적 길이의 경우 Maximum Length가 필요 없이 단지 현재 Length 값만 필요하다.

하지만 제한된 동적 길이는 Maximum Length가 추가로 필요하다.

동적 길이는 단순히 현재 length만 필요할 수 있겠다. (단순한 run-time descriptor 요구)

하지만 스트링의 길이와 바인딩되는 memory 공간은 동적으로 늘어나거나 줄어들어야 하기 때문에 allocation과 deallocation 비용이 발생한다.

## 사용자 정의 타입 (User-Defined Ordinal Type)

### 열거 타입 (Enumeration type)

열거 타입은 판독성과 신뢰성의 장점을 제공할 수 있다. (int 타입을 사용하는게 아닌 별도의 타입이기 때문)

하지만 언어마다 동작 방식이 다르므로 모호해질 수 있다.

Ada, C#, Java 등의 언어는 enum 타입에서 어떠한 산술 연산도 지원하지 않는다.

C은 enum 변수를 정수 변수처럼 취급하고 있다.

또한 Pascal, C/C++은 enum에서 사용한 이름을 다른 곳에서 사용할 수 없으나 Ada에서는 가능하다.

```ada
type LETTERS is ('A','B','C','D','E','F','G','H','I',
'J','K','L','M','N','O','P','Q','R',
'S','T','U','V','W','X','Y','Z');

type VOWELS is ('A','E','I','O','U');
```

### 부분 범위 타입 (subrange type)

Pascal에서 도입되었고 Ada에서 포함된 기능이다.

* Pascal
    ```pascal
    type
        index = 1..100;
    ```
* Ada
    ```ada
    type DAYS is (Sun, Mon, Tue, Wed, Thu, Fri, Sat);
    subtype WEEKDAYS is DAYS range Mon..Fri;
    ```

특정 범위의 값들만 저장할 수 있다는 점에서 판독성을 향상시킨다.

## 배열 타입 (Array Type)

C, C++, Java, Ada, C#과 같은 대부분의 언어에서 배열의 모든 원소들은 동일한 타입일 것을 요구한다.

하지만 Javascript, Python, Ruby 같은 언어들은 타입이 없는 참조(typeless references)이다. - 다른 타입의 객체, 값들을 참조할 수 있다.

C#, Java는 generic 배열을 제공한다.

### 정적 배열 (Static Array)

실행 시간 전에 첨자 범위(subscript range)가 정적으로 바인딩된다.

장점: 실행 효율성 (할당이나 회수를 하지 않는다.)

단점: 메모리 공간에 고정되어 있다.

### 고정 스택 배열 (fixed stack-dynamic array)

첨자 범위는 정적으로 바인딩되지만 할당은 선언문 세련화 기간(delaration elaboration time)에 바인딩된다.

장점: 메모리 공간의 효율성

단점: 할당을 요구하며 회수 시간도 존재 -- ?

### 스택 동적 배열 (stack-dynamic array)

첨자 범위, 메모리 할당 모두 세련화 기간에 동적으로 바인딩된다. (선언 이후 변수의 lifetime 동안 고정)

장점: 유연성, 배열을 사용할 때까지 크기를 알필요가 없다.

### 고정 힙 동적 배열 (fixed heap-dynamic array)

책에 있는데 학교에서는 안배움

### 힙 동적 배열 (heap-dynamic array)

첨자 범위, 기억공간 할당이 동적

장점: 유연성, 변화할 때마다 늘어나거나 줄어들 수 있다.

단점: 할당과 회수의 시간이 더 길어지고, 프로그램 실행 중에 여러 번 일어날 수 있다.

TODO: 책의 예시들 정리

## Conformant Array

## 연상 배열 (associative array)

ㅁㄴㅇㄴㅁㅇ

## 레코드 타입 (record type)

ㅁㄴㅇㅁㄴㅇ

## 공용체 타입 (union type)

ㅁㄴㅇㅁㄴㅇ

## 포인터 타입 (pointer type)

dangling pointer, dangling reference - User After Free

### 비석 접근 방법 (tombstone)

ㅁㄴㅇㅁㄴㅇ

### 잠금-키 접근 방법 (locks-and-keys approach)

ㅁㄴㅇㅁㄴㅇ

## 참조 타입 (reference type)

ㅁㄴㅇㅁㄴㅇ
