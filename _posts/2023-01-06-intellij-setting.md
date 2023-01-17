---
layout: post
title: "Intellij 세팅"
description: ""
date: 2023-01-06
tags: ["intellij", "setting"]
---

인텔리제이를 사용하다보면 필요 없는 기능들이 기본으로 활성화되어 있고, 불편한 점이 꽤 있다.

## 코드 스타일 변경하기

`Settings -> Editor -> Code Style -> Java -> Scheme 변경` <a href="https://github.com/google/styleguide/blob/gh-pages/intellij-java-google-style.xml">intellij-java-google-style.xml</a>

## 저장할 때 마지막 라인 자동으로 개행하기

`Settings -> Editor -> General -> On Save -> Ensure every saved file ends with a line break` 체크

마지막 라인 개행을 해주지 않으면 github action에서 걸리기도 하고, 지키라고 하니 해야지..

## 필요없는 객체 관련 팝업 삭제하기

클래스 만들 때마다 `no usages, 3 usages, 1 inheritor` 등등 뜨면서 라인을 하나씩 더 잡아먹는다..

굳이 알려주지 않아도 사용하지 않은 메서드, 클래스는 진한 회색으로 바뀌고 코드 왼쪽에 상속하고 있는 인터페이스 등등 다 알려주는데 굳이 필요가 있나 싶다.

`Settings -> Editor -> Inlay Hints -> Code vision -> Inheritors, Usages 체크 해제`

## 공백 문자 표시하기

`Settings -> Editor -> General -> Apperance -> Show whitespaces` 체크

## 코드 축약해주는 기능 끄기

`Code Folding`이라고 불리는 기능인데 뭔가 불편하다. import한 라이브러리들도 숨기고 괄호도 한 줄로 정리해서 보여주는데 딱히 필요가 없어 보인다.

`Settings -> Editor -> General -> Code Folding -> 필요없는 옵션들 체크 해제`

일단 나는 `General -> Imports`, `Java -> One-line methods`를 해제해서 사용하고 있다.

## 인코딩 설정 (한글 사용)

`Settings -> Editor -> File Encodings -> Transparent native-to-ascii conversion 체크, Default encoding for properties files: UTF-8 변경, 위에 인코딩 설정들 UTF-8로 변경`

`Help -> Edit Custom VM Options`에서 인코딩 설정

```text
-Dfile.encoding=UTF-8
-Dconsole.encoding=UTF-8
```

## Gradle 설정 변경하기

`Settings -> Build, Execution, Deployment -> Build Tools -> Gradle -> Build and run using, Run tests using에서 IntelliJ IDEA로 변경`

<a href="https://jojoldu.tistory.com/450">향로님 블로그</a>
 
## 단축키 보여주는 플러그인

<a href="https://plugins.jetbrains.com/plugin/7345-presentation-assistant/">Presentation Assistant</a>

인프런 김영한님 강의 외에도 유튜브 강의를 보면 단축키를 하단에 자동으로 보여주는데 위 플러그인을 사용한 것이다. 

설치하고 써보니 사이즈와 보여주는 시간이 은근 길었다.

`Settings -> Appearance & Behavior -> Presentation Assistant -> Font size 16으로 변경, Display duration (in ms) 3000으로 변경`

## 코드 미니맵 보여주는 플러그인

<a href="https://plugins.jetbrains.com/plugin/18824-codeglance-pro">CodeGlance Pro</a>

현재 <a href="https://github.com/vektah/CodeGlance">CodeGlance</a>는 유지보수가 안되고 있어서 다른 사람이 fork 떠서 하고 있다고 한다. (<a href="https://github.com/Nasller/CodeGlancePro">CodeGlancePro</a> - 현재 인텔리제이 플러그인에 검색하면 CodeGlance는 안뜬다.)

`CTRL + SHIFT + G` 단축키로 Hide/Unhide가 가능하다. 이 기능은 vscode에 기본적으로 있는데, 인텔리제이에서는 플러그인을 쓰면 된다.

