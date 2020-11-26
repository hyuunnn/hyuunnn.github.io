---
layout: post
title: "pintool 환경 세팅 방법"
description: ""
date: 2018-07-16
tags: [pintool]
comments: true
share: true
---

윈도우에서 pintool 컴파일 방법은 크게 cygwin, visual studio를 사용하는 방법 2가지가 있다.

환경 : Visual studio 2017

pin 버전 : 3.7

* 맨 처음 pin을 C:/에 복사한다. (C:/pin)

* 예제로 많이 사용되는 source/tools/MyPinTool을 visual studio로 실행한다.

![pintool](/assets/images/pintool-setting/pintool-setting-01.png)

release 모드로 무작정 컴파일을 하면 위 사진과 같은 에러가 뜬다.

![pintool](/assets/images/pintool-setting/pintool-setting-02.png)

* 위 사진과 같이 경로를 추가해준다.

![pintool](/assets/images/pintool-setting/pintool-setting-03.png)

* 위와 같은 에러가 뜨면 SAFESEH를 disable 한다.

![pintool](/assets/images/pintool-setting/pintool-setting-04.png)

![pintool](/assets/images/pintool-setting/pintool-setting-05.png)

* LNK4049로 시작하는 에러가 뜨면 아래 사진과 같이 crtbeginS.obj를 추가해준다.

![pintool](/assets/images/pintool-setting/pintool-setting-06.png)

* 컴파일 성공 !!

![pintool](/assets/images/pintool-setting/pintool-setting-07.png)