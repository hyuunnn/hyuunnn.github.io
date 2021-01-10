---
layout: post
title: "바이너리 분석 도구 Plugin 개발에 도움을 주는 것들"
description: ""
date: 2021-01-10
tags: [IDA, IDAPython, Plugin]
---

IDAPython 사용에 도움을 주는 플러그인을 직접 사용해보고 정리하였다.

IDA를 통해 설치되는 python의 pip 버전이 매우 낮으므로 업그레이드를 해야한다.

``C:\\python27-x64\\python.exe -m pip install --upgrade pip``

* <a href="https://github.com/eset/ipyida">ipyida</a>
    * IDA에 사용되는 python 경로에서 ``python setup.py build, python setup.py install``
    * 잘 동작하지만 외부 프로그램(jupyter)으로 사용할 수 없는 것 같다. IDA 내부에서 ipython으로 사용 가능하다.
    * 하지만 matplotlib를 통한 시각화라던지 난독화된 문자열을 IDAPython을 활용하여 복호화하는 과정을 ipynb으로 공유한다던지 활용 가능성이 높다.
        * `%notebook filepath`를 통하여 ipynb로 output 가능

* <a href="https://github.com/james91b/ida_ipython">ida_ipython</a>
    * 위 프로젝트와 동일한 기능을 가지고 있다.

* <a href="https://github.com/techbliss/Python_editor">Python_editor</a>
    * 세팅을 추가적으로 해야하는데 귀찮아서 넘어감

* <a href="https://github.com/0xeb/ida-qscripts">ida-qscripts</a>
    * <a href="https://github.com/0xeb/ida-qscripts/releases">releases</a> 사이트에서 컴파일된 dll 파일을 Plugins 폴더에 넣는다.
    * QScripts 플러그인을 실행한 후에 사용할 script 파일을 지정한다. (지정된 파일은 bold 처리가 되며, 아이콘도 바뀜)
    * 저장을 할 때마다 결과 값이 IDA에서 보여지며, IDAPython 또한 사용 가능하다.
    * 비활성화의 경우, CTRL+D 또는 오른쪽 클릭 후 Deactivate script monitor 선택
    * CTRL+E를 통하여 옵션 제공 (실행 시 결과창 clear, 모니터링 인터벌 간격 등)

* <a href="https://github.com/patois/IDAPyHelper">IDAPyHelper</a>
    * idapyhelper.py를 Plugins 폴더에 넣는다.
    * IDA에서 사용 가능한 모든 라이브러리, 함수들을 확인할 수 있다.
    * IDA를 실행하면 자동으로 오른쪽 하단에 실행되는데 그 이유는 PluginForm으로 핫키를 통하여 실행되지 않고, 바로 Show() 함수가 실행된다.

* <a href="https://github.com/arizvisa/ida-minsc">ida-minsc</a>
    * IDAPython을 쉽게 사용할 수 있다고 한다. 현재 내게 필요한 플러그인은 아닌 것 같아서 일단 패스했다. ㅎㅎ
    * (<a href="https://github.com/tmr232/Sark">Sark</a>, <a href="https://github.com/synacktiv/bip/">bip</a>)

* <a href="https://github.com/ioncodes/idacode">idacode</a>
    * Visual Studio Code에서 IDAPython을 사용할 수 있다고 한다.
    * github repo에 있는 marketplace 링크에서 vscode 플러그인을 설치한다.
    * ``idacode_utils/settings.py``에서 환경설정 후 Plugins 폴더에 넣는다.
    * VSCode에서 `Ctrl+Shift+P`를 눌러서 `IDACODE: Connect to IDA`를 선택하면 저장될 때마다 IDA에서 실행된다. - workspace folder를 지정하지 않은 상태에서 종료하면 disconnected (ida-qscripts와 유사한 컨셉)
    * 그리고 VSCode의 좋은 기능 중 하나인 디버깅 기능도 idacode 플러그인을 통하여 사용할 수 있다.

* <a href="https://github.com/a1ext/labeless">labeless</a>
    * ida에서 구한 값을 __extern__으로 넘겨서 ollydbg, x64dbg 파이썬 기능으로 작업할 수 있다.

* <a href="https://github.com/Jinmo/ifred">ifred</a>
    * 해당 주제와는 조금 다른 플러그인이지만 적어봤다.
    * IDA에서 제공하는 모든 기능이 모여있으며, 사용이 가능하다. `Ctrl+Shift+P`
    * IDA에서 정의한 모든 변수 및 함수에 접근이 가능하다. `Ctrl+P`
    * 검색 기능을 통하여 원하는 데이터, 기능에 빠르게 접근이 가능하다.

* Plugin Manager
    * https://github.com/deactivated/idaenv
    * https://github.com/Jinmo/idapkg

* Other
    * https://github.com/onethawt/idaplugins-list

* http://cutter.re/
    * python 3.6 버전이 내장되어 있음
    * ``python -m pip install -I -t C:\Cutter-v1.12.0-x64.Windows\python36\site-packages jupyter``
    * <a href="https://github.com/rizinorg/cutter-jupyter">cutter-jupyter</a>에서 cutter 플러그인 경로에 넣으면 끝
