---
layout: post
title: "Registry Explorer Plugin 개발"
description: ""
date: 2023-05-28
tags: [Forensic, Registry, EZ-Tools]
---

http://ericzimmerman.github.io/ 에서 RegistryExplorer 다운로드 후 RegistryExplorerManual.pdf 참고

<a href="https://github.com/EricZimmerman/RegistryPlugins">RegistryPlugins</a>에서 repo 다운로드 후 sln 파일을 open한다.

새 프로젝트의 이름을 `RegistryPlugin.Name` 형식으로 만든다.

데이터를 출력하는 cs파일과 Registry 데이터 추출에 사용되는 cs파일 2개를 만들어야 한다.

manual에서는 데이터 출력에 사용되는 파일을 ValuesOut.cs으로 사용하는데, github에 있는 파일명들을 보면 꼭 그렇진 않음.

아무튼 ValuesOut.cs는 IValueOut, 추출 파일은 IRegistryPluginGrid 인터페이스를 구현하면 된다.

![Registry-Explorer](/assets/images/RegistryExplorer-Plugin/0.png)

그리고 위 사진과 같이 Registry와 RegistryPluginBase를 import 해야하는데,

빌드 후 참조에서 dll 파일을 추가하면 된다. 

![Registry-Explorer](/assets/images/RegistryExplorer-Plugin/3.png)

또는 RegistryPluginBase를 nuget에서 설치

![Registry-Explorer](/assets/images/RegistryExplorer-Plugin/1.png)

ValuesOut.cs에서 생성자로 초기화한 변수명들이 테이블명으로 지정된다.

![Registry-Explorer](/assets/images/RegistryExplorer-Plugin/2.png)

테이블명에서 `Serial Number`가 중간에 띄어쓰기 되어있는데 변수명의 대문자를 기준으로 띄어쓰기가 되는 것 같다.

그리고 IRegistryPluginGrid 인터페이스에 있는 ProcessValues의 return 값이 테이블의 데이터로 들어간다.

return 값은 ``var l = new List<ValuesOut>();``으로 생성된 객체 (for loop를 돌면서 ``l.Add(ff)``) 

![Registry-Explorer](/assets/images/RegistryExplorer-Plugin/4.png)

최근에 플러그인을 개발하다가 알게 된건데 같은 경로에 여러 개의 플러그인을 사용할 수 있다.

위 사진은 이전에 만들어진 DHCPNetworkHints (Win7에서 사용되는 Artifact로 추정) 

그리고 이번에 내가 만들은 NetworkSettings 플러그인이다. 

사진을 보면 2개의 플러그인이 모두 활성화된 것을 볼 수 있다.
