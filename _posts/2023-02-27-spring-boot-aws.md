---
layout: post
title: "스프링 부트와 AWS로 혼자 구현하는 웹 서비스"
description: ""
date: 2023-02-27
tags: ["book"]
---

<a href="http://www.yes24.com/Product/Goods/83849117">스프링 부트와 AWS로 혼자 구현하는 웹 서비스</a>, <a href="https://jojoldu.tistory.com/463">출간 후기</a>

2019년에 나온 책이기 때문에 많은 부분이 업데이트가 되었고, deprecated된 기능도 꽤 있다.

저자 분께서 이를 해결하기 위해 <a href="https://jojoldu.tistory.com/539">블로그 글</a>도 작성해주셨고, <a href="https://github.com/jojoldu/freelec-springboot2-webservice/issues">github</a>으로도 이슈를 받고 있지만 한계가 있었다.

2023-02-27 기준 <a href="https://github.com/hyuunnn/spring-aws-book">spring-aws-book</a>의 코드를 사용하여 문제 없이 책을 완주할 수 있다.

책에서는 <a href="https://start.spring.io/">spring initializr</a>을 사용하지 말라고 하는데, 이니셜라이저에서 만들어주는 설정 값이 최신 버전에서 제일 깔끔하게 동작하기 때문에 `build.gradle` 설정 값만 가져다 사용했다. 또한 `build.gradle`에서 사용된 기능들을 하나씩 찾아봤다. (저자 분은 `build.gradle`을 어떻게 작성하는지, 어떤 역할을 하는지 이해하는 것을 원하셨기 때문에 ㅇㅇ..)

### 단위 테스트는 개발단계 초기에 문제를 발견하게 도와준다.

사람은 완벽하지 않기 때문에 코드를 수정하면서 실수를 하게 된다. 하지만 테스트 코드가 일종의 안전 장치 역할을 하기 때문에 미연의 방지를 할 수 있다.

### 단위 테스트는 시스템애 대한 실제 문서를 제공한다.

프로젝트를 하면서 정말 공감했던 문장이다. repository 테스트 코드를 보고 Service, Controller를 구현하면 정상적으로 동작하는 것이기 때문에 시간을 허비하지 않고 빠르게 개발할 수 있었다.

### Service는 트랜잭션, 도메인 간 순서 보장의 역할만 한다.

```java
@Transactional
public Order cancelOrder(int orderId) {
    OrdersDto order = ordersDao.selectOrders(orderId);
    BillingDto billing = billingDao.selectBilling(orderId);
    DeliveryDto delivery = deliveryDao.selectDelivery(orderId);

    String deliveryStatus = delivery.getStatus();

    if ("IN_PROGRESS".equals(deliveryStatus)) {
        delivery.setStatus("CANCEL");
        deliveryDao.update(delivery);
    }

    order.setStatus("CANCEL");
    ordersDao.update(order);

    billing.setStatus("CANCEL");
    deliveryDao.update(billing);

    return order;
}
```

```java
@Transactional
public Order cancelOrder(int orderId) {
    Orders order = ordersRepository.findById(orderId);
    Billing billing = billingRepository.findByOrderId(orderId);
    Delivery delivery = deliveryRepository.findByOrderId(orderId);

    delivery.cancel();

    order.cancel();
    billing.cancel();

    return order;
}
```

도메인은 DB와 매핑되었기 때문에 서비스에 비지니스 로직을 구현하는 경우가 있다.

하지만 서비스에 기능이 많이 있으면 위와 같이 코드가 불필요하게 길어진다.

2번째 `cancelOrder`는 각 도메인 클래스에 적절하게 `cancel()` 메서드를 만들어서 직관적인 코드를 만들 수 있게 된다.

