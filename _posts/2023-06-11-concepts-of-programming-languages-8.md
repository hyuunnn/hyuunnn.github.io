---
layout: post
title: "프로그래밍 언어론 - Chapter 08"
description: "Concepts of Programming Languages - Robert W. Sebesta"
date: 2023-06-11
tags: [PL]
---

### 단락회로 계산

* Pascal: 단락회로 계산 지원 X
* C, C++, Java, Modula-2: 논리곱, 논리합에 대해 단란회로 계산 지원
* Ada: 프로그래머가 선택

장점 복잡한 계산을 간결하게 표현 가능 (이전 조건이 false면 다음 조건을 계산할 필요가 없다.)

단점: 부대효과가 있는 연산항에 대해 결과 예측이 어렵다. `(a > b) || (b++ / 3)`

### 선택문 중첩

**짝 잃은 else 문제 (dangling else problem)**

```lua
if sum = 0 then
    if count = 0 then
        result := 0
else
    result := 1
```

* 근거리 우선: 가장 가까운 앞쪽 if와 짝 (Pascal, C, C++, Java 등)
* 직접 중첩 금지: if문이 중첩되려면 복합문 사용 (Algol 60)
* 종결어 사용: if의 종결어 (end if)를 사용 (Fortran 90, Ada, Modula-2, Lua 등)

### 조건부 명령어 (guarded commands)

**선택 구문**

```
if <조건> -> <문장>
[] <조건> -> <문장>
...
[] <조건> -> <문장>
[] <조건> -> <문장>
fi
```

```
if x >= y -> max := x
[] y >= x -> max := y
fi
```

1. 모든 조건식을 계산
2. 참인 것 중 하나를 무작위로 실행
3. 참인 것이 없으면 수행 오류

**반복 구문**

```
do <조건> -> <문장>
[] <조건> -> <문장>
...
[] <조건> -> <문장>
[] <조건> -> <문장>
od
```

```
do q1 > q2 -> temp := q1; q1 := q2; q2 := temp;
[] q2 > q3 -> temp := q2; q2 := q3; q3 := temp;
[] q3 > q4 -> temp := q3; q3 := q4; q4 := temp;
od
```

1. 모든 조건식을 계산
2. 참인 것 중 하나를 무작위로 수행한 후 1부터 반복
3. 참인 것이 없으면 종료