---
layout: post
title: "Code Coverage"
description: ""
date: 2023-06-05
tags: [testing]
---

### Statement Coverage

테스트 케이스가 대상이 되는 각 statement를 1번 이상 실행하자는 의미

![0](/assets/images/code-coverage/0.png)

TC-1은 11개 문장 중에서 5문장, TC-2는 11개 문장 중에서 6문장의 코드를 테스트했으므로 100%를 충족하지 않으며 추가적인 테스트 코드 작성이 필요할 수 있겠다고 판단할 수 있다.

![1](/assets/images/code-coverage/1.png)

테스트 코드가 서로 실행이 안된 문장을 테스트해주기 때문에 100% coverage를 충족하고 있다.

### Branch (Decision) Coverage

true, false로 인한 경로를 decision이라 하는데 이러한 branch의 coverage를 계산하는 방법

Branch Coverage가 100%일 때 Statement Coverage도 충족한다고 볼 수 있다.

![2](/assets/images/code-coverage/2.png)

![3](/assets/images/code-coverage/3.png)

![4](/assets/images/code-coverage/4.png)

### Condition Coverage

개별적인 condition에 해당하는 true, false를 계산하여 coverage를 계산하는 방법

![5](/assets/images/code-coverage/5.png)

![6](/assets/images/code-coverage/6.png)

하지만 Condition Coverage를 충족한다고 항상 Branch Coverage가 충족되진 않는다.

![7](/assets/images/code-coverage/7.png)

Condition Coverage 입장에서는 어쨌든 개별적인 condition들을 실행했지만, Branch Coverage 입장에서는 D1에서 `A > 1`과 `B == 0`을 모두 충족하는 테스트 케이스가 없어서 false일 때만 테스트하기 때문이다.

### Decision/Condition Coverage

Condition Coverage와 Decision Coverage를 포함 관계로 보지 않고, 각 coverage가 100%를 충족하면 Decision/Condition Coverage도 충족한다는 전략

### Multiple Condition Coverage

Condition Coverage는 각각의 true, false를 조사했는데 이번에는 모든 조합을 조사하는 방법

![8](/assets/images/code-coverage/8.png)

![9](/assets/images/code-coverage/9.png)

모든 조합을 조사하기 때문에 Decision Coverage와 Condition Coverage도 충족시킨다.

![10](/assets/images/code-coverage/10.png)

### MC/DC

Multiple Condition Coverage는 condition 개수가 n개일 때 2^n개가 필요하여 너무 많아진다는 것이다. (너무 과도한 테스트 케이스가 만들어지게 된다.)

그래서 각 condition이 true, false로 변경되었을 때 decision에 영항에 주는 값들로만 테스트하자는 것이다. 최대 n+1, 최악 2*n 개의 테스트 케이스가 만들어진다.

![11](/assets/images/code-coverage/11.png)

B와 C가 T인 상태에서 A가 T에서 F로 변경했는데 Outcome이 변경되었으므로 1, 5 테스트 케이스를 포함하자.

A와 C가 T인 상태에서 B가 T에서 F로 변경했는데 Outcome이 변경되었으므로 1, 3 테스트 케이스를 포함하자.

A와 B가 T인 상태에서 C가 T에서 F로 변경했는데 Outcome이 변경되었으므로 1, 2 테스트 케이스를 포함하자.

모든 테스트 케이스를 사용하는 것이 Multiple Condition Coverage인데 이렇게 decision에 영향을 주는 케이스들만 사용하여 꼭 필요한 테스트 케이스들로 구성할 수 있다.

**A or (B and C)**

![12](/assets/images/code-coverage/12.png)

**A and (B or C)**

![13](/assets/images/code-coverage/13.png)

### Coverage 별 범위

![14](/assets/images/code-coverage/14.png)

### Coverage 도구 정리

<a href="https://github.com/jacoco/jacoco">JaCoCo</a>

<a href="https://github.com/JetBrains/intellij-coverage">intellij-coverage</a>

<a href="https://github.com/pmd/pmd">PMD</a>

<a href="https://github.com/checkstyle/checkstyle">checkstyle</a>

<a href="https://github.com/SonarSource/sonarqube">SonarQube</a>

<a href="https://github.com/gcovr/gcovr">gcovr</a>

<a href="https://github.com/linux-test-project/lcov">locv</a>
