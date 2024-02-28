---
layout: post
title: "Registry Explorer Plugin 개발"
description: ""
date: 2024-02-27
tags: [Forensic, Registry, EZ-Tools]
---

우선 플러그인을 개발하기 전에 http://ericzimmerman.github.io/ 에서 RegistryExplorer 다운로드 후 압축파일에 있는 `RegistryExplorerManual.pdf`를 받아두자. 플러그인 개발과 관련된 내용이 있다.

## 환경 세팅

<a href="https://github.com/EricZimmerman/RegistryPlugins">RegistryPlugins</a>에서 repo 다운로드 후 `sln` 파일을 실행한다.

프로젝트에서 사용하는 라이브러리들이 존재하는데, 버전이 같지 않다는 warning 또는 패키지가 없다는 오류가 발생한다.

![Registry-Explorer](/assets/images/RegistryExplorer-Plugin/5.png)

나는 <a href="https://www.nuget.org/packages/">nuget</a>에서 패키지를 수동으로 다운로드 받아서 해결했다.

`도구` -> `NuGet 패키지 관리자` -> `패키지 관리자 설정` -> `패키지 소스`를 보면 사용 가능한 패키지들이 위치한 경로를 확인할 수 있는데, 다운로드한 패키지 파일들이 해당 경로에 위치하면 된다.

![Registry-Explorer](/assets/images/RegistryExplorer-Plugin/6.png)

![Registry-Explorer](/assets/images/RegistryExplorer-Plugin/7.png)

패키지 소스 경로에 파일들을 준비했다면 플러그인을 빌드했을 때 `dll` 파일이 정상적으로 생성된다.

![Registry-Explorer](/assets/images/RegistryExplorer-Plugin/8.png)

## 프로젝트 생성

`dll` 파일을 만들어야 하기 때문에 `클래스 라이브러리` 선택 -> 새 프로젝트의 이름을 `RegistryPlugin.Name` 형식으로 만든다.

![Registry-Explorer](/assets/images/RegistryExplorer-Plugin/9.png)

플러그인 프로젝트에 존재하는 `csproj` 파일을 보면 `netstandard2.0`을 사용하고 있으니, 알맞게 선택하자.

![Registry-Explorer](/assets/images/RegistryExplorer-Plugin/10.png)

데이터를 출력하는 cs파일과 Registry 데이터 추출에 사용되는 cs파일 2개를 만들어야 한다.

manual에서는 데이터 출력에 사용되는 파일을 `ValuesOut.cs`으로 사용하는데, github에 있는 파일명들을 보면 꼭 그렇진 않은 것 같다.

아무튼 `ValuesOut.cs`는 `IValueOut`, 추출 파일은 `IRegistryPluginGrid` 인터페이스를 구현하면 되며, 다른 플러그인들의 코드를 참고해서 개발하면 된다. 물론 필요한 라이브러리들을 import해서 개발할 수 있다.

![Registry-Explorer](/assets/images/RegistryExplorer-Plugin/11.png)

개발하기 전에 종속성 패키지에 `RegistryPluginBase`가 없는 것을 확인할 수 있다.

환경 설정 파트에서 `nupkg` 파일들을 준비했다면 `프로젝트 오른쪽 클릭` -> `NuGet 패키지 관리` -> 필요한 패키지들을 설치하면 된다.

![Registry-Explorer](/assets/images/RegistryExplorer-Plugin/13.png)

다른 패키지가 추가로 필요하지 않는 이상 대부분의 경우 `RegistryPluginBase`만 설치하면 된다.

![Registry-Explorer](/assets/images/RegistryExplorer-Plugin/12.png)

이제 빨간 밑줄이 뜨지 않는다. `IRegistryPluginGrid`에 있는 밑줄은 인터페이스에서 요구하는 메서드들을 구현하지 않았기 때문이다. 이제 개발을 시작하면 된다.

## 동작 방식

![Registry-Explorer](/assets/images/RegistryExplorer-Plugin/1.png)

`ValuesOut.cs`에서 생성자로 초기화한 변수명들이 테이블명으로 지정된다.

![Registry-Explorer](/assets/images/RegistryExplorer-Plugin/2.png)

테이블명에서 `Serial Number`가 중간에 띄어쓰기 되어있는데 변수명의 대문자를 기준으로 띄어쓰기가 되는 것 같다.

그리고 `IRegistryPluginGrid` 인터페이스에 있는 `ProcessValues`의 `return` 값이 테이블의 데이터로 들어간다.

`var l = new List<ValuesOut>();`으로 생성된 객체에 for loop를 돌면서 `l.Add(ff)`에 의해 추가되며, `l` 변수를 반환한다.

![Registry-Explorer](/assets/images/RegistryExplorer-Plugin/4.png)

최근에 플러그인을 개발하다가 알게 된건데 같은 경로에 여러 개의 플러그인을 사용할 수 있다.

위 사진은 이전에 만들어진 `DHCPNetworkHints` (Win7에서 사용되는 Artifact로 추정) 

그리고 이번에 내가 만든 `NetworkSettings` 플러그인이다. 

사진을 보면 2개의 플러그인이 모두 활성화된 것을 볼 수 있다.
