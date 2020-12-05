---
layout: post
title: "WSL VIM에서 Copy/Paste"
description: ""
date: 2020-12-06
tags: [Vim]
---

https://www.codentalks.com/t/topic/3687/5

위 사이트에서 설명하는 방식으로 vimrc를 세팅해주면 된다.

```vim
vnoremap <C-c> "zy:call writefile(getreg('z', 1, 1), "/tmp/vimbuffer") \| !cat /tmp/vimbuffer \| /mnt/c/Windows/System32/clip.exe <CR><CR>
```

.vimrc 외에도 다양한 개발자들의 dotfiles를 보면서 어떻게 세팅했는지 공부해보고 직접 커스터마이징 해봐야겠다. ~~(어중간하게 알고 있는 bash script도 공부 해야겠다.)~~

* 링크
    * https://www.codentalks.com/t/topic/3687/5
    * https://hidekuma.github.io/vim/wsl/synchronize-system-clipboard-vim-on-WSL/
    * https://github.com/johngrib/simple_vim_guide/blob/master/md/vimrc.md
    * https://github.com/junegunn/dotfiles
    * https://hongsii.github.io/2018/01/29/vim_configuration/
    * https://johngrib.github.io/wiki/Vim/
    * https://nolboo.kim/blog/2016/11/15/vim-for-beginner/
    * https://nolboo.kim/practical-vim/
    * https://agvim.wordpress.com/2017/09/05/vim-plugins-100/
    * https://github.com/tmwilliamlin168/CP-YouTube/blob/master/.vimrc
    * https://gist.github.com/ChangJoo-Park/5410685
    * https://github.com/amix/vimrc
    * https://github.com/dokenzy/vimrc
    * https://github.com/nvie/vimrc
    * https://github.com/vgod/vimrc
    * https://dotfiles.github.io/
    * https://github.com/geohot/configuration
    * https://github.com/search?o=desc&q=dotfiles&s=stars&type=Repositories
    * https://github.com/webpro/awesome-dotfiles