---
layout: post
title: "웹 해킹 & 보안 완벽 가이드 - Chapter 02"
description: ""
date: 2023-07-08
tags: [book]
---

<a href="https://www.yes24.com/Product/Goods/14275829">웹 해킹 & 보안 완벽 가이드</a>

## 사용자 접근 처리

### 인증

대부분의 웹사이트는 아이디와 비밀번호를 입력하여 사용자를 인증한다. 하지만 인증하는 코드에 결함이 존재하여 악의적인 입력이 가능하다면 인증 과정을 우회하거나 권한을 얻을 수 있다. (SQL Injection 등)

### 세션 관리

다음으로 인가된 사용자의 세션을 효율적으로 관리하는 것이다.

일반적으로 서버에서 토큰을 발행하여 유효한 세션인지 확인한다. (히든 필드나 URL 쿼리에 사용하는 경우도 있지만 표준 방법은 쿠키에 넣는다.)

하지만 토큰에 의존하여 세션을 확인하기 때문에 토큰 생성 알고리즘에 결함이 있거나 추측할 수 있다면 다른 사용자로 위장할 수 있게 된다.

### 접근 통제

사용자의 권한을 확인하여 허용 범위에 맞는 페이지에만 접근할 수 있어야 한다. (HTTP 상태 코드)

## 사용자 입력 값 처리

1장에서 말한 것처럼 입력 값은 신뢰할 수 없는 데이터라고 가정해야 한다. 

예를 들어 회원가입에서 특수문자, 공백 등을 허용하지 않는 등 규칙이 있어야 한다. (이 외에도 최대 길이, HTML 태그 허용하지 않기 등)

### 블랙리스트 방법

공격에 사용되는 문자들을 필터링하는 방법이다. (입력 값을 제어하는 방법 중에서 가장 비효율적이다.)

하지만 필터링이 제대로 되지 않았거나 문자를 변형, 인코딩하면 우회할 수 있기 때문에 취약하다. (새로운 공격 페이로드에 대한 필터를 추가해야 한다.)

* `SELECT`가 막힌 경우 `SeLeCt`
* `or 1=1--`가 막힌 경우 `or 2=2--`
* `alert('xss')`가 막힌 경우 `prompt('xss')`
* `%00<script>alert(1)</script>` -  `null` 문자를 이용하여 필터링 우회
* `<script>`가 막힌 경우 `<scr<script>ipt>`
* `'`가 막힌 경우 URL 인코딩하여 `%27`, `%2527` 등으로 우회

### 화이트리스트 방법

화이트리스트 기준에 일치하는 문자들만 허용하고 나머지는 거부하는 방법이다. (입력 값을 제어하는 방법 중에서 가장 효과적이
다.)

하지만 일부 사람의 이름에서 `'`과 같은 문자들을 허용해야 하는 경우도 있기 때문에 완벽한 해결책은 아니다.

### 에러 핸들링

에러 메세지는 공격자에게 민감한 정보를 제공할 수 있기 때문에 최소화해야 한다. 

<a href="https://infosecwriteups.com/exploiting-error-based-sql-injections-bypassing-restrictions-ed099623cd94">Exploiting Error Based SQL Injections & Bypassing Restrictions</a>

<a href="https://github.com/pallets/flask">flask</a>에 존재하는 디버그 모드를 통해 원하는 명령어를 실행할 수도 있다. (<a href="https://www.daehee.com/werkzeug-console-pin-exploit/">Werkzeug Console PIN Exploit</a>, <a href="https://lactea.kr/entry/python-flask-debugger-pin-find-and-exploit">python flask debugger pin, find and exploit</a>)

### 어플리케이션 관리

* 인증 기능이 존재한다면 관리자 권한을 획득할 수 있는 가능성을 제공한다.
* 권한을 확실히 분리하여 관리자 기능에 대한 접근을 제한해야 한다.
* 또한 관리자 기능은 신뢰된 사용자가 사용한다고 가정하여 엄격하지 않은 검사를 수행하기도 한다.
