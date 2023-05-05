---
layout: post
title: "Stack Frame"
description: ""
date: 2023-05-05
tags: [pwnable, stack]
---

```c
// ex1.c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
void qweqwe() {
    char bufff[16];
    gets(bufff);
    printf("%s\n", bufff);
}
void qwe() {
    char buff[16];
    gets(buff);
    printf("%s\n", buff);
}
int main() {
    char buf[16];
    printf("[buf addr] = %p\n", buf);
    gets(buf);
    printf("%s\n", buf);
    qwe();
}
```

```console
gcc –o ex1 ex1.c -fno-stack-protector -fno-PIE -no-pie
```

위 코드는 `main`함수 내에서 `qwe`함수를 호출하고 있으며 `qwe`함수 내에서 `qweqwe`함수를 호출하고 있다.

`buf`에는 aaaaaaaaaaaa, `buff`에는 bbbbbbbbbbbb, `bufff`에는 cccccccccccc를 입력할 예정이다. (12바이트)

![1](/assets/images/stack-frame/1.png)

`main`함수의 적절한 곳에 breakpoint를 설정하고 실행해보자. (입력을 받는 get 함수 호출 전)

![2](/assets/images/stack-frame/2.png)

`call qwe`를 호출하기 전까지 이동해보자 (gdb 명령어인 n을 입력하면 된다.)

![3](/assets/images/stack-frame/3.png)

`qwe`함수 내부의 어셈블리 코드로 들어가기 위해서 s(step) 명령어를 사용해보자.

n(next) 명령어를 사용하면 `main`함수의 다음 코드(`main+73`)가 실행된다. (이전 사진 참고)

![4](/assets/images/stack-frame/4.png)

RSP: 0x7fffffffdde8, RBP: 0x7fffffffde00

n(next) 명령어를 하나씩 사용해보면서 스택 프레임의 동작 과정을 확인해보자.

pwndbg의 시각화 결과를 보면 스택은 밑에서 위로 쌓이면서 주소가 작아지고 있다. 이때 RBP는 스택의 맨 아래인 시작점이며, RSP는 스택의 꼭대기이다.

RBP는 스택의 시작 주소가 저장되어 있으며, RSP는 꼭대기에서 주소를 빼면서 스택을 확보하게 된다.

RSP: 0x7fffffffdde8 -> 0x7fffffffdde0, RBP: 0x7fffffffde00

![5](/assets/images/stack-frame/5.png)

`push rbp` 명령을 수행했을 때 RSP 주소가 -8 되었고 이 주소(0x7fffffffdde0)에는 이전 스택 프레임 주소 (0x7fffffffde00)가 저장(push)된다.

즉 `main`으로 돌아갈 때 복구할 스택의 주소를 메모리에 보관한다. (`main`에서 `qwe`를 호출했으니까 `qwe`의 역할이 끝난 후에 `main`에서 하던 일을 마저 해야한다.)

![6](/assets/images/stack-frame/6.png)

RSP: 0x7fffffffdde0, RBP: 0x7fffffffdde0

이제 `qwe`함수에서 새롭게 사용할 스택 공간을 사용하기 위해 `mov rbp, rsp`를 수행하여 RSP와 RBP를 같게 한다.

![7](/assets/images/stack-frame/7.png)

RSP: 0x7fffffffdde0 -> 0x7fffffffddd0, RBP: 0x7fffffffdde0

`sub rsp, 0x10`을 수행하여 16바이트만큼 스택의 공간이 확보되었다. 해당 함수의 모든 코드를 수행한 후 `leave(mov rsp, rbp; pop rbp) ret;`을 통해 이전 스택 프레임 주소로 돌아간다.

![8](/assets/images/stack-frame/8.png)

이전과 동일한 방법으로 n과 s 명령어를 적절하게 사용하여 `qweqwe`함수까지 들어가보자!

![9](/assets/images/stack-frame/9.png)

`qweqwe`함수에 있는 `gets`함수까지 실행했다면 총 3번의 입력을 넣었다.

이제 메모리에 어떻게 데이터가 저장되어 있는지 확인해보자.

![10](/assets/images/stack-frame/10.png)

현재 `qweqwe`함수를 수행 중인 상태이다.

RSP 레지스터 주소의 메모리에 남아있는 값을 보면 0x63(c)를 12번 입력한 값이 저장되어 있다.

12번 입력했으므로 마지막 4바이트는 0x00000000으로 되어있다. (16바이트 공간을 확보했기 때문이다.)

다음으로 0x7fffffffddd0은 반환할 예정인 이전 함수 스택의 시작점 SFP(Stack Frame Pointer) 주소인 것이다.

마지막으로 0x4011d5는 `qwe`함수를 이어서 수행하기 위한 return 주소이다.

![11](/assets/images/stack-frame/11.png)

0x7fffffffddd0의 메모리를 확인해보니 0x7fffffffddf0을 가리키고 있다.

해당 주소에서 buf의 크기(16)만큼 마이너스한 메모리 주소를 확인해보면 0x62(b)를 입력한 값이 저장되어 있다.

![12](/assets/images/stack-frame/12.png)

return 주소인 0x4011d5를 확인해보니 `qwe+51`을 가리키고 있다.

![13](/assets/images/stack-frame/13.png)

`qwe+51`은 `call qweqwe`를 수행한 후 다음으로 실행해야 하는 주소이다.

![14](/assets/images/stack-frame/14.png)

0x7fffffffddf0 메모리를 보니 0x1이 있다. 더 이상 연결된 스택 프레임이 존재하지 않다.

![15](/assets/images/stack-frame/15.png)

return 주소인 0x401221를 확인해보니 `main+73`을 가리키고 있다.

![16](/assets/images/stack-frame/16.png)

`main+73`은 `call qwe`를 수행한 후 다음으로 실행해야 하는 주소이다.

### 정리

`push rbp`를 통해 이전 함수 스택의 시작점을 메모리에 보관한 후에 수행해야 하는 함수가 실행된다.

해당 함수를 마무리하고 빠져나올 때 메모리에 보관했던 주소로 스택의 시작점을 복구한다.

그다음 이어서 수행할 명령어인 RET 주소를 실행한다.

