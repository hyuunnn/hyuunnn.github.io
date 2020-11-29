---
layout: post
title: "LogonTracer"
description: "LogonTracer - JPCERT"
date: 2020-11-30
tags: [Forensic, Tool, evtx]
---

LogonTracer는 이벤트 로그에서 악성 로그온 행위를 탐지하여 시각화를 해주는 tool

구글 검색 엔진으로 활용되는 알고리즘인 <a href="https://github.com/JPCERTCC/LogonTracer/blob/bd97a968e4bddc82948adc1d52b81912c4db94b2/logontracer.py#L516">PageRank</a> 그리고 <a href="https://github.com/hmmlearn/hmmlearn">HMM(Hidden Markov Model)</a>, <a href="https://github.com/shunsukeaihara/changefinder">changefinder</a>가 사용되었다고 한다. (공부해야겠다..)

```
git clone https://github.com/JPCERTCC/LogonTracer.git
pip install -r requirements.txt
```
사용되는 evtx 분석 라이브러리는 rust를 python에 포팅한 것이므로 ```pip install setuptools_rust```

현재 사용하는 PC에서는 hmmlearn 설치시 에러가 발생한다.

<a href="https://www.lfd.uci.edu/~gohlke/pythonlibs/#hmmlearn">hmmlearn whl</a>에서 whl 파일을 받아서 설치

```pip install hmmlearn‑0.2.4‑cp39‑cp39‑win_amd64.whl```

사실 더 좋은 방법이 있는데 그냥 docker 이미지를 받아서 사용하면 된다.

```
https://github.com/JPCERTCC/LogonTracer/wiki/jump-start-with-docker

docker pull jpcertcc/docker-logontracer

$ docker run \
   --detach \
   --publish=7474:7474 --publish=7687:7687 --publish=8080:8080 \
   -e LTHOSTNAME=127.0.0.1 \
   jpcertcc/docker-logontracer
```

메인 페이지 오른쪽에서 의심되는 계정 목록을 확인할 수 있다.

왼쪽에는 다양한 카테고리가 존재하며, 버튼을 누르면 결과를 보여준다.

타임라인 기능을 통하여 해당 날짜에 어떤 로그가 많이 발생했는지 확인할 수 있다.

ElasticSearch로 데이터를 넘길 수도 있다.

* 링크
    * https://github.com/JPCERTCC/LogonTracer
    * https://www.youtube.com/watch?v=_shpz5RnppA
    * https://www.youtube.com/watch?v=aX-vTd7-moY
    * https://www.youtube.com/watch?v=ISbbzaCGBns
