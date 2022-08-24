---
layout: post
title: "VSCode Vim에서 불편한 한글 문제"
description: ""
date: 2022-08-24
tags: ["vscode","vim"]
---

WSL에서 vim을 사용할 때는 한글로 변환된 상태에서도 ESC 키를 누르면 자동으로 영어로 변환되어서 키가 씹히는 일이 없었는데,

VSCode에서 Vim Extension을 사용할 때 자동 변환을 해주지 못하다보니, 수동으로 한영 전환을 해야해서 매우 불편하였다.

Vim Extension 문서를 읽어보니 AutoSwitch 기능을 활용하여 해결이 가능하다고 한다.

ESC 키를 누르면 영어로 변환하고 편집 모드로 접근하면 자동으로 이전에 사용했었던 언어로 변환해준다고 한다.

근데 이게 문제가 존재가 존재하는데, 윈도우 US 키보드를 설치해야하며 변환되는 과정이 매끄럽지 않았다. (윈도우 US 키보드에 머물고 있으면 한글 변환이 안되므로, 수동으로 한글 키보드를 선택해야함)

Vim Extension에서 지원하는 vimrc 기능은 기본적인 set 설정도 인식하지 못했다. 

구글링을 해보니 MacOS는 hammerspoon, Windows는 AutoHotKey를 활용하여 해결하고 있었다.

나는 AutoHotKey를 사용하였고, 이번 기회에 AutoHotKey를 공부해보고 싶다는 생각이 들었다.

다른 사람들이 hammerspoon으로 구현한 기능들을 AutoHotKey에서 구현할 수 있는지 고민해보고 어떤 한계가 있는지도 알아보려고 한다.

참고사이트

<a href="https://johnny-mh.github.io/post/Windows%EC%97%90%EC%84%9C-VS-Code-vim%ED%94%8C%EB%9F%AC%EA%B7%B8%EC%9D%B8-%EC%9E%90%EB%8F%99-%ED%95%9C%EC%98%81%EC%A0%84%ED%99%98/">Windows에서 VS Code vim플러그인 자동 한영전환</a>

<a href="https://johngrib.github.io/blog/2017/05/04/input-source/">Vim 사용시 한/영 전환 문제 해결하기</a>

<a href="https://johngrib.github.io/wiki/hammerspoon/">johngrib 블로그 - hammerspoon tags</a>

<a href="https://github.com/johngrib/simple_vim_guide/blob/master/md/with_korean.md">johngrib - simple_vim_guide</a>

<a href="https://twitter.com/John_Grib/status/1534909491581308929">johngrib 트위터 - hammerspoon</a>