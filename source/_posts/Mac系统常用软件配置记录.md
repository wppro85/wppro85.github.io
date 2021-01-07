---
title: Mac系统常用软件配置记录
date: 2021-01-06 08:43:01
tags: Mac,Soft,profile,config
---


#### nvALT - 随叫随到随处用的Mac速记工具  
#### unclutter - 藏在顶栏的便签、临时文件、剪切板记录器三合一工具  
#### 安装v2ray命令行代理工具和qv2ray图形界面工具  
```
brew install v2ray  
brew install qv2ray  
```
![20210106-v2U9yi](https://cdn.jsdelivr.net/gh/wppro85/blog_img@main/images/20210106-v2U9yi.png)
#### 几个重要的配置文件
```
.zshrc
.tmux.conf
.vimrc init.vim
```

#### Mac系下解决neovim输入中文后 退出插入模式后自动切换成英文输入法
参考内容:https://github.com/hiberabyss/smartim
但是对于我的VIM和MAC来说还有两步需要设置
1.把im-select拷贝到/usr/local/bin  
`sudo cp ~/.vim/plugged/smartim/plugin/im-select /usr/local/bin/`  
确保在终端下可以直接使用im-select  
2.修改lin.vim的配置  
`nvim ~/.vim/user.vim`  
加入以下内容:
```
" 13. im-select配置
let g:smartim_default = 'com.apple.keylayout.ABC'
```

