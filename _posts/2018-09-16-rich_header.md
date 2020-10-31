---
layout: post
title: "Rich Header"
description: ""
date: 2018-09-16
tags: [PE]
comments: true
share: true
---


### 공부하게 된 이유
<a href="https://securelist.com/the-devils-in-the-rich-header/84348/">the-devils-in-the-rich-header</a>

요약 : 평창올림픽(olympic destroyer) 악성코드 제작자가 Lazarus 그룹이라는 이야기가 있었음

rich header를 확인해본 결과 6.0에서 컴파일 한 bluenoroff 샘플과 동일한 값을 가지고 있음

하지만 olympic destroyer 샘플은 mscoree.dll 참조와 msvc 라이브러리에 대한 오류 메시지 등등 6.0에서 나올 수 없는 데이터들이 있음

-> bluenoroff 샘플을 의도적으로 유사하게 만들었다는 결론


### Rich header란?

참고 자료에 있는 the_microsoft_rich_header 글을 읽어보면 문서화되지 않은 구조이며, obj, 링커 등등 컴파일 정보가 있다고 함

즉 컴파일 환경에 대한 정보가 있다고 볼 수 있음 -> 특정 공격 그룹의 rich header를 비교하여 어느정도 유사하다는 부가 기능으로 사용될 수 있음

### Rich header 분석 방법

1. offset 0x3c부터 4바이트가 rich header size

2. offset 0x80부터 Rich header가 시작됨

3. Rich라는 문자열 이후 4바이트가 XOR key로 사용

4. 하지만 0x80부터 4바이트를 복호화하면 DanS라는 문자열이 동일하게 사용되기 때문에 4바이트만 긁어온 후 DanS로 xor 시키면 그 값이 XOR key임

5. 4바이트 씩 각각 xor key로 xor 연산을 하면 복호화 성공

6. 체크섬 검사 후 prodid 값과 매칭시켜서 이 값을 이용하여 visual studio 몇 버전에서 사용된 링커, obj인지 결과를 보여주는 방식

첫 번째 코드는 hexdump, linker 정보까지 보여주는 코드

두 번째 코드는 decode한 rich header를 md5 hash로 저장 해주는 코드

yara에서는 hash.md5(pe.rich_signature.clear_data) == "MD5 hash" 형식으로 사용

XOR key 값이 다름에도 decode한 rich header 결과가 같을 수 있음

<script src="https://gist.github.com/hy00un/a5a601b5bdae1504b4434f5ea7076f3f.js"></script>

### 참고 자료

<a href="https://gist.github.com/skochinsky/07c8e95e33d9429d81a75622b5d24c8b">skochinsky gist</a>

<a href="http://bytepointer.com/articles/the_microsoft_rich_header.htm">the_microsoft_rich_header</a>

<a href="https://github.com/CIRCL/PyRichHeader">PyRichHeader</a>

<a href="https://ntcore.com/files/richsign.htm">richsign - ntcore</a>

<a href="https://infocon.hackingand.coffee/Hacktivity/Hacktivity%202016/Presentations/George_Webster-and-Julian-Kirsch.pdf">George_Webster-and-Julian-Kirsch.pdf</a>

<a href="https://github.com/dishather/richprint/blob/master/comp_id.txt">prodid list</a>