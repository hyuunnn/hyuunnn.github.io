---
layout: post
title: "도구 개발 아이디어 정리"
description: ""
date: 2023-01-27
tags: ["forensic", "recover", "carver", "tool"]
---

`volatility`를 사용하여 메모리를 분석할 때 파일을 추출하는 명령어인 `dumpfiles`를 사용할 때가 많다.

하지만 대부분의 바이너리 파일들은 도구가 인식하지 못하고, 인식하더라도 대부분의 데이터는 날린 채로 보여준다.

DFC2022 304 문제를 풀면서 각 아티팩트들의 recover 도구들을 사용했더니 생각보다 큰 차이를 볼 수 있었다.

![1](/assets/images/forensic-recover/1.png)

위 사진은 Chrome 브라우저의 History.db를 추출한 사진이다.

![2](/assets/images/forensic-recover/2.png)

일반적인 sqlite 뷰어 프로그램으로는 열리지 않았고, 손상된 SQLite 파일을 복구해주는 도구인 <a href="https://github.com/alitrack/undark">undark</a>를 사용했더니 위 사진과 같이 악성 파일을 다운로드 받은 중요 정보를 얻을 수 있었다.

![3](/assets/images/forensic-recover/3.png)

두 번째는 powershell 사용 기록을 확인할 수 있는 evtx 파일을 추출한 사진이다.

![4](/assets/images/forensic-recover/4.png)

역시 이벤트 뷰어 프로그램을 사용하면 데이터를 확인할 수 없었다.

![5](/assets/images/forensic-recover/5.png)

하지만 <a href="https://github.com/williballenthin/EVTXtract">EVTXtract</a>를 사용하면 이벤트 로그 데이터의 많은 부분을 볼 수 있었다.

![6](/assets/images/forensic-recover/6.png)

![7](/assets/images/forensic-recover/7.png)

마지막으로 $UsnJrnl:$J 파일을 dumpfiles로 추출 후 NTFS Log Tracker에 돌렸을 때,

<a href="https://github.com/jschicht/UsnJrnlCarver">UsnJrnlCarver</a>를 사용한 결과 파일을 돌렸을 때 차이가 매우 큰 것을 확인할 수 있다.

EnCase, FTK, Axiom 등 상용 도구들은 결과가 어떻게 나오는지 모르겠지만 오픈소스 도구들을 찾아봤을 때 활발히 연구되고 있지 않은 분야인 것 같았다.

registry 카빙 도구 중에 <a href="https://github.com/msuhanov/yarp">yarp</a>에서 `yarp-carver`를 사용했던 기억이 있는데, 원할하게 동작하지 않았던 것으로 기억하고 있다.

결론은 esedb, OLE, ETL(Event Trace Log) 등 찾아보면 연구할만한 주제가 꽤 있지 않을까 생각이 든다.

<a href="https://i.blackhat.com/us-18/Thu-August-9/us-18-Kobayashi-Reconstruct-The-World-From-Vanished-Shadow-Recovering-Deleted-VSS-Snapshots.pdf">Recovering Deleted VSS Snapshots - blackhat</a> ~~벌써 5년 전이네...~~
