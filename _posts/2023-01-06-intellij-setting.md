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

`Settings -> Editor -> Code Style -> Java -> Wrapping and Braces -> Chained method calls: Wrap if long -> Wrap always` 

프로젝트에서 요구하는 코드 스타일을 참고하여 커스터마이징하면 되겠다.

## 저장할 때 마지막 라인 자동으로 개행하기 (No newline at end of file)

`Settings -> Editor -> General -> On Save -> Ensure every saved file ends with a line break` 체크

<a href="https://velog.io/@doondoony/posix-eol">파일 끝에는 항상 개행을 추가해야 해요</a>, <a href="https://blog.coderifleman.com/2015/04/04/text-files-end-with-a-newline/">파일 끝에 개행을 추가해야 하는 이유</a>

POSIX 표준에 의하면 마지막 행은 개행으로 끝나야 한다. 자세한 내용은 위 글을 참고하자.

## line separator 설정하기

`Settings -> Editor -> Code Style -> Line separator: Unix and macOS (\n) 설정`

<a href="https://www.jetbrains.com/help/idea/configuring-line-endings-and-line-separators.html">jetbrains 튜토리얼</a>

팀으로 개발할 때 Windows, Mac, Linux 등 다양한 OS를 사용하는데 OS마다 개행을 처리하는 방법이 다르다.

윈도우는 CRLF(\r\n), Unix/Linux는 LF(\n)으로 되어있다. 보통 LF로 통일하여 사용하는 것 같다. (큰 프로젝트들은 LF로 사용하고 있다.)

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

## visual guides 지우기

`Settings -> Editor -> General -> Appearance -> Show hard wrap and visual guides 체크 해제`

위 옵션이 체크되어 있으면 코드 오른쪽에 가이드 줄이 생기는데, 딱히 필요가 없어서 사용하지 않고 있다.

## Project 트리 구조 변경하기

`Project -> Options -> Tree Appearance -> Flatten Packages, Compact Middle Packages 체크 해제`

패키지가 많을 때 편리한 기능이겠지만, 개인적으로는 불편해서 사용하지 않고 있다.

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

## snippet 설정하기

`Settings -> Editor -> Live Templates`에서 자유롭게 추가

최근에 test snippet을 만들어서 사용 중인데 매우 편리하다.

*Template text*

```java
@org.junit.jupiter.api.Test
@org.junit.jupiter.api.DisplayName("$TEST_NAME$")
public void $METHOD_NAME$() {
  //given
  $END$
  //when
  
  //then
  org.assertj.core.api.Assertions.assertThat(true);
}
```

`Abbreviation: test`, `Description: java test` (대충 적음)

`Edit Variables` 버튼 클릭 후 아래 사진과 같이 설정

METHOD_NAME의 Expression는 아래 코드를 사용하면 된다.

`regularExpression(spacesToUnderscores(TEST_NAME), "[.]", "")`

![1](/assets/images/intellij-setting/01.png)

마지막으로 Options에서 `Use static import if possible`, `Shorten FQ names` 체크

첫 번째 옵션을 활성화하면 `Assertions.assertThat`을 static import하여 `assertThat`으로 사용할 수 있다.

두 번째 옵션을 활성화하면 템플릿에 적혀있는 경로의 라이브러리들을 import 해준다.

`assertThat`에 `true`를 넣은 이유는 에러를 뜨지 않게 하기 위함이다. `F2` 단축키로 에러를 처리할 때 `assertThat` 때문에 1번 더 누르는게 귀찮다.

`test`를 입력하면 위 템플릿으로 만들어주며, 아래 사진과 같이 space를 눌렀을 때 underscore로 변환해준다.

그리고 정규표현식을 사용하여 점을 빈 값으로 replace 하였다.

`@DisplayName`에 점을 포함하는 문장을 적었을 때 메서드명에는 점을 사용할 수 없기 때문이다.

![2](/assets/images/intellij-setting/02.png)

<a href="https://www.jetbrains.com/help/idea/template-variables.html">Jetbrains 튜토리얼</a>

<a href="https://github.com/mraible/idea-live-templates">idea-live-templates</a> 

## ligatures 설정하기

`Settings -> Editor -> Font -> Enable ligatures 체크`

## Download pre-built shared indexes

https://whitekeyboard.tistory.com/874

## New UI

`Settings -> Appearance & Behavior -> New UI -> Enable new UI`

jetbrains 도구에서 지원하는 새로운 UI인데 전체적으로 깔끔하고, 화면에 필요한 기능만 보여주는 것 같아서 사용하고 있다.

## Dark Purple Theme

<a href="https://plugins.jetbrains.com/plugin/12100-dark-purple-theme">Dark Purple Theme</a>

jetbrains 기본 테마인 Darcula만 사용하다가 다른 테마도 다운받아서 사용해보고 있는데, 꽤 괜찮다.

## Atom Material Icons

<a href="https://plugins.jetbrains.com/plugin/10044-atom-material-icons">Atom Material Icons</a>

아이콘 관련 플러그인들을 중에서 다운로드 수를 보면 압도적으로 많다. 나는 일단 New UI에서 제공하는 기본 아이콘을 사용하고 있는데, Atom 아이콘은 뭔가 정신 사납다는 느낌이 들었다 ㅎ.. 기본 아이콘이 깔끔하고, 개인적으로 더 좋은 것 같다.

## 단축키 보여주는 플러그인

<a href="https://plugins.jetbrains.com/plugin/7345-presentation-assistant/">Presentation Assistant</a>

인프런 김영한님 강의 외에도 유튜브 강의를 보면 단축키를 하단에 자동으로 보여주는데 위 플러그인을 사용한 것이다. 

설치하고 써보니 사이즈와 보여주는 시간이 은근 길었다.

`Settings -> Appearance & Behavior -> Presentation Assistant -> Font size 16으로 변경, Display duration (in ms) 3000으로 변경`

## 코드 미니맵 보여주는 플러그인

<a href="https://plugins.jetbrains.com/plugin/18824-codeglance-pro">CodeGlance Pro</a>

현재 <a href="https://github.com/vektah/CodeGlance">CodeGlance</a>는 유지보수가 안되고 있어서 다른 사람이 fork 떠서 하고 있다고 한다. (<a href="https://github.com/Nasller/CodeGlancePro">CodeGlancePro</a> - 현재 인텔리제이 플러그인에 검색하면 CodeGlance는 안뜬다.)

`CTRL + SHIFT + G` 단축키로 Hide/Unhide가 가능하다. 이 기능은 vscode에 기본적으로 있는데, 인텔리제이에서는 플러그인을 쓰면 된다.

