---
layout: post
title: "ILEAPP, ALEAPP"
description: ""
date: 2020-12-03
tags: [Forensic, Tool, IOS, Android]
---
```
git clone https://github.com/abrignoni/iLEAPP.git
pip install -r requirements.txt
python ileappGUI.py

git clone https://github.com/abrignoni/ALEAPP.git
pip install -r requirements.txt
python aleappGUI.py
```

* Sample Image
    * Android 10 Image : <a href="https://digitalcorpora.org/corpora/cell-phones/android-10">digitalcorpora Android 10</a>
    * Android 11 Image : <a href="https://thebinaryhick.blog/2020/10/07/new-android-image-available-this-one-goes-to-11/">thebinaryhick Android 11</a>
    * IOS 13 Image : <a href="https://digitalcorpora.org/corpora/cell-phones/ios-13">digitalcorpora IOS13 13</a>

![ILEAPP](/assets/images/ILEAPP-ALEAPP/ALEAPP-01.png)

![ALEAPP](/assets/images/ILEAPP-ALEAPP/ILEAPP-01.png)

ILEAPP은 IOS 분석 도구이며, ALEAPP은 Android 분석 도구이다.

각각의 알맞은 도구를 사용하여 Process 버튼을 누르면 index.html 파일이 브라우저로 열리며, 다양한 아티팩트들을 확인할 수 있다. (오픈소스이기 때문에 꾸준히 업데이트 되고 있다.)

모바일 분야는 공부한 적이 없어서 어떤 아티팩트들이 남는지 잘 모르는데, 위 도구를 사용하여 결과를 보면서 정말 다양한 아티팩트가 남아있다는 것을 알 수 있었다.

특히 KnowledgeC.db는 Apple 디바이스의 다양한 로그를 저장한다고 알고 있었는데 ILEAPP으로 어떤 데이터들이 남는지 알 수 있었다.

![ILEAPP](/assets/images/ILEAPP-ALEAPP/ILEAPP-02.png)

카카오톡은 국내에서만 쓰이는 어플이기 때문에 ILEAPP, ALEAPP에 기능을 추가해서 사용하면 괜찮겠다는 생각이 들었다. (<a href="https://github.com/dfrc-korea/carpe/tree/master/modules/kakaotalk_mobile_decrypt">1</a>, <a href="https://github.com/dfrc-korea/carpe/blob/master/modules/kakaotalk_mobile_decrypt_connector.py">2</a>)

* 링크
    * https://www.youtube.com/c/AlexisBrignoni/videos
    * https://www.youtube.com/playlist?list=PLz61osc7c3OpOYUBx88_drHBjPFHKl24B
    * https://www.youtube.com/playlist?list=PLz61osc7c3OqQ_xBZJbzZdIkVd8HnxLmC
    * https://www.youtube.com/playlist?list=PLz61osc7c3OrTKtUiWI8mBeh08djpN6Gp
    * https://youtu.be/-VbNreAFL6I
    * https://abrignoni.blogspot.com/2020/07/dfir-resources.html