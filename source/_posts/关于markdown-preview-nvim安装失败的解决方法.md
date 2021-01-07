---
title: 关于markdown-preview.nvim安装失败的解决方法
date: 2021-01-05 22:04:51
tags: vim,neovim,markdown
---

##### mac下采用了官方的配置文件进行安装a,即使挂了代理也无法安装成功  
```Plug 'iamcco/markdown-preview.nvim', { 'do': 'cd app && yarn install'  }```  
##### 经过排查mac一开始没有安装yarn,尝试了  
```brew install yarn```  
##### 安装后依然无法安装成功markdown-preview,等待时间较长,尝试了以下方法:  
1. 克隆完成后,进入plugin目录
```
yarn install
```
2. 使用终端代理proxychains4 启动nvim 挂代理安装插件
```
proxychains4 nvim   
PlugInstall
```
安装后成功,再次启动nvim打开markdown文件:MarkdownPreview可以正常启动浏览器进行预览了
