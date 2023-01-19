---
layout: post
title: "스프링 부트와 AWS로 혼자 구현하는 웹 서비스"
description: ""
date: 2023-01-17
tags: ["book"]
---

<a href="http://www.yes24.com/Product/Goods/83849117">스프링 부트와 AWS로 혼자 구현하는 웹 서비스</a>

2019년에 나온 책이기 때문에 많은 부분이 업데이트가 되었고, deprecated된 기능도 꽤 있다.

저자 분께서 이를 해결하기 위해 <a href="https://jojoldu.tistory.com/539">블로그 글</a>도 작성해주셨고, <a href="https://github.com/jojoldu/freelec-springboot2-webservice/issues">github</a>으로도 이슈를 받고 있지만 한계가 있다.

그런데 사실 build.gradle 설정은 <a href="https://start.spring.io/">spring initializr</a>에서 만들어주는 설정 값들을 가져다 사용하면 된다. (저자 분은 build.gradle을 어떻게 작성하는지, 어떤 역할을 하는지 이해하는 것을 원하셨기 때문에 ㅇㅇ..)

이니셜라이저에서 만들어주는 plugins, sourceCompatibility 버전, dependencies 등을 변경하고 build 해주면 깔끔하게 동작한다.

아직 도입부라서 나중에 어떤 문제가 발생할지는 모르겠지만.. 앞으로 개발할 때 최신 버전을 쓰지 구버전을 쓰진 않을테니..

최신 버전에 초점을 맞추고, 문제가 발생하면 최신 버전에서 어떤게 바뀌었는지 등 해결 방법을 찾아봐야겠다.
