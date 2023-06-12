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

```csharp
List<String> stringList = new List<String>();
stringList.Add("Michael");
```

```perl
my @colors = ("red", "green");
push @colors, "blue";
unshift @colors, "yellow";
print "@colors";  # Output: yellow red green blue
```

### 첨자 타입 (Subscript Type)

Fortran, C, Java 등은 index에서 integer 값만 사용할 수 있다.

하지만 Pascal, Ada에서는 ordinal type을 사용할 수 있다는 것이다.

```pascal
type
  Month = (January, February, March, April, May, June, July, August, September, October, November, December);
  TemperatureArray = array[Month] of Integer;

var
  Temperatures: TemperatureArray;

begin
  Temperatures[January] := 10;
  Temperatures[February] := 15;
  Temperatures[March] := 20;

  writeln('Temperature in January:', Temperatures[January]);
  writeln('Temperature in February:', Temperatures[February]);
  writeln('Temperature in March:', Temperatures[March]);
end.
```

key-value 형태로 동작하고 있다..?

### 적응 배열 (Conformant Array)

<a href="https://stackoverflow.com/questions/8482318/what-is-a-conformant-array">stackoverflow</a>

### 슬라이스 (slice)

슬라이스는 배열의 substructure이며 새로운 데이터 타입이 아니다.

* FORTRAN90
    ```fortran
    INTEGER VECTOR(1:10), MAT(1:4, 1:4)
    MAT(1:4, 1): the first column
    MAT(2, 1:4): the second row
    VECTOR((/3, 2, 1, 8/)): the array consists of the 3rd, 2nd, 1st, and 8th elements
    ```

### 배열 타입의 구현

* Compile-Time Descritor
    * 도프 벡터 (dope vector)
* Access Function

TODO.. p289 참고

## 연상 배열 (associative array)

인덱싱되는 순서를 갖지 않는 데이터 요소들의 모임 (key, value 쌍으로 되어 있음)

* Perl
    ```perl
    %salaries = ("Gary" => 75000, "Perry" => 57000);
    %salaries{"Perry"} = 58850;
    delete %salaries{"Gary"};
    ```
* Python
    ```python
    a = {"Gary":75000, "Perry":57000}
    a["Perry"] = 58850
    del a["Gary"]
    ```

### 연상 배열의 구현

수업에서는 언급안됨 -> 나중에 읽어보고 정리하기 (p297 참고)

## 레코드 타입 (record type)

개개의 원소들이 같은 타입, 크기가 아닌 데이터들의 모임으로 모델링할 필요가 자주 발생한다. (예: 학생 정보 - 이름(문자), 학번(정수), 학점(실수) 등)

```cobol
01 EMPLOYEE-RECORD
    02 EMPLOYEE-NAME
        05 FIRST PICTURE IS X(20)
        05 MIDDLE PICTURE IS X(10)
        05 LAST PICTURE IS X(20)
    02 HOURLY-RATE PICTURE IS 99V99
```

앞의 숫자는 수준 번호 (level number), X(20)은 20개의 영수치(alphanumeric), 99V99는 중간에 소수점을 갖는 4개의 십진수 숫자 (ex: 12.34)

일반적인 필드 접근 방법은 `Employee_Record.Employee_Name.Middle`와 같이 dot(`.`)을 사용한다. 

하지만 COBOL은 `OF` 키워드를 사용하여 `MIDDLE OF EMPLOYEE-NAME OF EMPLOYEE-RECORD`처럼 첫 번째 레코드명은 가장 작은 레코드를 가리킨다.

### 완전 자격 참조 (fully qualified reference)

위 예제처럼 해당 값을 가져올 때 모든 record 이름을 입력해야 하는 방법이다.

### 생략 참조 (elliptical reference)

`MIDDLE`, `MIDDLE OF EMPLOYEE-RECORD`, `MIDDLE OF EMPLOYEE-NAME`과 같이 모호하지 않은 일부분의 생략을 허용하는 방법이다.

프로그래머 입장에서는 편리하지만, 판독성을 해치는 방법이다. 

```pascal
type
  TEmployee = record
    FirstName: string[20];
    MiddleName: string[10];
    LastName: string[20];
    HourlyRate: Double;
  end;

var
  Employee1: TEmployee;
  CurrentEmployee: ^TEmployee;

begin
  Employee1.FirstName := 'John';
  Employee1.LastName := 'Doe';
  Employee1.HourlyRate := 12.50;

  CurrentEmployee := @Employee1;

  with CurrentEmployee^ do
  begin
    WriteLn('Employee 1: ', FirstName, ' ', LastName, ', Hourly Rate: ', HourlyRate:0:2);
  end;

  ReadLn;
end.
```

Pascal, Modula-2에서는 with을 사용하여 위 코드처럼 생략 참조를 사용할 수 있다.

### 레코드 타입의 구현

p301 참고 - 수업에서 언급 X

## 공용체 타입 (union type)

변수가 프로그램 실행 중에 다른 시기에 다른 타입의 값을 저장할 수 있는 타입이다. - 자세한 정리는 일단 보류

## 포인터 타입 (pointer type)

값은 메모리 주소 혹은 nil(null)을 가리킨다.

주소 지정으로 인한 유연성 제공, 동적 기억공간을 관리하기 위한 용도로 설계되었다. (포인터를 사용하여 동적으로 할당되는 heap 공간에 접근할 수 있다.)

포인터는 구조화된 타입이 아니다. 데이터를 저장하기보다 참조하기 위해서 사용하기 때문이다.

### 역참조 (dereferencing)

주소에 접근하여 주소의 데이터 값에 접근하는 것을 의미

C++에서 별표(`*`)를 사용하여 역참조를 명시할 수 있다.

![0](/assets/images/concepts-of-programming-languages-6/0.png)

```cpp
j = *ptr
```

```cpp
struct Main {
    int age;
    char *name;
};

struct Man m1 = {18, "Bob"};
struct Man *p;
...
p = &m1;
if (p->age > 20)
// (*p).age > 20
...
```

### 묵시적 역참조 (implicit dereferencing)

ALGOL 68, FORTRAN 90

### 명시적 역참조 (explicit dereferencing)

Pascal, C/C++, Ada

### 허상 포인터 (dangling pointer)

```cpp
int * arrayPtr1;
int * arrayPtr2 = new int[100];
arrayPtr1 = arrayPtr2;
delete [] arrayPtr2;
```

arrayPtr2를 delete하면서 arrayPtr1은 허상 포인터가 된다.

heap 공간이 반환되었기 때문이다. (동적 변수의 명시적 회수는 허상 포인터의 원인을 제공한다. - 데이터 훼손의 가능성 제공)

이는 User After Free 취약점의 원인을 제공하기도 한다.

### 분실된 힙-동적 변수 (lost heap-dynamic variable)

```cpp
p = new Integer(1);
p = new Integer(2);
```

이전에 생성한 `new Integer(1)`에는 이제 접근할 수 없는 쓰레기(garbage) 상태가 된다.

분실되어 접근할 수 없는 상태는 메모리 누수(memory leak)를 유발하기도 한다.

### 비석 접근 방법 (tombstone)

힙-동적 변수는 비석을 가리키고 비석이 해당 포인터를 가리킨다.

회수될 때도 변수는 비석을 가리키고, 비석은 nil을 가리키도록 설정한다.

에러를 감지하여 dangling pointer 문제를 해결하지만, 공간과 역참조 오버헤드가 존재한다. (비석이라는 별도의 공간 소모, 비석을 통해 접근하기 때문에 한 수준 더 많은 간접 주소지정을 요구한다.)

**이러한 추가 비용을 지불할 정도로 의미있는 안정성을 제공하지 않는다고 판단 - 안쓴다고 함**

### 잠금-키 접근 방법 (locks-and-keys approach)

포인터 값을 (키, 주소) 형태의 쌍으로 표현한다. (여기서 키는 정수 값)

역참조된 포인터에 접근할 때 포인터의 key 값을 힙-동적 변수의 잠금 값과 비교하며, 일치하지 않으면 실행 시간 오류로 처리된다. (포인터의 값을 다른 포인터로 복사할 때 반드시 key 값도 함께 복사해야 한다.)

객체가 회수될 때 잠금도 해제된다.

## 참조 타입 (reference type)

포인터는 메모리 주소를 참조하는 것이며, 참조 변수는 메모리 객체, 값을 참조하는 것 (C++에서 `&` 사용)

```cpp
void swap(int &i, int &j) { // pass-by-reference
    int t = i;
    i = j;
    j = t;
}

int result = 0;
int &ref_result = result;
...
ref_result = 100;
```

Java에서는 포인터 계산이 없으며 참조만 존재하고, 묵시적으로 회수(가비지 컬렉터에 의해 회수됨, 별도의 회수 연산자가 없음)하기 때문에 허상 참조가 존재하지 않는다. - heap에 있는 객체만 가리킬 수 있다.

## 타입 검사 (type checking)

수업 언급 X

## 타입 동등 (type equivalence)

수업 언급 X
