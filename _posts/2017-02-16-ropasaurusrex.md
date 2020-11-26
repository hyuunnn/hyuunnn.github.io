---
layout: post
title: "Plaid CTF 2013 ropasaurusrex"
description: "ropasaurusrex"
date: 2017-02-16
tags: [ropasaurusrex, plaid ctf, pwnable]
comments: true
share: true
---

rop를 입문 할 때 좋은 문제라고 해서 풀어봤다.

![ropasaurusrex](/assets/images/ropasaurusrex/ropasaurusrex-01.png)

NX 보호기법이 걸려있어서 실행권한이 없기 때문에 쉘코드를 이용한 공격은 할 수 없다.

![ropasaurusrex](/assets/images/ropasaurusrex/ropasaurusrex-02.png)

![ropasaurusrex](/assets/images/ropasaurusrex/ropasaurusrex-03.png)

WIN이라고 뜨기 전에 함수를 실행하는게 있어서 들어가봤더니

read함수가 256바이트까지 읽을 수 있었고 바이너리의 buf는 테스트를 통해 136바이트라는 것을 알아냈다.

read함수를 system함수로 바꿔서 /bin/sh를 실행하는 방식으로 nx를 우회 할 수 있다.

![ropasaurusrex](/assets/images/ropasaurusrex/ropasaurusrex-04.png)

해당 바이너리에는 dynamic 영역에 /bin/sh를 담기 위한 충분한 공간이 있어서 사용했다.

pppr를 이용해서 함수에 알맞는 값(file descriptor, address, size)을 넣고 ret가 다음 함수를 가리키면서

쭉 이어지게 payload를 짜면 된다.

첫번째 dynamic 영역에 /bin/sh를 넣을 준비를 한다. 그 다음 write 함수를 이용해서 변환 할 read_got의 값을 저장한다.

그리고 read_got를 system으로 overwrite한다.

이제 read함수를 실행하면 system함수를 실행하기 때문에 system 함수에 맞게 넣어주면 된다.

<script src="https://gist.github.com/hy00un/fb2589628398e71fc5283c17c3bb457d.js"></script>
