---
layout: post
title: "hindsight 사용법"
description: ""
date: 2020-12-02
tags: [Forensic, Tool, Web]
---

```
git clone https://github.com/obsidianforensics/hindsight.git
pip install -r requirements.txt (설치 오류시 pip install plyvel-win32 후 plyvel를 제외한 나머지 라이브러리 설치)
python setup.py build
python setup.py install
python hindsight_gui.py
http://localhost:8080 접속
```

![hindsight](/assets/images/hindsight/0.png)
    
xlsxwriter를 사용하여 cache 데이터는 회색, url은 검은색, json 데이터는 파란색으로 처리하여 보여줌