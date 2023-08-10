---
layout: post
title: "WSL 접근 에러"
description: ""
date: 2023-08-10
tags: []
---

파일 탐색기에서 `\\wsl.localhost`에 접속하려고 했는데 에러가 발생했다.

![0](/assets/images/wsl-access-error/0.png)

![1](/assets/images/wsl-access-error/1.png)

https://learn.microsoft.com/en-us/answers/questions/602778/windows-11-does-not-read-linux-tree

위 링크를 보면 `HKLM\SYSTEM\CurrentControlSet\Control\NetworkProvider\Order`에 있는 `ProviderOrder` 값에서 `RDPNP, P9NP`가 맨 앞으로 와야한다고 한다...

나는 `cbfsconnect2017`을 지우니까 해결됐다.

![2](/assets/images/wsl-access-error/2.png)

### 참고 링크

https://github.com/microsoft/WSL/discussions/7742

<a href="https://www.reddit.com/r/bashonubuntuonwindows/comments/og727b/accessing_wsllocalhost_through_windows_11_file/">reddit</a> - <a href="https://github.com/microsoft/WSL/issues/4027#issuecomment-496628274">github issue</a>

https://github.com/microsoft/WSL/issues/5211

https://github.com/microsoft/WSL/issues/3995
