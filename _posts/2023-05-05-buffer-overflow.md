---
layout: post
title: "Buffer Overflow"
description: ""
date: 2023-05-05
tags: [pwnable]
---

<a href="https://hyuunnn.github.io/2023/05/05/stack-frame/">스택 프레임 설명</a>

```c
// bof.c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

void get_shell() {
  char *shell[2];
  shell[0] = "/bin/sh";
  shell[1] = NULL;
  execve(shell[0], shell, NULL);
}

int main() {
  char buf[16];
  printf("[buf addr] = %p\n", buf);
  printf("[get_shell addr] = %p\n", get_shell);
  gets(buf);
  printf("%s\n", buf);
}
```

```console
gcc -o bof bof.c -fno-stack-protector -fno-PIE -no-pie
```

get_shell 함수는 shell을 띄우기 위해 임의로 만들었다.

buf 변수는 16 바이트의 공간을 만들었고, gets 함수를 사용하여 입력한 값을 buf에 넣는다.

하지만 gets 함수를 사용했을 때 입력 값의 길이를 체크하지 않아서 16바이트를 넘는 입력 값을 넣었을 때 Buffer Overflow가 발생하게 된다.

![1](/assets/images/buffer-overflow/1.png)

SFP:0x7fffffffddd0, RET:0x4011d5

위 사진은 스택 프레임 설명에서 사용한 사진이며, 16바이트 버퍼 이후에 8바이트의 SFP(Stack Frame Pointer)까지 덮어준 후에 원하는 RET 주소를 넣음으로써 원하는 기능을 수행할 수 있는 것이다.

8바이트인 이유는 32비트(4바이트), 64비트(8바이트)의 차이에 있다.

![2](/assets/images/buffer-overflow/2.png)

<a href="https://hyuunnn.github.io/2023/04/28/python3-print/">sys.stdout.buffer.write를 사용해야 하는 이유</a>
