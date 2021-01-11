---
layout: post
title: "바이너리 분석 도구 Plugin 개발에 도움을 주는 것들"
description: ""
date: 2021-01-10
tags: [IDA, IDAPython, cutter, binaryninja, Plugin]
---

IDAPython 사용에 도움을 주는 플러그인을 직접 사용해보고 정리하였다.

IDA를 통해 설치되는 python의 pip 버전이 매우 낮으므로 업그레이드를 해야한다. (구버전)

``C:\\python27-x64\\python.exe -m pip install --upgrade pip``

IDA 7.5 기준 PC에 설치된 python3 경로를 지정하여 사용할 수 있으며, 위 과정은 생략해도 된다.

* IDAPython cheatsheet
    * https://github.com/inforion/idapython-cheatsheet
    * https://github.com/Inndy/idapython-cheatsheet
    * https://gist.github.com/icecr4ck/7a7af3277787c794c66965517199fc9c
    * https://gist.github.com/icecr4ck/9dea9d1de052f0b2b417abf0046cc0f6
    * https://gist.github.com/icecr4ck/6c744d489efbb07a32bb22e8a3c748e3
    * https://gist.github.com/icecr4ck/ec39ddedf3f1948fdf7873094561739a

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

* <a href="https://github.com/idapython/src">idapython-src</a>
    * idapython 구현 코드를 볼 수 있으며, example 코드도 다양하게 있다.

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


<a href="http://cutter.re/">cutter</a>는 IDA와 다르게 오픈소스이며, radare2를 활용한다.

python 3.6.8 버전이 python36.dll로 내장되어 있어서 동일한 python 3.6을 PC에 설치 후 아래 명령어를 사용하여 설치

``python -m pip install -I -t C:\Cutter-v1.12.0-x64.Windows\python36\site-packages jupyter``

* <a href="https://github.com/rizinorg/cutter-jupyter">cutter-jupyter</a>
    * 위 플러그인을 설치하여 cutter에서 jupyter를 사용할 수 있다. (위와 같이 jupyter 설치 후 플러그인 경로에 파일 넣기)

* <a href="https://cutter.re/docs/plugins.html">cutter docs</a>
    * Plugin 개발 관련 문서가 잘 정리되어 있음

* <a href="https://github.com/radareorg/cutter-plugins">cutter-plugins</a>
    * cutter plugin 정리

<a href="https://binary.ninja/">BinaryNinja</a>도 위 도구들과 같은 유형의 분석 도구임

* https://api.binary.ninja/
* https://github.com/Vector35/binaryninja-api
* https://github.com/Vector35/sample_plugin
* https://github.com/Vector35/community-plugins
* https://github.com/Vector35/official-plugins