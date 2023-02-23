---
layout: post
title: "자바 코딩의 기술"
description: ""
date: 2023-02-22
tags: ["book"]
---

프로그래밍, 개발을 오래했던 사람이라면 무난하게 읽고 넘어갈만한 책이다. (대부분의 사람들이 알만한 내용들이다.) 그럼에도 괜찮은 조언들을 얻어갈 수 있었다.

## 직접 만들지 말고 자바 API 사용하기

당연한 이야기라고 생각하겠지만 생각보다 자바에서 이미 구현되어 있는 API가 정말 많다. 예를 들어 누군가는 `null`을 if 문으로 체크하고 예외 처리를 하겠지만 `Objects.requireNonNull`를 사용할 수도 있다. 그외에도 `Collections`에도 다양한 기능들이 구현되어 있다.

그렇기 때문에 자바 API 문서, 관련 책(<a href="http://www.yes24.com/Product/Goods/113416654">1</a>, <a href="http://www.yes24.com/Product/Goods/92529658">2</a>)들을 찾아보고 공부해야 한다.

## 예제로 설명하기

지금까지 개발하면서 주석에는 코드와 관련된 설명을 적었는데 API 문서를 보면 **사용 방법**을 포함하는 주석을 많이 볼 수 있다. (**예제로 본을 보인다.**)

```java
/** 
 * S로 시작해 숫자 다섯자리 재고 번호가 나오고
 * 뒤이어 앞의 재고 번호와 구분하기 위한 역 슬래시가 나오고
 * ...
*/

/**
 * 형식: "S<inventory-number>\<COUNTRY-CODE>.<name>"
 *
 * 예시: "S12345\US.pasta", "S08342\CN.wrench"
 *
 * 유효하지 않은 예:
 * "R12345\RU.fuel." (재고가 아닌 자원)
 * "S1234\US.light" (숫자가 다섯 개여야 함)
 * "S01234\AI.coconut" (잘못된 국가 코드)
*/
static final Pattern CODE = Pattern.compile("^S\\d{5}\\\\(US|EU|RU|CN)\\.[a-z]+$");
```

정규표현식을 설명하는 주석에서 전자보다 후자가 훨씬 직관적이고 바로 이해할 수 있다. **백문이 불여일타**와 비슷한 맥락인 것 같다. 나 같은 경우에도 실제 API의 설명을 읽기보다 일단 예제 코드를 돌려보고 결과가 어떻게 나오는지 확인하고 있다.

구현 과정에서 트레이드 오프가 있는 코드에는 왜 그렇게 작성했는지 주석을 적는 경우도 있는데, `README.md` 파일을 만들어서 별도의 설명 파일을 만드는 것도 좋은 것 같다.

## JavaDoc을 활용하기

p95-p105

## 축약 쓰지 않기

약자, 축약한 이름으로 지정하면 어떻게 동작하는지 이해하는데 시간이 걸린다. 흔히 사용되는 **CSV** 같은 경우 그대로 사용해도 괜찮지만, 나머지는 전체 이름을 사용해라.

하지만 `bufferedWriter`보다 `writer`처럼 짧은 이름이 더 직관적인 경우도 있다. (굳이 buffer에 넣는다고 알 필요는 없다. - 클래스명을 봐도 buffer 관련임을 알 수 있다.)

결론은 축약은 매우 일반적일 때만 사용하고 확신이 없다면 풀어서 써라.

*이름은 쓸 일보다 읽힐 일이 훨씬 많다. 오해의 소지가 있거나 이해하기 어려우면 유지보수에 어려움을 준다.*

## 도메인 용어 사용하기

클래스의 역할에 맞는 필드명으로 설정해야 한다.

```java
class Person {
    String lastName;
    String role;
    int travels;
    LocalDate employedSince;

    String serializeAsLine() {
        return ~~;
    }
}

class Astronaut {
    String tagName;
    String rank;
    int missions;
    LocalDate activeDutySince;

    String toCsv() {
        return ~~;
    }
}
```

화성 탐사 작전에서 사용되는 코드라고 가정했을 때 `Person`은 큰 범주에 있다. 반면에 `Astronaut`는 우주비행사의 정보임을 한 번에 이해할 수 있다.

그리고 `lastName`을 보고 왜 성만 저장하는거지? 생각할 수 있지만 `tagName`은 우주복에 붙어있는 이름표에 성이 적혀있구나 이해할 수 있다.

**더 나은 이름이 떠오를 때까지 항상 리팩토링하자.**

또한 어떤 프레임워크에서는 자바 규칙을 따르지 않으면 오류가 발생할 수도 있는데 이런 부분도 네이밍의 중요성이라고 볼 수 있겠다.

## 빈 catch 블록 설명하기

빈 catch 블록은 어떤 의도인지 불분명하다. 책에서는 `e` 대신에 `ignored`(흔한 `e`보다 더 나은 이름)를 사용하고 `조건(CONDITION) -> 영향(EFFECT)` 형식으로 주석을 적는 방법을 제안했다.

## 실제 값보다 기대 값을 먼저 적기

```java
Assertions.assertEquals(cruiseControl.getTargetSpeedKmh(), 7667);
// expected: <1337> but was <7667>
```

위 오류는 1337이 올바른 결과이고 7667이 잘못된 결과이라는 의미인데, 사실은 7667이 올바른 결과이다. 테스트 코드 동작에는 문제가 없지만 나중에 복잡한 테스트 코드에서 디버깅할 때 문제가 발생할 수 있다.

## 부동소수점 테스트에서는 약간의 오차를 허용해야 한다.

```java
static final double TOLERANCE = 0.00001;
Assertions.assertEquals(0.114, tank.getStatus(), TOLERANCE);
// assertEquals(double expected, double actual, double delta)
```

부동소수점은 근사값이기 때문에 완전한 일치는 존재하지 않는다.

## 독립형 테스트 사용하기

`@BeforeEach`, `@BeforeAll`은 반복적인 테스트에 필요한 코드들을 미리 선언할 수 있다.

하지만 테스트 코드가 길어지고, 복잡하면 코드를 이해할 때마다 스크롤을 위로 올려서 봐야한다는 것이다.

```java
class OxygenTankTest {
    static OxygenTank createHalfFilledTank() {
        OxygenTank tank = OxygenTank.withCapacity(10_000);
        tank.fill(5_000);
        return tank;
    }

    @Test
    void depressurizingEmptiesTank() {
        OxygenTank tank = createHalfFilledTank();

        tank.depressurize();

        Assertions.assertTrue(tank.isEmpty());
    }

    @Test
    void completelyFillTankMustBeFull() {
        OxygenTank tank = createHalfFilledTank();

        tank.fillUp();

        Assertions.assertTrue(tank.isFull());
    }
}
```

책에서는 스크롤 없이 테스트 메서드만 봐도 이해할 수 있다면 훌률한 코드이며, `@BeforeEach`를 가능하면 쓰지 말라고 한다. 프로그래머 입장에서 코드를 읽기 힘들어지기 때문이다. (trade off가 있다.)

## 참조 누수 피하기

```java
public class Inventory {

  private final List<Supply> supplies;

  Inventory(List<Supply> supplies) {
    this.supplies = supplies;
  }

  List<Supply> getSupplies() {
    return supplies;
  }
}
```

```java
List<Supply> externalSupplies = new ArrayList<>();
Inventory inventory = new Inventory(externalSupplies);

inventory.getSupplies().size(); // 0
externalSupplies.add(new Supply("Apple"));
inventory.getSupplies().size(); // 1

inventory.getSupplies().add(new Supply("Banana"));
inventory.getSupplies().size(); // 2
```

첫 번째 문제는 생성된 하나의 `ArrayList`를 공유하여 사용하고 있기 때문에 초기화 이후의 데이터를 마음대로 수정할 수 있다. 

두 번째는 `getSupplies()`가 `list`를 반환하기 때문에 `add`를 사용하여 데이터를 수정할 수 있다.

```java
public class Inventory {

  private final List<Supply> supplies;

  Inventory(List<Supply> supplies) {
    this.supplies = new ArrayList<>(supplies);
  }

  List<Supply> getSupplies() {
    return Collections.unmodifiableList(supplies);
  }
}
```
