---
layout: post
title: "Codegate 2017 angrybird"
description: "angrybird"
date: 2017-02-14
tags:  [codegate, angr, pwnable]
comments: true
share: true
---

![angrybird](/assets/images/angrybird/angrybird-01.png)
![angrybird](/assets/images/angrybird/angrybird-02.png)

ida의 기능인 그래프 뷰로 보면 정말 끝이 없을 정도로 많이 있다.

여기서는 angr을 이용해서 원하는 주소 값을 들어가고 원하지 않는 주소 값은 피하면 된다.

하지만 angr가 분석 할 경로를 제대로 찾지 못해서 blank_state를 이용해 주소 값을 입력해줬다.

<a href="https://docs.angr.io/docs/paths.html" target="_blank">angr 가이드</a>

![angrybird](/assets/images/angrybird/angrybird-03.png)

<script src="https://gist.github.com/hyuunnn/4bb3fb859e93f4c3025ef8d891096864.js"></script>
