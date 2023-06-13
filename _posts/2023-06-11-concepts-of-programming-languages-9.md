---
layout: post
title: "프로그래밍 언어론 - Chapter 09"
description: "Concepts of Programming Languages - Robert W. Sebesta"
date: 2023-06-11
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

ㅁㄴㅇㅁㄴㅇ

### pass by result (call by result)

ㅁㄴㅇㅁㄴㅇ

### pass by value-result (call by value-result)

ㅁㄴㅇㄴㅁㅇ

### pass by reference (call by reference)

ㅁㄴㅇㄴㅁㅇ

### pass by name (call by name)

<a href="https://for-development.tistory.com/142">[Scala] Call-by-value 와 Call-by-name</a>

<a href="https://bambielli.com/til/2016-07-24-CBV-vs-CBN/">Call By Value vs Call By Name</a>

<a href="https://stackoverflow.com/questions/2962987/what-is-call-by-name">What is "Call By Name"?</a>
