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

* 스트링이 단순히 문자 배열의 특수한 유형인가 또는 기본 타입인가?
* 스트링이 정적 길이 또는 동적 길이를 갖는가?

* 선언 (assignment)
* 비교 (comparison)
* 문자 연결
    * `NAME1 := NAME1 & NAME2;` - Ada
* 슬라이스(slice) - 부분 스트링 참조(substring reference)
    * `NAME(2:4);` - Ada
* 패턴 매칭 (Pattern Matching)
    * `NAME ~= /\d+/` - Perl

## 사용자 정의 타입 (User-Defined Ordinal Type)

### 열거 타입 (Enumeration type)

### 부분 범위 타입 (subrange type)

## 배열 타입 (Array Type)

## Conformant Array

## 연상 배열 (associative array)

## 레코드 타입 (record type)

## 공용체 타입 (union type)

## 포인터 타입 (pointer type)

dangling pointer, dangling reference - User After Free

### 비석 접근 방법 (tombstone)

### 잠금-키 접근 방법 (locks-and-keys approach)

## 참조 타입 (reference type)
