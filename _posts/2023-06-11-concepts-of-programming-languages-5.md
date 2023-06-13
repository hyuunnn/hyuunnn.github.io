---
layout: post
title: "프로그래밍 언어론 - Chapter 05"
description: "Concepts of Programming Languages - Robert W. Sebesta"
date: 2023-06-11
tags: [PL]
---

## 이름 (Names)

**설계 고려 사항**

* 이름에 대문자와 소문자가 구별되는가?
    * C언어를 포함하여 대부분의 언어는 대소문자를 구별한다.
    * Basic, Fortran, Pascal 언어는 구별하지 않는다.
    * 이는 누구한테는 판독성의 심각한 손상이라고 볼 수 있다.
        * 유사하게 보이는 이름이 실제로는 다른 개체를 가리키기 때문이다. (ex: Java의 parseInt -> 특정 대소문자의 사용을 기억해야 한다.)

* 이름의 최대 길이 제한이 있는가?
    * FORTRAN 77은 6글자, Fortran 95+는 31글자, COBOL은 30글자까지 허용한다.
    * C99는 내부 이름에서 제한이 없으나 처음 63글자까지만 의미가 있다.
    * 모던한 언어들은 제한하지 않는다. (Ada, Java, C++ 등)
        * 이론적으로 무한이어야 하나 사실상 그렇진 않다.

* 언어의 특수어가 예약어인가 또는 키워드인가?
    * 대부분의 언어에서 특수어는 예약어로 정의되어서 재정의할 수 없다.
    * 하지만 Fortran은 특수어가 키워드로 사용할 수 있다.
        * `INTEGER REAL`에서 REAL이 이름, `REAL INTEGER`에서 INTEGER가 이름이 될 수 있으므로 모호하다.
        * Fortran에서 아래 코드는 valid하다. (주석은 C언어로 예시를 들었다.)
            ```fortran
            INTEGER REAL  !! int double;
            REAL INTEGER  !! double int;
            REAL = 3      !! double = 3;
            INTEGER = 3.4 !! int 3.4;
            ```

    * COBOL은 300개의 예약어를 가지고 있다. (ex: LENGTH, BOTTOM, DESTINATION, COUNT 등)
        * 자주 사용되는 LENGTH, COUNT도 이름으로 사용할 수 없는 것이다.

* 첫 글자에서 대문자가 중요한가?
    * Prolog에서 대문자는 변수로 사용하며, 소문자는 상수로 사용한다.

* connector characters나 특수 기호를 허용하는가?
    * Perl - 변수 이름 앞에 특수 기호($, @, % 등)를 사용
    * Scheme - S-expression
    * Ruby - @는 변수, @@는 클래스 변수임을 나타냄
    * Python - <a href="https://mingrammer.com/underscore-in-python/">underscore(_)</a>

## 바인딩 (Binding)

이름과 속성을 연관지은 것이다. 바인딩은 언어 정의 시점, 언어 구현 시점, 컴파일 시점, 로드(적재) 시점, 링크 시점, 실행 시점 등에서 일어날 수 있다.

이는 언어 구성요소(예약어, 일반어 등)마다, 또는 같은 구성요소라도 속성(동적, 정적 언어)에 따라 바인딩 시간이 다를 수 있다.

바인딩이 어려운 이유는 값을 정확히 지칭하기 위한 방법에서부터 복잡해진다.

```c
static int X; int Y; 
scanf("%d", &x); 
X = X + 10;
```

* 변수 X의 타입은 **프로그램 컴파일 시점**에 바인딩된다.
* 변수 X의 값은 **프로그램 실행 시점**에 바인딩된다.
* 기본 타입에 관한 `+`의 의미는 **언어 정의 시점**에 바인딩된다.
* `+`의 의미 (C++에서는 operator로 재정의가 가능함)는 **컴파일 시점**에 바인딩된다.
* 숫자 10의 의미는 **언어 정의 시점**에 바인딩된다.
* 숫자 10의 내부 표현 형태는 **언어 구현 시점**에 바인딩된다.
* scanf 호출 시 수행될 내용은 **Program link edit time**
* 변수 X의 주소는 **프로그램 로드 시점**에 바인딩된다.
* 변수 Y의 주소는 **프로그램 실행 시점**에 바인딩된다.

### 정적 타입 바인딩 (Static Type Binding)

**명시적 선언(explicit declaration)**

변수 이름과 특정 타입이라고 명시하는 방법이다. (대부분의 언어들이 채택하고 있다.)

1960년대 이후 설계된 널리 사용되는 대부분의 언어들은 명시적 선언을 요구한다. (JavaScript, Perl, Ruby 등은 예외)

```c
int a, b, c;
```

**묵시적 선언(implicit declaration)**

컴파일러, 인터프리터에 의해서 네이밍 형식에 기반하여 타입을 바인딩한다.

예를 들어 Fortran에서 식별자가 I, J, K, L, M, N의 대문자나 소문자로 시작하면 Integer, 아니면 Real 타입으로 선언된다.

이런 방법은 약간의 편리성을 제공하지만 철자와 같은 오류 탐지에 방해를 주기 때문에 신뢰성에 유해할 수 있다.

Perl에서는 `@`으로 시작하는 이름은 배열, `%`으로 시작하는 이름은 해시 구조체로 인식한다. 

묵시적 선언의 다른 유형은 문맥을 파악하고 타입을 추론(type inference)하여 정적으로 타입을 부여하는 방법이다. (70년대부터 있었는데 최근 2011년경에 유명해졌다. - 메이저 언어들에서 이 기능을 도입하고 있다.)

* C#
    ```csharp
    var sum = 0; // int
    var total = 0.0; // float 
    var name = "Fred"; // string
    ```
* Basic
    ```basic
    X = 10
    A$ = "WIN"
    ```
* Perl
    ```perl
    # 빈 배열 생성
    my @array;

    # 배열에 요소 추가
    push @array, "apple";
    push @array, "banana";
    push @array, "orange";

    # 빈 해시 생성
    my %hash;

    # 해시에 키-값 쌍 추가
    $hash{"name"} = "John";
    $hash{"age"} = 30;
    $hash{"city"} = "New York";
    ```
* Java
    ```java
    // ArrayList의 element 타입이 Integer 타입으로 추론된다.
    List<Integer> list2 = new ArrayList<>();
    ```
    * <a href="https://docs.oracle.com/javase/tutorial/java/generics/erasure.html">Type Erasure</a>에 의해 컴파일 타임에 타입이 추론된다고 한다. (<a href="https://stackoverflow.com/questions/7213355/java-arraylist-generics-and-late-binding">stackoverflow</a>)
* C++
    ```cpp
    string s = "HELLO";
    for (auto c: s)
        cout << c << endl;
    ```

### 동적 타입 바인딩 (Dynamic Type Binding)

타입을 선언문으로 명세하지 않고, 값이 할당될 때 타입이 바인딩된다. (Late Binding)

즉 변수의 타입은 일시적일 수 있음을 인지해야 한다. (언제든지 타입의 변경 가능성을 제공한다.) - type checking을 하긴 하는데 정적이 아닌 동적으로 하기 때문에 프로그램이 동작하다가 오류가 발생하면 터지게 되는 것임

Python, Ruby, JavaScript, PHP, <a href="https://xpqz.github.io/learnapl/intro.html">APL</a>(<a href="https://en.wikipedia.org/wiki/APL_syntax_and_symbols">APL syntax and symbols</a>), SNOBOL4, Scheme 등은 동적 타입 바인딩이다.

```python
a = [10.2, 3.5] # list type
a = 47 # int type
a = 3 * x # 정수 * x를 계산할 때 x도 정수, 결과도 정수겠다고 타입 추론

def f(a, b): return a + b
b = f(1, 2) # 가능
b = f("qwe", "qweqwe") # 가능
```

```apl
LIST <- 2 4 6 8 (LIST는 integer의 배열 타입이다.)
LIST <- 10.2 (LIST는 부동소수점 타입이다.)
```

유연한 프로그래밍이 가능하다는 장점이 있으나 단점도 존재한다.

* 프로그램이 덜 신뢰적이게 한다.
    * 잘못된 타입에 대한 오류를 탐지하지 못한다. (새로운 변수 선언으로 인식하게 되는 것)
        ```python
        i = x
        i = y # 이때 y가 다른 타입이라도 오류가 발생하지 않는다.
        ```
* 타입 검사가 실행 시간에 수행되기 때문에 비용이 크다.
    * 해당 이유로 인해 오류 검사가 늦어질 수 밖에 없다.

위와 같은 이유들로 인해 동적 타입 바인딩은 **인터프리터**로 구현된다.

정적 타입 바인딩 언어는 **순수 인터프리터**로 구현되지 않는데, 기계 코드로 쉽게 번역할 수 있기 때문이다. (Early Binding)

Python에서 최근에 지원하는 <a href="https://docs.python.org/ko/3.10/library/typing.html">Type Hinting</a>은 Early Binding을 흉내라도 내보자고 하여 기능이 추가되었다고 함 

## 기억 공간 바인딩 (Storage Binding)

변수에 바인딩되는 memory cell은 memory pool로부터 가져와야 하는데 이때 memory pool에 할당(allocation)하고, 회수(deallocation)하여 다시 반환하는 과정을 의미한다.

이때 변수의 존속기간(lifetime)은 4가지 유형으로 구분할 수 있다.

Storage Binding은 garbage, dragling reference 등의 이슈가 존재한다.

Java는 위 이슈를 제거하기 위해 goto, delete를 제거했다.

Rust는 소유권이라는 개념을 추가하여 dragling reference를 차단한다.

### 정적 변수 (Static Variable)

프로그램 실행이 시작되기 전에 메모리에 바안딩되며, 프로그램이 종료될 때까지 동일한 메모리에 존재한다.

* 언어별 특징
    * FORTRAN I, II, IV는 모든 변수가 static이다.
    * C, C++, Java는 static 키워드를 제공한다.

장점은 효율성이다. 변수를 할당하고 회수하는 것에 대한 부담이 초래되지 않는다. (history sensitive하다. - 동일한 공간에 바인딩되어 프로그램 종료 시까지 유지되기 때문)

단점은 유연성 감소이다. 정적 변수만을 갖는 언어는 재귀를 지원할 수 없다.

### 스택-동적 변수 (static-dynamic variable)

책에 있는데 학교에서는 안배움

### 명시적 힙-동적 변수 (explicit heap-dynamic variable)

책에 있는데 학교에서는 안배움

### 묵시적 힙-동적 변수 (implicit heap-dynamic variable)

책에 있는데 학교에서는 안배움

## 영역 (Scope)

블록 외부에 선언된 변수들에 대한 참조가 어떻게 선언과 연관되는지 등

### 정적 영역 (static scoping)

쉽게 말해 소스코드를 보고 이미 다 알고 있는 상태를 의미하며, lexical scope라 부르기도 한다.

```javascript
function big() {
    function sub1() {
        var x = 7;
        sub2();
    }

    function sub2() {
        var y = x;
    }
    
    var x = 3;
    sub1();
}
```

sub2의 변수 x에 대한 참조는 프로시저 big에 선언된 x이다.

sub1은 sub2의 정적 조상에 속하지 않기 때문에 sub1의 x는 은폐된다. (sub1의 x은 지역변수의 역할을 수행하며 외부에 선언된 x는 sub1으로부터 은폐된다.) - Scope hole

쉽게 말해 선언된 x들 중에서 가장 가까운 x를 선택한다.

* 장점
    * 정적 타입 체킹으로 인한 신뢰성
    * 타입을 기반으로 효율적인 코드 생성 (빠름)
    * 읽기 쉽다.
* 단점
    * 필요 이상의 프로시저가 필요하다.
    * 필요 이상의 변수가 보인다.
    * 참조할 때 필요 이상의 프로그램 구조가 변경될 수 있다. -- ?

### 전역 영역

책에 있는데 학교에서는 안배움

### 동적 영역 (dynamic scoping)

정적 영역은 비지역 변수에 접근하는 많은 상황에서 잘 동작하게 한다.

하지만 책에서는 변수와 부프로그램에 대한 접근을 필요 이상으로 많이 허용한다고 한다.

두 번째로 소프트웨어는 동적이기 때문에 프로그램이 지속적으로 변화한다.

이러한 변화는 프로그램 재구조화를 초래하고, 변수와 부프로그램 접근을 제한하는 처음의 구조를 파괴한다고 한다.

동적 영역은 **공간 배치 관계**가 아닌 부프로그램들의 **호출 시퀀스**에 기반한다.

이전 javascript 코드의 예제를 동적 영역으로 해석해보면 big이 sub1을 호출하고, sub1이 sub2를 호출하므로 sub2부터 탐색을 시작한다. 

sub2는 그 다음으로 sub1을 탐색하는데 이때 x에 대한 선언이 발견되어 sub2의 참조는 sub1에 선언된 x를 의미한다.

* 장점
    * 일부 매개변수 전달을 생략할 수 있다.
* 단점
    * caller의 local variable이 callee에게 항상 보인다.
    * 정적 타입 체킹을 할 수 없다.
    * 프로그램의 판독을 어렵게 한다. (가독성이 별로다.)
    * 효율적으로 구현하기 어렵다.
    * 동적 영역은 어떤 것을 먼저 호출하냐에 따라서 다르게 동작하기 때문에 덜 신뢰적인 프로그램을 초래한다.

정적 영역으로 작성된 프로그램이 더 판독하기 쉽고, 신뢰적이고 빠르게 실행된다. - 동적 영역을 쓸 이유가 크게 없다.

## 존속 기간 (lifetime)

ㅁㄴㅇㅁㄴㅇ

## 참조 환경 (Referencing Environment)

ㅁㄴㅇㅁㄴㅇ
