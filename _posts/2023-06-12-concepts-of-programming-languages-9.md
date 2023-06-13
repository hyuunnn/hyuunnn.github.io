---
layout: post
title: "프로그래밍 언어론 - Chapter 09"
description: "Concepts of Programming Languages - Robert W. Sebesta"
date: 2023-06-12
tags: [PL]
---

## 부프로그램 (subprogram)

* 각 부프로그램은 **단일 진입점**을 갖는다. (입구가 하나다.)
* 서브프로그램을 호출한 서브프로그램(호출자)은 호출된 서브프로그램이 수행되기 전에 수행이 정지된다. (수행 중인 서브프로그램은 꼭 **하나**이다.)
* 호출된 서브프로그램의 수행이 완료되면 호출자로 돌아간다. (호출자로 수행 흐름이 바뀐다.)

### 매개변수 (parameter)

**형식인수, 형식 매개변수 (formal parameter)**

서브프로그램 헤더에 선언된 형식적 변수

**실인수, 실 매개변수 (actual parameter)**

서브프로그램 호출 시 전달되는 실제 값 또는 주소 (형식 매개변수에 바인딩된다.)

### 대응(correspondence)

**위치 (positional)**

매개변수 위치에 따라 전달, 일반적인 방법

**키워드 (keyword)**

매개변수 이름에 따라 전달, 순서가 중요하지 않지만 이름을 기억해야 한다.

Ada, FORTRAN90, Python 등은 두 방법을 병용한다.

## 프로시저와 함수 (Procedure and Function)

### 프로시저 (Procedure)

형식 매개변수, 비지역 변수를 외부로 전달하기 때문에 side effect를 발생시킨다.

### 함수 (Function)

수학 함수를 모델로 만들었기 때문에 매개변수, 함수 외부에서 정의한 변수를 수정하지 않는다.

그렇기 때문에 함수형 프로그래밍 컨셉이 side effect 없이 값만 유일하게 가지고 있는 형태이다.

언어에 따라서 함수와 프로시저의 구분은 프로그래머의 몫이다.

예를 들어 C언어에서 void 함수는 외부로 전달하는게 없기 때문에 프로시저이다.

## Parameter Passing

* in mode: 실 매개변수 -> 형식 매개변수
* out mode: 형식 매개변수 -> 실 매개변수
* in-out mode: 양방향 전달이 모두 일어남

### pass by value (call by value)

호출 시 복사 (in mode)

### pass by result (call by result)

반환 시 복사 (out mode)

### pass by reference (call by reference)

주소를 전달 (in-out)

### pass by value-result (call by value-result, call by copy)

reference 전달을 할 때 별칭(alias)로 인한 문제가 발생할 수 있다.

이를 해결하기 위해 전달, 반환할 때 복사하는 방법 (in-out)

### pass by name (call by name)

주소 계산 함수, 값 계산 함수를 통틀어서 thunk라고 하는데 parameter를 사용할 때마다 thunk가 판단하여 상황에 맞는 함수로 동작함  -> 함수를 호출할 때 대응된 식을 계산한다 - lazy binding

단순 변수(scalar variable)는 `reference` 전달, 상수 식(constant expression)은 `value` 전달 방식으로 동작한다.

algol 60은 pass by name을 사용하는 언어 중 하나인데, DrRacket에서 <a href="https://github.com/racket/algol60">algol 60</a>을 실험용으로 제공하고 있다.

```algol60
#lang algol60

begin
  comment
    algol60에서 value는 프로시저에 전달된 인자의 값을 변경할 수 없게 지정한다.
    value U, V, W, X 앞에 있는 comment를 놔두면 call by name, 지우면 call by value로 동작한다.

    * 동작 과정
    단순 변수 전달인 U, V, W, X는 pass by reference처럼 동작한다.
    상수 식인 A, B, C, D는 pass by value처럼 동작한다.

    V := U + A에서 V는 reference로 동작하여 B = A + A (2 + 2 = 4)
    W := A + B에서 W는 refernece로 동작하여 W = A + B (2 + 4 = 6)
    A := A + 1에서 A는 전역변수로 동작하여 A = A + 1 (2 + 1 = 3)
    X := A + 2에서 X는 reference로 동작하여 X = A + 2 (3 +2 = 5)

    결국 reference로 접근하여 값을 변경했기 때문에 P와 main에서의 출력 결과는 3 4 6 5로 같다.
  ;

  procedure P(U, V, W, X);
    comment value U, V, W, X;
    integer U, V, W, X;
  begin
    V := U + A;
    W := A + B;
    A := A + 1;
    X := A + 2;
    printn(U); prints(` ');
    printn(V); prints(` ');
    printn(W); prints(` ');
    printnln (X);
  end;

  integer A, B, C, D;
  A := 2; B := 5; C := 8; D := 9;

  P(A, B, C, D);
  printn(A); prints(` ');
  printn(B); prints(` ');
  printn(C); prints(` ');
  printn(D);
end
```

![0](/assets/images/concepts-of-programming-languages-9/0.png)

<a href="https://bambielli.com/til/2016-07-24-CBV-vs-CBN/">Call By Value vs Call By Name</a> - Example 3을 보면 y에 무한 루프를 수행하는 코드가 들어왔을 때 call by value라면 값을 계산하기 위해 함수가 종료되지 않지만, call by name은 y를 호출하지 않기 때문에 함수가 정상적으로 종료된다.

<a href="https://for-development.tistory.com/142">[Scala] Call-by-value 와 Call-by-name</a> - 위와 매우 비슷한 예제

<a href="https://stackoverflow.com/questions/2962987/what-is-call-by-name">What is "Call By Name"?</a>

<a href="https://stackoverflow.com/questions/838079/what-is-pass-by-name-and-how-does-it-work-exactly">What is "pass-by-name" and how does it work exactly?</a>
