---
layout: post
title: "Investigating Windows Systems 책 후기"
description: "Investigating Windows Systems - harlan carvey"
date: 2020-11-08
tags: [Forensic, Book]
---

* 분석 계획 만들기
    1. 아티팩트는 매우 다양하게 있으며, 이를 모두 외우는 것은 불가능
    2. 악성코드 탐지를 위한 방법, 정보 유출을 탐지하기 위한 방법은 다 다름
        - 윈도우 버전에 따라서도 아티팩트의 존재 유무, 새로운 아티팩트 등등 다양하게 존재
    3. 제한 시간 내에 효과적인 분석 가능
    4. 분석을 마친 후에는 분석에 대한 평가, 정리를 하는 것이 중요
        - 언급된 아티팩트가 다른 케이스에서 다시 언급된다면 이는 중요한 아티팩트임을 의미할 수도 있음
    5. 더 나아가 이러한 데이터 분석을 자동화하기 위하여 플러그인 개발이나 툴 개발을 통하여 분석 과정을 개선할 수 있음

* 타임라인이 중요한 이유
    1. 악성코드 케이스에서 악성코드를 찾아냈더라도 언제부터 얼마나 PC에 어떻게 들어왔는지 알아야함
    2. 감염 시점을 파악하여 어떻게 감염되었는지 확인할 수 있음
    3. 악성 파일이 존재한다는 이유만으로 실행되었다고 볼 수 없음 -> 실행되었다는 증거를 확보해야함

* 다양한 도구 활용 및 플러그인 개발
    1. volatility에서 보여주지 못한 네트워크 통신을 bulk_extractor를 통하여 확인하는 등 도구마다 장단점 존재 (<a href="https://www.datadigitally.com/2019/07/extracting-pcap-from-memory-image.html">bulk_extractor를 사용하여 메모리에서 pcap 추출</a>)
    2. 플러그인 개발, 툴 개발을 통하여 새로운 아티팩트 분석을 자동화 및 공유 가능

* 느낀점
    - 할렌 카비는 책에서 도구화의 중요성을 여러번 강조하고 있다.
    - 실무를 경험한 적은 없지만 포렌식 대회에서 사용된 이미지를 분석할 때 각각의 도구를 활용하여 타임라인의 연결고리를 확인할 때 불편함이 존재하였다. ~~(plaso라는 타임라인 도구가 있긴 함)~~
    - 하지만 할렌 카비가 만든 도구는 여러 곳에서 수집한 아티팩트들을 동일한 구조의 텍스트로 만들어서 타임라인을 한 번에 확인할 수 있었다. 
    - <a href="https://www.sleuthkit.org/">The Sleuth Kit</a> output 형식으로 제작하는 기능도 인상 깊었다. ~~(정형화된 데이터)~~ <a href="https://forensicswiki.xyz/wiki/index.php?title=Bodyfile">bodyfile format</a>

* 도구
    - <a href="https://github.com/keydet89/Tools">keydet89 Tools</a>
    - <a href="https://github.com/keydet89/RegRipper3.0">RegRipper3.0</a>
    - <a href="https://metacpan.org/pod/Parse::Win32Registry">Parse::Win32Registry (perl)</a>
    - <a href="https://github.com/simsong/bulk_extractor">bulk_extractor</a>

* 도구 사용법?
    - ftkparse.pl
        1. ftk imager의 Export Directory Listing 기능을 통하여 모든 파일의 타임라인 추출 (output:l.csv)
        2. ``ftkparse.exe ../../l.csv > bodyfile.txt`` 데이터를 tsk 형식에 맞게 수정

    - bodyfile.pl
        1. ``bodyfile.exe -f bodyfile.txt > events.txt`` 5개 필드로 구성된 tln.exe 프로그램의 형식으로 변환

    - pref.pl
        1. ``pref.exe -d "E:\WINDOWS\Prefetch" -t >> events.txt`` 프리패치 타임라인 저장

    - evtparse.pl, evtxparse.pl
        1. ``evtparse.exe -d "E:\WINDOWS\System32\config" -t >> events.txt`` evt 데이터 저장
        2. ``evtxparse.exe -d "E:\WINDOWS\System32\config" -t >> events.txt`` evtx 데이터 저장
        3. -s 옵션을 사용하여 타임스탬프를 출력하여 시스템 시간이 수정된 것도 확인할 수 있다.

    - regtime.pl
        1. ``regtime.exe -m HKLM/Software -r "E:\WINDOWS\system32\config\software" >> events.txt`` 레지스트리 타임라인 저장
        2. ``regtime.exe -m HKCU -u "Mr. Evil" -r "E:\Documents and Settings\Mr. Evil\NTUSER.DAT" >> events.txt``

    - rip.pl (RegRipper)
        1. ``rip.exe -r "E:\Documents and Settings\Mr. Evil\NTUSER.DAT" -p userassist_tln >> events.txt``
        2. ``rip.exe -r "E:\Documents and Settings\Mr. Evil\NTUSER.DAT" -p recentdocs_tln -u "Mr. Evil" >> events.txt``

    - tin.pl
        1. ``tln.exe`` 수동으로 타임라인을 넣는 프로그램이며 필요한 경우에만 사용

    - parse.pl
        1. ``parse.exe -f events.txt > tln.txt`` 지금까지 저장한 데이터(events.txt)들을 타임라인 순서로 정렬

    * 전체적인 타임라인 수집 후 regripper를 사용하여 레지스트리 아티팩트 저장 -> parse.pl를 통하여 타임라인 순 정렬