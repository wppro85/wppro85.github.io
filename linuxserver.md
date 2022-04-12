# Linux 常用指令记录

## 常用命令

| 命令                                                         | 命令功能                                                  | 其他                                                         |
| :----------------------------------------------------------- | :-------------------------------------------------------- | :----------------------------------------------------------- |
| pwd                                                          | 显示当前绝对路径                                          |                                                              |
| ifconfig                                                     | 查询网络网卡情况                                          |                                                              |
| du -sh \*                                                    | 在项目目录下使用该命令 查看项目占用多少空间               | s代表Samarry 输出最后的统计总结果，不加的话每个子目录、文件都逐项列出 h代表human-readable 用k m g来表示文件大小 |
| ls -lah                                                      | 查看列表查看隐藏文件                                      | h代表human-readable                                          |
| ls -ld                                                       | 显示当前目录信息                                          | 而不是现实目录下的文件                                       |
| ls -r                                                        | 逆向排序显示                                              |                                                              |
| mkdir -pv                                                    | 一次性创建多级目录并显示                                  | -p 递归创建 -v显示创建目录的过程                             |
| touch                                                        | 创建文件                                                  | 存在的文件在touch的话，则更新文件修改的时间                  |
| cat /etc/os-release                                          | 查看系统信息                                              |                                                              |
| cat 文件名                                                   | 查看文件内容                                              | -n 显示文件行号(含空行) -b(不含空行) -A 显示换行(检查是否有windows下的回车符号^) |
| tac 文件名                                                   | 逆向查看内容与cat相反                                     |                                                              |
| more                                                         | 查看文件内容                                              | enter向下一行 space向下一页 b向上一页 =显示行号 :f 显示当前文件名和当前所在行号 |
| less                                                         | 查看文件内容                                              | y向上滚动 与more相比支持pageup pagedown翻页  -m显示百分比 -N显示行号 支持g/G跳转 支持“/”搜索 |
| tail -f                                                      | 实时显示                                                  | 实时查看日志文件变化 用tailf也可以                           |
|                                                              |                                                           | head -1 /etc/os-release ｜awk -F'[" ]' '{print $2}'以"或者空格 为分隔符 分割 并打印第二部分内容 |
| su - root                                                    | 切换到root用户                                            | ubuntu: sudo -i或sudo su - 输入普通密码就可以切换到root了 这些用户需要在wheel组中 |
| cp -a                                                        | 保留源文件的属性包括修改时间                              |                                                              |
| cp -a /root/soft {,bak}                                      | 复制并备份文件                                            | {}前的字符穿与{}中逗号后的字符串相连，形成新的字符串         |
|                                                              |                                                           |                                                              |
| ncdu                                                         | 查看磁盘文件占用空间 非常清楚                             |                                                              |
| lsblk -n -e7                                                 | 查看硬盘情况                                              |                                                              |
| uname -a                                                     | 查看系统版本                                              |                                                              |
| apt install /yum install                                     | 安装软件                                                  |                                                              |
| which python3                                                | 检查 python3 的程序位置                                   |                                                              |
| jobs                                                         | 查看后台任务是否在运行                                    |                                                              |
| ps -ef ｜ grep ‘python’                                      | 查看进程运行情况                                          |                                                              |
| netstat -ntlp                                                | 查看占用的端口                                            |                                                              |
| netstat -nat                                                 | 查看监听端口情况                                          |                                                              |
| lsof -nPi                                                    | 看端口开放情况                                            |                                                              |
| lsof -i :3000                                                | 查看 3000 端口进程情况                                    |                                                              |
| tail -n 5                                                    | 查看最新的日志                                            |                                                              |
| kill -9 %1                                                   | 杀死旧的进程                                              |                                                              |
| top                                                          | 查看进程状态                                              | P：cpu 占用排序 M:内存占用排序                               |
| chmod +x start.sh                                            | 加上可执行权限                                            |                                                              |
| histroy                                                      | 历史命令                                                  |                                                              |
| free                                                         | 检查内存                                                  |                                                              |
| df -l                                                        | 检查磁盘空间                                              |                                                              |
| scp ~/.vimrc bdyun:/root/                                    | 从本地复制文件到服务器                                    | 本地文件 服务器:远程路径                                     |
| scp bdyun:/root/~.vimrc ~                                    | 从服务器复制文件到本地                                    | 服务器:远程文件 本地路径                                     |
| rsync -avP ~/mysh txyun:/root/mysh/                          | 远程数据同步工具，可通过 LAN/WAN 快速同步多台主机间的文件 | -a 参数中包含了很多选项，后面会详细介绍 -v 查看到可视化过程 -P 显示同步过程，比如速率，比-v 更加详细 |
| pstree                                                       | 看进程情况                                                |                                                              |
| free -m                                                      | 查看内存                                                  |                                                              |
| curl -KIsS https://www.google.com                            | 测试谷歌访问情况                                          |                                                              |
| ssh -qfN -D localhost:1080 oversea-host ss -tln \| grep 1080 |                                                           |                                                              |
| cat /proc/cmdline                                            |                                                           |                                                              |
| systemctl list-unit-files - -state=enable virt-what          | 查看虚拟机类型                                            |                                                              |

———————————————————————

## SSH 配置

### 客户端

1.配置 ssh 别名

```shell
vim .ssh/config
Host txyun
  HostName 111.229.xxx.xx
  User root
```

2.生成 ssh 公私密钥对

```
ssh-keygen
```

3.利用 ssh 直接发送命令 添加公钥 到服务器的 authorized_keys,authorized_keys 文件权限也不能过高 chmod 600

```shell
cat .ssh/id_rsa.pub | ssh aliyun tee -a .ssh/authorized_keys # 或
cat .ssh/id_rsa.pub | ssh txyun tee -a .ssh/authorized_keys # （-a表示不是overwrite 是追加到文本后）
```

使用 ssh-copy-id 命令也可以实现上述功能 windows 系统没有这个功能，这个命令必须要能使用密码登陆

### 服务端

```shell
vim /etc/sshd_config # 编辑该文件修改如下内容
PermitRootLogin prohibit-password  #root只允许密钥登陆不允许密码登陆
PermitRootLogin yes # root既允许密钥登陆也允许密码登陆
PermitRootLogin no # 不允许root ssh登陆（可以用普通用户登陆再 su 或者sudo 提权）
```

### 其他

`ssh -v` 查看详细信息 可用于登陆失败排查错误
`-rw-rw-r— authorized_keys` 文件权限要求不能太高

`service sshd reload` 重新读取 sshd 配置
`systemctl restart sshd` 重启服务

## SSH 端口转发

### 正向端口转发

```sh
ssh -L 123:localhost:456 remotehost
ssh -L 123:farawayhost:456 remotehost
```

### 反向端口转发

```sh
ssh -R 123:localhost:456 remotehost
ssh -R 123:nearhost:456 remotehost
```

### 指令参考：

```shell
ssh-keygen # 生成公私钥对（带.pub的是公钥，不带的是私钥，私钥千万不要泄露给别人！）
ssh-copy-id # 一键传送私钥
vim .ssh/authorized_keys # 在阿里云/学校服务器上添加公钥
autossh -CNR 端口号二:localhost:22 服务器名 # 在学校服务器上挂起反向代理，绕过防火墙，如果服务器监听的ssh端口不是22号则修改localhost:后面的号码
nohup autossh -CNR 端口号二:localhost:22 服务器名 & #刚才那条命令的后台挂机版本，可以把TeamViewer什么的关掉了，如果测试通过就可以改用这条
```

### 参考的.ssh/config 文件

```shell
Host txyun
    HostName 腾讯云的IP
    User root
    Port 阿里云修改后端口
Host 内网服务器别名
    HostName localhost
    User root
    Port 端口二
    ProxyCommand ssh -q -x -W %h:%p 腾讯云别名
```

## 数据整理

### linux 日志系统

`ssh aliyun 'journalctl | grep ssh | grep "Disconnected from" | head -n300' | less`
过滤完结果以后 发送到本机显示结果

```shell
echo 'aba' | sed 's/[ab]//'    # 结果ba
echo 'dba' | sed 's/[ab]//'    # 结果 da
echo 'cccba' | sed 's/[ab]//g'  # 结果ccc
echo 'abcaba' | sed -E 's/(ab)*//g’  # 结果 ca
echo 'abcaba' | sed 's/\(ab\)*//g’   # 结果 ca
cat ssh.log | wc -l # 结果 300 300行
#不加-E 为传统模式 需要做\反斜杠转义  (an extended regular expression)
ssh txyun 'journalctl | grep ssh | grep "Disconnected from" |  > ssh.log  #获取登陆失败的日志信息
#获取登陆失败次数前10的用户名及日志记录数量
cat ssh.log  | sed -E 's/^.*Disconnected from (invalid |authenticating )?user (.*) [0-9.]+ port [0-9]+( \[preauth\])?/\2/' | sort | uniq -c | sort -nk1,1 | tail -n10
```

## FishShell

Yvan_1989 参考视频
https://www.bilibili.com/video/BV1s7411V7sR?from=search&seid=17945577765811205319

### PYTHON

### 修改 jupyter-notebook 默认启动端口

```
jupyter notebook --generate-config
```

## Github 加速

```
查询结果 修改/etc/hosts
https://ipaddress.com/website/github.global.ssl.fastly.net
https://ipaddress.com/website/github.com
```

### lunar vim 的关于 github 加速的提示

```shell
# Add the following lines to /etc/hosts to accelerate your installation.
mirror.ghproxy.com github.com
mirror.ghproxy.com raw.githubusercontent.com
```

### fastgithub

下载 FastGithub[https://github.com/dotnetcore/FastGithub]解压后 `./fastgithub`启动

方案一：
vim ~/.bashrc 增加

```shell
alias proxyon='export http_proxy=127.0.0.1:38457'
alias proxyoff='export http_proxy='
```

通过 `proxyon`启动代理 `proxyoff`关闭代理
通过 `echo $http_proxy`检查代理
方案二：

```shell
apt install proxychains4
vim /etc/proxychains4.conf #增加
http 127.0.0.1 38457
```

## 定时任务

```shell
at now +5 minutes
at now 02:00 2022-03-20
at> /bin/sync
at>/sbin/shutdown -r now
ctrl+D # 保存退出
```

## MYSQL

### 支持 root 用户允许远程连接 mysql 数据库

```mysql
grant all privileges on *.* to 'root'@'%' identified by '123456' with grant option;
flush privileges;
```

## 其他临时记录

### 腾讯云登陆提示

`warning: setlocale: LC_CTYPE: cannot change locale (UTF-8): No such file or directory`
fish shell 参考：https://www.jianshu.com/p/b83852603f3c

```shell
~/.config/fish 增加
export LC_ALL=en_US.UTF-8
export LANG=en_US.UTF-8
```

### B 站几个 linux 的 up 主

- LinuxTube https://space.bilibili.com/505839682
- TheCW https://space.bilibili.com/13081489
- MR\*Penguin 企鹅先生 https://space.bilibili.com/543384718
- Klesh https://space.bilibili.com/475071035
- 傻馒我们结婚吧 https://space.bilibili.com/393271893
- 胡小旭-\*- https://space.bilibili.com/326562584
- Mopip77 https://space.bilibili.com/2977639
