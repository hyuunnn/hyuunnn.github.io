---
layout: post
title: "Timeline Explorer Plugin 개발"
description: ""
date: 2021-10-31
tags: [Forensic, Timeline, EZ-Tools]
---

Timeline Explorer는 Eric Zimmerman이 만든 도구이며, 엑셀처럼 csv 파일을 분석할 수 있지만

header를 그룹핑하여 시각화 및 분석에 도움을 준다는 장점이 있다. 

![TLEFilePlugins](/assets/images/TLEFilePlugins/0.png)

위 사진처럼 카테고리로 그룹핑 했을 때 원하는 정보를 효율적으로 분석할 수 있다.

Eric Zimmerman의 도구들을 보면 GUI 도구는 많이 없고 거의 다 CUI를 통하여 output으로 csv 파일을 만들어준다.

이러한 output 파일들을 효율적으로 분석하기 위하여 위 도구를 만든 것으로 보인다.

Plugin 목록을 보면 EZ-Tools들은 모두 구현되어 있지만, 이 외에 다른 도구들은 지원이 미비하다.

Timeline Explorer의 심플한 GUI와 기능으로 다른 도구의 output을 분석할 수 있게 만들면 좋겠다는 생각이 들어서 시작하게 되었다.

![TLEFilePlugins](/assets/images/TLEFilePlugins/1.png)

Registry Explorer Plugin 만들 때처럼 DLL 파일을 생성해야하기 때문에 클래스 라이브러리로 프로젝트를 생성한다.

![TLEFilePlugins](/assets/images/TLEFilePlugins/2.png)

왼쪽 사진은 TLEFileMisc/TLEFileMisc.csproj이며, 오른쪽은 방금 생성한 프로젝트의 csporj 파일이다.

이전에 만들어진 프로젝트에서 CsvHelper, NLog, ITLEFileSpec을 사용하기 위하여 코드를 추가해주고 

![TLEFilePlugins](/assets/images/TLEFilePlugins/3.png)

NuGet 패키지 복원까지 해주면 환경 세팅은 끝이 난다.

