---
layout: post
title: "LG 그램 프리징 현상"
description: ""
date: 2023-07-13
tags: [etc]
---

아무런 이유 없이 갑자기 화면이 멈춤

구글링을 해보니 LG 그램 프리징 관련 글이 많이 있음

OneDrive 프로그램 때문에 멈춘다는 사람도 있었으나 나는 그전부터 삭제해서 사용하고 있었음 (<a href="https://www.clien.net/service/board/park/13221872">1</a>, <a href="https://www.fmkorea.com/3160999112">2</a>)

프리징 발생 후 재부팅하여 가장 최근에 발생한 이벤트 로그를 확인해보니 `10016, DistributedCOM`에 CLSID는 `{2593F8B9-4EAF-457C-B68A-50F6B8EA6B54}` APPID는 `{15C20B67-12E7-4BB6-92BB-7AFF07997402}`

<a href="https://tiplog.co.kr/1422">1</a>, <a href="https://itsvc.tistory.com/entry/%EC%9D%B4%EB%B2%A4%ED%8A%B8-ID-10016-DistributedCOM-%EC%98%A4%EB%A5%98-%ED%95%B4%EA%B2%B0-%EB%B0%A9%EB%B2%95">2</a>, <a href="https://showering.tistory.com/144">3</a>, <a href="https://gall.dcinside.com/m/td2/264445">4</a>, <a href="https://www.backyrd.net/entry/20211004/1633347934">5</a>, <a href="https://learn.microsoft.com/ko-kr/troubleshoot/windows-client/application-management/event-10016-logged-when-accessing-dcom">6</a>, <a href="https://answers.microsoft.com/ko-kr/windows/forum/all/%EC%9D%B4%EB%B2%A4%ED%8A%B8id-10016-distributedcom/c6572cb7-4143-4417-a9b2-4444971daf0b">7</a>

위 링크들 참고해서 설정했는데 더 지켜봐야겠다...
