---
layout: post
title: "Black Box Testing"
description: ""
date: 2023-06-20
tags: [testing]
---

테스트 대상의 소프트웨어가 어디에 결함이 있는지 알지 못한다.

입력 값들을 조합하여 내부의 모든 부분을 조사할 필요가 있다.

하지만 가능한 모든 경우를 테스트하는 것은 불가능에 가깝다. (<a href="https://www.codementor.io/@korneliaodoherty/what-to-know-about-exhaustive-testing-p4ei01m6l">Exhaustive Testing</a>)

즉 의미있는 입력 값을 누락하지 않고 만들어야 한다. (시스템의 결함을 노출시킬 수 있는 입력 값)

## Single Function

하나의 동작에 대한 테스트 케이스를 결정

### Equivalence Partitioning Testing (동등 분할) - 인자가 하나 존재

![1](/assets/images/black-box-testing/1.png)

![2](/assets/images/black-box-testing/2.png)

같은 결과를 반환하는 여러개의 다른 입력 값들 중에서 하나만 선택하고 분할하는 방법

```java
public int 입장방식(int age) {
    if (age <= 10)
        result = 입장불가;
    else if (age <= 17)
        result = 부모동반입장;
    else
        result = 단동입장가능;
    return result;
}
```

예를 들어 위와 같은 코드를 테스트할 때 age가 10보다 작으면 result가 0이 되는 것을 테스트할 수 있는데 1부터 10까지 모두 테스트할 필요는 없다는 것이다.

### Boundary-Value Analysis Testing (경계값 분석) - 인자가 하나 존재

```
Bugs lurk in corners and congregate at boundaries. - Boris Beizer
```

결함이라는건 보통 구석에서 자주 발생한다.

동등 분할에서 가운데나 임의의 값보다 경계값 부분에 결함이 자주 식별된다는 것이다.

예를 들어 `70 < S <= 100`이 정상적인 코드라고 가정했을 때 `70 < S <= 99`, `70 <= S <= 100`과 같이 경계값을 설정할 때 개발자의 실수가 자주 발생하곤 한다.

![0](/assets/images/black-box-testing/0.png)

![4](/assets/images/black-box-testing/4.png)

**2-value boundary**

예를 들어 `100 <= X < 300`을 테스트할 때 양쪽 경계에서 2개를 선택하는 방법을 말한다.

좌측의 조건을 충족시키는 가장 큰 값 100, 충족시키지 않는 가장 큰 값 99

우측의 조건을 충족시키는 가장 큰 값 299, 충족시키지 않는 가장 작은 값 300

**3-value boundary**

2-value boundary에서 경계 내부까지 추가로 테스트하는 방법

### Combinatorial Testing - 인자가 복수개 존재

포트(1024 ~ 65535)를 지정하는 -p 옵션과 접속 시도 횟수(0 ~ 65535)를 정하는 -r 옵션을 가진 FTP 클라이언트를 테스트한다고 가정하자.

![5](/assets/images/black-box-testing/5.png)

![6](/assets/images/black-box-testing/6.png)

valid, invalid한 케이스들을 조합하여 적절한 테스트 케이스를 만드는 방법

### Decision Table/Cause-Effect Graphing - 인자가 복수개 존재

-

### Pair-wise Testing - 인자가 복수개 존재

`void format(type, size, method, filesystem)`이라는 함수를 테스트한다고 가정하자.

type은 Primary, Logical, Single, Span

Size는 10, 100, 500, 1000

Method는 Quick, Slow

Filesystem은 FAT, FAT32, NTFS

4 * 4 * 2 * 3 = **96**가지의 조합이 가능하다.

하지만 이는 너무 많기 때문에 4개의 모든 조합이 아닌 임의의 2개 조합을 정하는 것이다.

![7](/assets/images/black-box-testing/7.png)

위 사진과 같이 17개의 조합으로 테스트 케이스를 줄일 수 있다.

#### PICT

Microsoft에서 <a href="https://github.com/microsoft/pict">pict</a>라는 도구를 제공하고 있다.

```text
이름: 고딕, 본고딕, 궁서, 바탕
크기: 10, 14, 18, 22, 28
스타일: 보통, 굵게, 기울임
효과: 위첨자, 아래첨자

# 글꼴의 이름이 "궁서" 일 때는 위 첨자는 테스트하지 않는다.
IF [이름] = "궁서" THEN [효과] <> "위첨자";

# 글꼴의 크기가 18이상일 때는 스타일 중에서
# 굴게와 기울임을 반드시 테스트한다.
IF [크기] > 18 THEN [스타일] IN {"굵게", "기울임"};
```

워드프로세서의 기능을 테스트한다고 가정했을 때 위와 같이 rule을 정의하여 아래와 같은 결과를 얻을 수 있다.

```console
C:\Users\hyuunnnn\Downloads>pict.exe font.txt
이름    크기    스타일  효과
궁서    28      굵게    아래첨자
바탕    22      굵게    위첨자
바탕    28      기울임  위첨자
궁서    10      기울임  아래첨자
본고딕  14      보통    위첨자
바탕    14      보통    아래첨자
본고딕  10      굵게    위첨자
바탕    10      보통    위첨자
고딕    10      보통    위첨자
본고딕  28      기울임  아래첨자
고딕    14      기울임  아래첨자
고딕    22      굵게    아래첨자
궁서    18      보통    아래첨자
고딕    28      굵게    위첨자
고딕    18      굵게    위첨자
바탕    18      기울임  아래첨자
본고딕  22      기울임  위첨자
궁서    14      굵게    아래첨자
궁서    22      굵게    아래첨자
본고딕  18      굵게    아래첨자
```

두 번째 예시

```
Destination = Paris, London, Sydney
Class = First, Business, Economy
Seat = Aisle, Window
```

![8](/assets/images/black-box-testing/8.png)

Destination의 3가지, Class의 3가지, Seat의 2가지 3 * 3 * 2 = **18**가지의 경우를 조사해야 한다. (모든 경우의 조합 - All combinations)

하지만 pairwise 방법을 사용하면 Destination-class 조합, destination-seat 조합 , class-seat 조합으로 만들어서 **9**가지로 경우가 줄어들었다.

![9](/assets/images/black-box-testing/9.png)

## Sequence of Functions

연속적인 동작에 대한 테스트 케이스를 결정

### State-based Testing

### Use Case Testing
