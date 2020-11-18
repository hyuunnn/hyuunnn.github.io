---
layout: post
title: "Windows Registry Forensics 책 후기"
description: "Windows Registry Forensics - harlan carvey"
date: 2020-11-18
tags: [Forensic, Book]
---

다양한 연구 사례, 저자가 개발한 도구 외에도 다양한 도구들을 알려주면서 레지스트리를 어떻게 분석해야하는지 알려주고 있다.

레지스트리 구조부터 시작하여 관련 자료, 각각의 레지스트리 파일에 어떤 유용한 데이터들이 존재하는지 알려준다.

그리고 저자가 만든 도구인 RegRipper의 각 플러그인에 대한 결과값과 왜 만들었는지, 언제 사용하면 좋은지 등 설명해주고 있다.

RegRipper 사용 시 한글이 깨질 때 ```chcp 65001``` 해주면 된다.

책을 끝까지 읽어본 결과 번역이 제대로 되어있지 않아서 잘 읽히지 않거나, 이해가 안되는 부분이 있어서 많이 아쉬웠다.

RegRipper 플러그인 중에서 tln이 붙은 파일은 tsk 결과에 맞게 만들어준다.

* SECURITY
    * 정책 설정 관련 데이터 -> 악성 프로그램을 통하여 바뀐 정책의 시간 전후를 확인 `secrets.pl, secrets_tln.pl`
    * SID 정보 `polacdms.pl`
    * auditpol 정보 `auditpol.pl`

* SAM
    * 사용자 정보 `samparse.pl, samparse_tln.pl`

* SYSTEM
    * 네트워크 정보 `nic2.pl`
    * 프리패치 설정 정보 확인 `prefetch.pl`
    * svchost를 통하여 로드되는 서비스 확인 `svcdll.pl`
    * AppCompatCache `appcompatcache.pl, appcompatcache_tln.pl`
    * 악성코드 탐지 `fileless.pl, routes.pl`
    * <a href="https://www.secureworks.com/blog/how-to-hide-malware-in-unicode">rlo</a> 문자를 사용한 악성코드 탐지 목적 `rlo.pl`
    * 특정 크기 이상의 데이터를 가진 값 검색 `sizes.pl`
    * USB 연결 정보 `usbstor.pl, usbdevices.pl mountdev.pl(MountedDevices)`
        * USB 마지막 연결 시간은 ControlSet00n\Enum\USBSTOR 키의 LastWrite, SOFTWARE의 MountPoints2를 활용할 수 있다고 한다.

* SOFTWARE
    * 네트워크 어뎁터 정보 `networkcards.pl`
    * ssid 정보 (윈도우 비스타 이전 버전) `ssid.pl` (비스타 이상 버전) `networklist.pl`
    * 다양한 Run 정보 출력 `run.pl`
    * Image File Execution Options `imagefile.pl`
    * AppInit_DLLs `appinitdlls.pl`
    * KnownDlls와 같이 dll 지속성 조사에 사용 `shellext.pl`
    * Browser Heloper Objects `bho.pl`
    * Scheduled Tasks `tasks.pl, taskcache.pl, at.pl`
    * AppCompatFlags `appcompatflags.pl`
    * 네트워크 관리 도구 (Landesk) `landesk.pl`
    * 해당 파일은 존재하지 않지만, RegRipper repo에 malware를 검색하면 해당 카테고리의 다양한 플러그인을 확인할 수 있다. `malware.pl`
    * 오디오 장치 관련 `audiodev.pl`

* Amcache.hve
    * Amcache 정보 `amcache.pl`

* NTUSER.DAT
    * Run, RunOnce 정보 출력 `run.pl`
    * Applets 프로그램 사용 정보 `applets.pl`
    * UserAssist - 더블클릭, 시작 프로그램을 통해 실행된 프로그램 `userassist.pl`
    * AppCompatFlags (PCA) `appcompatflags.pl`
    * Terminal Server Client `tsclient.pl, tsclient_tln.pl`
    * RecentDocs `recentdocs.pl`
    * ComDlg32 - 다른 이름으로 저장 등 대화상자를 통해 접근한 파일 정보 `comdlg32.pl`
    * MS Office 정보 `msoffice.pl`
        * MRU
        * TrustRecords - 제한된 보기에서 편집 사용 시 기록된다.
    * TypedPaths - 윈도우 검색 창에 입력한 값 `typedpath.pl`
    * TypedURLs - IE 주소 창에서 입력한 주소 `typedurls.pl`
    * TypedURLsTime - IE 주소 창에서 입력한 날짜와 시간 `typedurls.pl`
    * 검색에 대한 정보 `wordwheelquery.pl`

* USRCLASS.DAT
    * `inprocserver.pl`
    * MUICache `muicache.pl`
    * Shellbags `shellbags.pl`
        * MRUListEx - 가장 최근에 접근한 데이터

* 책에서 언급된 링크들
    * https://github.com/keydet89/RegRipper3.0
    * https://docs.microsoft.com/en-us/previous-versions//cc750583(v=technet.10)
    * http://amnesia.gtisc.gatech.edu/~moyix/suzibandit.ltd.uk/MSc/
    * https://pogostick.net/~pnh/ntpasswd/
    * https://code.google.com/archive/p/mft2csv/wikis/SetRegTime.wiki, https://github.com/jschicht/SetRegTime
    * https://www.swiftforensics.com/search?q=amcache
    * https://www.fireeye.com/content/dam/fireeye-www/services/freeware/shimcache-whitepaper.pdf
    * http://sentinelchicken.com/data/JolantaThomassenDISSERTATION.pdf, https://github.com/keydet89/Tools/blob/master/source/regslack.pl
    * https://hexacorn.com/tools/3r.html
    * https://www.mitec.cz/wrr.html
    * https://ericzimmerman.github.io/
    * https://github.com/libyal/libfwsi/blob/master/documentation/Windows%20Shell%20Item%20format.asciidoc
    * https://github.com/libyal/winreg-kb/blob/master/documentation/Application%20Compatibility%20Cache%20key.asciidoc
    * https://github.com/libyal/winreg-kb/blob/master/documentation/User%20Assist%20keys.asciidoc
    * https://github.com/libyal/winreg-kb/blob/master/documentation/SysCache.asciidoc

* 그 외 링크들
    * http://forensic.korea.ac.kr/DFWIKI/index.php/RegRipper
    * https://github.com/msuhanov/yarp
    * https://github.com/Silv3rHorn/4n6_misc/blob/master/registryFlush.py
    * https://github.com/Silv3rHorn/autoripy
    * https://blog.dfir.fi/tools/2020/02/19/install-regripper.html
    * http://windowsir.blogspot.com/2020/05/tips-on-using-regripper-v30.html
    * https://medium.com/@stdout_/installing-regripper-v2-8-on-ubuntu-26dc8bc8a2d3
    * https://brettshavers.com/brett-s-blog/entry/regripper

    * http://index-of.co.uk/readings/readings_cf2.pdf
    * <a href="https://github.com/proneer/Slides/blob/master/Windows/(FP)%20%EB%A0%88%EC%A7%80%EC%8A%A4%ED%8A%B8%EB%A6%AC%20%ED%8F%AC%EB%A0%8C%EC%8B%9D%EA%B3%BC%20%EB%B3%B4%EC%95%88%20(Registry%20Forensics).pdf">(FP) 레지스트리 포렌식과 보안 (Registry Forensics).pdf</a>
    * http://index-of.co.uk/Forensic/A%20Forensic%20Analysis%20of%20the%20Windows%20Registry.pdf
    * https://www.ssi.gouv.fr/uploads/2019/01/anssi-coriin_2019-analysis_amcache.pdf
    * https://dfir.ru/2018/12/02/the-cit-database-and-the-syscache-hive/