---
layout: post
title: "RegRipper Plugin 개발"
description: ""
date: 2020-12-01
tags: [Forensic, Plugin, RegRipper]
---

<a href="https://youtu.be/rkzXyqa5e1g">Effectively Using RegRipper 3.0, Harlan Carvey, OSDFCon 2020</a>

* timestamp 표시 방법이 바뀜 (rfc3339)
* regripper 2.8 플러그인은 3.0에서 동작한다.
* 레지스트리 트랜잭션 로그 분석의 경우 yarp + registryFlush.py 또는 rla.exe 사용
* GUI 버전에서 하이브를 자동으로 인식하여 분석이 진행된다.
* 유의미한 모든 정보를 추출하지 않는다. -> 분석 목표를 정하고 사용해야한다. -> 나중에 해당 데이터를 추출해주는 플러그인 개발

RegRipper는 레지스트리의 다양한 곳에 존재하는 유의미한 데이터들을 추출하는 도구이다.

레지스트리에는 프로그램을 통하여 open 했던 기록이 저장되어 있는데

이 도구는 외국에서 만들었기 때문에 국내에서 주로 사용되는 프로그램의 플러그인은 존재하지 않는다. ~~(당연..)~~

그래서 국산 프로그램들의 레지스트리 경로를 찾아보고, 플러그인을 개발하였다.

국내에서 주로 사용되는 프로그램 목록은 여기서 찾을 수 있다.

https://software.naver.com/software/softwareList.nhn

그리고 만들어지지 않은 외국 프로그램도 일부 추가하였다.

![regripper](/assets/images/regripper/1.png)

* 반디집
    - `HKEY_CURRENT_USER\SOFTWARE\Bandizip`
    - RecentArchive{number} 형태로 최근 실행 압축 파일 존재 -> 10개의 최근 기록을 저장하며(0~9), 최근 실행 파일이 위(0)로 간다
    - RecentFolder{number} 형태로 최근 접속 폴더 존재 - 압축 해제된 경로인지 테스트해야할듯 - 아직 모르겠다

* 빵집
    - `HKEY_CURRENT_USER\SOFTWARE\BreadZip\4.0\Reopen`
    - 0000~0009 까지 10개의 최근 기록을 저장하며, 최근 실행 파일이 위(0000)로 간다.

* 알집
    - `HKEY_CURRENT_USER\SOFTWARE\ESTsoft\ALZip\MRUOpen`

* 알씨
    - `HKEY_CURRENT_USER\SOFTWARE\ESTsoft\ALSee` - X

* 알pdf
    - `HKEY_CURRENT_USER\SOFTWARE\ESTsoft\ALPDF` - X

* 알캡처
    - `HKEY_CURRENT_USER\SOFTWARE\ESTsoft\ALCapture\Config` - FilePath에서 마지막 저장 경로만 존재

* 알송
    - `HKEY_CURRENT_USER\SOFTWARE\ESTsoft\ALSong` LastFileDir에 가장 최근 실행 경로 존재

* 한글
    - `2005 - HKEY_CURRENT_USER\SOFTWARE\HNC\Hwp\6.5\RecentFile` - file{number} 형태로 저장 (utf-16)
    - `2007 - HKEY_CURRENT_USER\SOFTWARE\HNC\Hwp\7.0\HwpFrame\RecentFile` - file{number} 형태로 저장 (utf-16)
    - `2010 - HKEY_CURRENT_USER\SOFTWARE\HNC\Hwp\8.0\HwpFrame\RecentFile` - file{number} 형태로 저장 (utf-16)
    - `HKEY_CURRENT_USER\SOFTWARE\HNC\Hwp\FindReplace\Find 찾기/바꾸기 목록` - 2005, 2007, 2010에서 동작하며 2014부터 추가되지 않는다.
    - `2018 - HKEY_CURRENT_USER\SOFTWARE\HNC\Hwp\10.0\HwpFrame` - X
    - `2020 - HKEY_CURRENT_USER\SOFTWARE\HNC\Hwp\11.0\HwpFrame` - X
    
* 한글 viewer
    - `HKEY_CURRENT_USER\SOFTWARE\HNC\Hwp\9.6`

* 팟플레이어
    - `HKEY_CURRENT_USER\SOFTWARE\DAUM\PotPlayer` - X
    - `HKEY_CURRENT_USER\SOFTWARE\DAUM\PotPlayer64` - X

* 곰플레이어
    - `HKEY_CURRENT_USER\SOFTWARE\GRETECH\GOMPLAYER` - X

* 곰캠
    - `HKEY_CURRENT_USER\SOFTWARE\GOM\GOMCAM` - X

* 곰오디오
    - `HKEY_CURRENT_USER\SOFTWARE\GRETECH\GOMAudio` - X

* 곰녹음기
    - `HKEY_CURRENT_USER\SOFTWARE\GRETECH\GOMRecorder` - X

* 곰인코더
    - `HKEY_CURRENT_USER\SOFTWARE\GRETECH\GOMEncoder64` - X

* kmplayer x64
    - `HKEY_CURRENT_USER\SOFTWARE\KMPlayer 64X\KMPlayer 64X\Recent File List` - File{number} 형태로 존재

* kmplayer
    - `HKEY_CURRENT_USER\SOFTWARE\KMPlayer\KMP3.0` - LastFileName, LastFileTitle로만 남아있음

* 카카오톡
    - `HKEY_CURRENT_USER\SOFTWARE\Kakao\KakaoTalk\Update` - 가장 최근 업데이트 시간이 남아있음
    - 적어도 이 시간 이후에 카톡에 접근했다는 추정 가능..?
    - `HKEY_CURRENT_USER\SOFTWARE\Kakao\KakaoTalk\UserAccounts` - 접속 계정 이름 존재

* ocam
    - X

* ezpdf
    - `HKEY_CURRENT_USER\SOFTWARE\UNIDOCS\ezPDF Editor3.0\Recent File List`
    - `HKEY_CURRENT_USER\SOFTWARE\UNIDOCS\ezPDF Editor3.0\OpenFileList`

* polaris office
    - `HKEY_CURRENT_USER\SOFTWARE\Infraware\PolarisOffice` - X

* libre office
    - X

* Enterprice Architect
    - `HKEY_CURRENT_USER\SOFTWARE\Sparx Systems\EA400\EA\RecentFiles`

* sqlitebrowser
    - `HKEY_CURRENT_USER\SOFTWARE\sqlitebrowser\sqlitebrowser\General\RecentFileList`

* foxit reader
    - `HKEY_CURRENT_USER\SOFTWARE\Foxit Software\Foxit Reader 10.0\MRU\File MRU`
    - `HKEY_CURRENT_USER\SOFTWARE\Foxit Software\Foxit Reader 10.0\MRU\Place MRU`
    - `HKEY_CURRENT_USER\SOFTWARE\Foxit Software\Foxit Reader 10.0\CommentPanel\Filter\Record {number}\File`
    - https://github.com/forensenellanebbia/My-RegRipper-plugins/blob/master/foxitrdr.pl

* 꿀뷰
    - `HKEY_CURRENT_USER\SOFTWARE\Honeyview` - RecentFolder{number} 0~9
    - 반디집 소프트웨어를 만든 회사인데 반디집에는 파일 정보가 있지만, 꿀뷰에는 남아있지 않음

* photoscape
    - `HKEY_CURRENT_USER\SOFTWARE\Mooii\PhotoScape` - X

* 고클린
    - `HKEY_CURRENT_USER\SOFTWARE\GoClean\GoClean\GoClean` - RecentExecTime

* notepad
    - `HKEY_CURRENT_USER\SOFTWARE\Microsoft\Notepad` - 가장 최근 검색 기록 존재, replace 문자 존재

* 링크
    * https://github.com/keydet89/RegRipper3.0
    * https://github.com/msuhanov/yarp
    * https://github.com/Silv3rHorn/4n6_misc/blob/master/registryFlush.py
    * https://github.com/forensenellanebbia/My-RegRipper-plugins
    * https://github.com/randomaccess3/Regripper-Plugins
    * https://github.com/Silv3rHorn/autoripy
    * https://github.com/mkorman90/regipy
    * https://github.com/airbus-cert/regrippy
    * http://cheeky4n6monkey.blogspot.com/2012/02/writing-ccleaner-regripper-plugin-part.html
    * http://cheeky4n6monkey.blogspot.com/2012/02/writing-ccleaner-regripper-plugin-part_05.html