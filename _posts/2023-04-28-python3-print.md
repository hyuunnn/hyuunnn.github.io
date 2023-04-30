---
layout: post
title: "커맨드 라인에서 파이썬으로 값을 넣을 때 발생하는 문제"
description: ""
date: 2023-04-28
tags: [pwnable, python]
---

pwnable 문제를 풀 때 대부분의 사람들은 <a href="https://github.com/Gallopsled/pwntools">pwntools</a>를 사용한다.

하지만 예전에 LOB(Lord Of Buffer overflow)를 풀을 때 command line을 사용했던 기억이 나서 python3에서 사용해보니 생각했던 결과와 다른 값이 메모리에 저장되었다.

원인을 알아보기 위해 xxd 명령어로 확인해봤더니 값이 다르게 남는 것을 확인할 수 있었다.

![1](/assets/images/python3-print/1.png)

마지막에 남는 `0x0a`는 개행(LF)을 의미한다.

### 첫 번째 결과

python3은 ascii에서 unicode로 바뀌면서 `b"A"`처럼 `b`를 사용해야 하는데 첫 번째 결과를 보면 A가 24개이므로 `*24`는 정상적으로 동작했다. 

하지만 `\x96\x11\x40\x00`, `b'` 문자 자체가 ascii 코드로 출력됨을 확인할 수 있었다.

gdb를 사용하여 메모리에 정말 그렇게 남는지도 확인하였다.

```console
r <<< $(python3 -c 'print(b"A"*24 + b"\x96\x11\x40\x00")')
```

![6](/assets/images/python3-print/6.png)

### 두 번째 결과

두번째는 `0x41` ascii 값이 정상적으로 들어갔지만 그 이후에 `0xc2`라는 의문을 모르는 값이 들어갔고 그 이후에 `\x96\x11\x40\x00`이 추가되었다. 

![3](/assets/images/python3-print/3.png)

출처: <a href="https://secgroup.dais.unive.it/teaching/security-course/tips-for-the-challenges/">Tips for the challenges</a>

`0x80`보다 큰 값이 들어오면 `0xc2`라는 값이 추가된다는 것이다. 테스트를 해보니 그렇게 동작했다..

![4](/assets/images/python3-print/4.png)

```console
r <<< $(python3 -c 'print("A"*24 + "\x96\x11\x40\x00")')
```

![7](/assets/images/python3-print/7.png)

### 마지막 결과

마지막은 이전에 참고한 글처럼 `std.stdout.buffer.write`를 사용한 결과 깔끔하게 값이 출력되는 것을 확인할 수 있었다.

```console
r <<< $(python3 -c 'import sys;sys.stdout.buffer.write(b"A"*24 + b"\x96\x11\x40\x00\x00\x00\x00\x00")')
```

![8](/assets/images/python3-print/8.png)

python2는 2가지 방법 모두 동일한 결과를 보여주고 있다.

![2](/assets/images/python3-print/2.png)

마지막으로 `;cat`을 사용하여 파이프라인을 넘겨주면 되겠다.

![5](/assets/images/python3-print/5.png)

### cat의 역할

<a href="https://www.hackerschool.org/HS_Boards/zboard.php?id=QNA_system&page=1&sn1=&divpage=1&sn=off&ss=on&sc=on&select_arrange=headnum&desc=asc&no=1950">;cat을 사용하는 이유</a>

<a href="https://github.com/Gallopsled/pwntools/blob/dev/pwnlib/tubes/tube.py#L862">pwntools interactive()</a>

<a href="https://stackoverflow.com/questions/68137400/using-pwntools-process-interactive-mode-to-control-python3">Using pwntools process interactive mode to control python3</a>

### 참고

<a href="https://oz1ng019.tistory.com/125">oz1ng019님의 글</a>

<a href="https://secgroup.dais.unive.it/teaching/security-course/tips-for-the-challenges/">Sending bytes with python3</a>
