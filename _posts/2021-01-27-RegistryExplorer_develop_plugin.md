---
layout: post
title: "Registry Explorer Plugin 개발"
description: ""
date: 2021-01-27
tags: [Forensic, Registry, EZ-Tools]
---

http://ericzimmerman.github.io/ 에서 RegistryExplorer 다운로드 후 RegistryExplorerManual.pdf 참고

<a href="https://github.com/EricZimmerman/RegistryPlugins">RegistryPlugins</a>에서 repo 다운로드 후 sln 파일을 open한다.

새 프로젝트의 이름을 `RegistryPlugin.Name` 형식으로 만든다.

데이터를 출력하는 cs파일과 Registry 데이터 추출에 사용되는 cs파일 2개를 만들어야 한다.

보고서에서는 데이터 출력에 사용되는 파일을 ValuesOut.cs으로 사용하는데, github에 있는 파일명들을 보면 꼭 그렇진 않음.

아무튼 ValuesOut.cs는 IValueOut, 추출 파일은 IRegistryPluginGrid 인터페이스를 구현하면 된다.

![Registry-Explorer](/assets/images/RegistryExplorer-Plugin/0.png)

그리고 위 사진과 같이 Registry와 RegistryPluginBase를 import 해야하는데,

참조에서 dll 파일을 추가하면 된다. (Registry는 없는 경우, nuget에서 설치)

![Registry-Explorer](/assets/images/RegistryExplorer-Plugin/1.png)

ValuesOut.cs에서 생성자로 초기화한 변수들이 테이블명으로 지정된다.

![Registry-Explorer](/assets/images/RegistryExplorer-Plugin/2.png)

테이블명에서 `Serial Number`가 중간에 띄어쓰기 되어있는데 변수명의 대문자를 기준으로 띄어쓰기가 되는 것 같다.

그리고 IRegistryPluginGrid 인터페이스에 있는 ProcessValues의 return 값이 테이블의 데이터로 들어간다.