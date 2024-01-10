# windows 使用记录

## windows terminal 设置

[Windows Terminal 完美配置 PowerShell 7.1
](https://zhuanlan.zhihu.com/p/137595941)

## scoop 设置开发环境

参考文件

- [scoop——强大的 Windows 命令行包管理工具](https://www.jianshu.com/p/50993df76b1c)
- [scoop+git+配置代理](https://www.jianshu.com/p/0b6dcef94610)
- [官方文档设置 scoop 代理](https://github.com/ScoopInstaller/Scoop/wiki/Using-Scoop-behind-a-proxy)


## scoop 软件更新不到最新版本
使用scoop update * 提示最新
但是版本不是最新的
```
scoop bucket rm main
scoop bucket add main
```
删除/重新添加main
再执行
```
scoop update *
```
运行成功了
