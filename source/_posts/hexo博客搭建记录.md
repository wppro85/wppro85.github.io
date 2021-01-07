---
title: hexo博客搭建记录
date: 2021-01-05 00:00:30
tags: hexo,github,upic,阿里云域名
---



#### 安装hexo完成初始化配置

#####  安装Nodejs
`node -v`	#查看node版本
`npm -v`	#查看npm版本
`npm install -g cnpm --registry=http://registry.npm.taobao.org`	#安装淘宝的cnpm 管理器
`cnpm -v`	# 查看cnpm版本
`cnpm install -g hexo-cli`    #安装hexo框架
`hexo -v`	#查看hexo版本
`mkdir blog`	#创建blog目录
`cd blog`	 #进入blog目录
`sudo hexo init` 	#生成博客 初始化博客
`hexo s`	#启动本地博客服务
`http://localhost:4000/`	#本地访问地址
`hexo n "我的第一篇文章"` #创建新的文章 

##### 返回blog目录
`hexo clean` #清理
`hexo g` #生成

##### Github创建一个新的仓库 YourGithubName.github.io
`cnpm install --save hexo-deployer-git` #在blog目录下安装git部署插件

##### 配置_config.yml 
```
# Deployment
## Docs: https://hexo.io/docs/deployment.html
 deploy:
   type: git
   repo: https://github.com/YourGithubName/YourGithubName.github.io.git
   branch: master
```
`hexo d`	#部署到Github仓库里
`https://YourGithubName.github.io/`  #访问这个地址可以查看博客

##### 配置next主题
`git clone https://github.com/iissnan/hexo-theme-next.git themes/next` #下载next主题到本地,这个主题直接带全文内容搜索,界面比较简洁

<img src="https://cdn.jsdelivr.net/gh/wppro85/blog_img@main/images/20210106-image-20210106092140528.png" alt="image-20210106092140528" style="zoom:25%;" />

#修改hexo根目录下的 _config.yml 文件 ： theme: next

`hexo c`	#清理一下
`hexo g`	#生成
`hexo d`	#部署到远程Github仓库
`https://YourGithubName.github.io/`  #查看博客





#### uPic图床设置 token添加

#### 关于阿里云域名解析的设置
![rCct52](https://cdn.jsdelivr.net/gh/wppro85/blog_img@main/uPic/rCct52.png)

阿里云上这样设置(更新时间还是比较长的 一开始ping域名ping不同,后来还是ping不同,但是访问正常,以为设置不对,也不知道怎么处理,直接放了一天自动就好了,访问正常,域名定向也正常)

github进入项目页面 选择Settings

![20210105-bVsnaT](https://cdn.jsdelivr.net/gh/wppro85/blog_img@main/images/20210105-bVsnaT.png)

项目目录下会自动生成CNAME文件,内容就是绑定的域名

这里需要注意本地blog(hexo项目)文件夹下 必须在source目录(不是source/_posts目录)下保持CNAME文件与github一致,否则每次更新都会删除 github的CNAME文件,造成域名定向失败.

#### 通过github多设备管理hexo内容
参考文件:  
1. https://blog.csdn.net/u014568993/article/details/84308188
2. https://www.jianshu.com/p/0558c041e56d



#### 参考内容:

https://www.bilibili.com/video/BV1Yb411a7ty

https://zhuanlan.zhihu.com/p/138012354
https://blog.svend.cc/upic/tutorials/github/

