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
* Boolean
* Character

## 문자 스트링 타입 (Character String Type)

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
