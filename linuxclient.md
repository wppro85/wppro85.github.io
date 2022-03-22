# 总标题

> ## 关于 markdown-preview.nvim 安装失败的解决方法

mac 下采用了官方的配置文件进行安装 a,即使挂了代理也无法安装成功
`Plug 'iamcco/markdown-preview.nvim', { 'do': 'cd app && yarn install' }`
经过排查 mac 一开始没有安装 yarn,尝试了
`brew install yarn`
安装后依然无法安装成功 markdown-preview,等待时间较长,尝试了以下方法:

1. 克隆完成后,进入 plugin 目录

`yarn install`

2. 使用终端代理 proxychains4 启动 nvim 挂代理安装插件

```
proxychains4 nvim
PlugInstall
```

安装后成功,再次启动 nvim 打开 markdown 文件:MarkdownPreview 可以正常启动浏览器进行预览了

## 解决外接机械键盘功能键区无法使用的问题

**参考文件**:  
How to swap the “fn” use of Function keys on an Apple Keyboard in Linux  
https://superuser.com/questions/79822/how-to-swap-the-fn-use-of-function-keys-on-an-apple-keyboard-in-linux

```
echo 0 > /sys/module/hid_apple/parameters/fnmode
```

Or, in case of permission issue:  
`echo 0 | sudo tee /sys/module/hid_apple/parameters/fnmode`

This will prevent you from having to reboot. Adding the option is a good idea, so the change persists through reboots.  
0 = Fn key disabled  
1 = Fn key pressed by default  
2 = Fn key released by default  
From `/drivers/hid/hid-apple.c` line 42:  
 Mode of fn key on Apple keyboards (0 = disabled, [1] = fkeyslast, 2 = fkeysfirst)

his worked for me on **Fedora 24** Create a new file for SystemD to start.
`gedit /usr/lib/systemd/system/mac-keyboard.service`  
**Ensure the file contains the following**

```
[Unit]
 Description=mac-keyboard
[Service]
 Type=oneshot
 ExecStart=/bin/sh -c "echo 2 > /sys/module/hid_apple/parameters/fnmode"
 ExecStop=/bin/sh -c "echo 1 > /sys/module/hid_apple/parameters/fnmode"
 RemainAfterExit=yes
[Install]
 WantedBy=multi-user.target
```

1. Reload SystemD to read your new file  
   `systemctl --system daemon-reload`
2. Start the SystemD service.  
   `systemctl start mac-keyboard.service`
3. Enable service to start on boot.  
   `systemctl enable mac-keyboard.service`  
   Reference: https://www.dalemacartney.com/2013/06/14/changing-the-default-function-key-behaviour-in-fedora/

**按照文件选择**

```
echo 2 | sudo tee /sys/module/hid_apple/parameters/fnmode
```

开启 F 区标准键盘功能  
mod+z 选择 manjaro setting manager  
键盘布局选择 apple,功能键就可以正常使用了(我这里使用的 k2 键盘有 mac 的键盘布局模式)

## 解决蓝牙链接问题

参考文件:  
https://www.cnblogs.com/lirenhe/p/12716395.html
https://blog.csdn.net/github_39664893/article/details/88758120

## 科学上网方案

v2ray 命令行工具 安装方式:`sudo pacman -S v2ray`  
Qv2ray 图形化支持订阅 安装方式:`yay -S Qv2ray`  
终端代理工具 proxychains 安装方式:`sudo pacman -S proxychins`  
修改 proxychains 配置文件:  
`sudo vim /etc/proxychains.conf`  
修改以下内容:  
`socks4 127.0.0.1 1089`(根据 v2ray 配置情况 默认 1089 端口)

## HDMI 没有声音的问题

https://wiki.archlinux.org/index.php/PulseAudio/Examples#Overview_of_the_targeted_PulseAudio_configuration

### HDMI output configuration 的相关部分

`aplay -D plughw:1,3 /usr/share/sounds/alsa/Front_Right.wav`

## Manually configuring PulseAudio to detect the Nvidia HDMI

Having identified which HDMI device is working, PulseAudio can be forced to use it via an edit to `/etc/pulse/default.pa`:  
`# load-module module-alsa-sink device=hw:1,7`  
where the 1 is the card and the 7 is the device found to work in the previous section restart pulse audio

```
 $ pulseaudio -k
 $ pulseaudio --start
```

## Automatically switch audio to HDMI

Create a script to switch to the desired audio profile if an HDMI cable is plugged in:

```
/usr/local/bin/hdmi_sound_toggle.sh
#!/bin/bash

export PATH=/usr/bin

USER_NAME=$(who | awk -v vt=tty$(fgconsole) '$0 ~ vt {print $1}')
USER_ID=$(id -u "$USER_NAME")
CARD_PATH="/sys/class/drm/card0/"
AUDIO_OUTPUT="analog-surround-40"
PULSE_SERVER="unix:/run/user/"$USER_ID"/pulse/native"

for OUTPUT in $(cd "$CARD_PATH" && echo card*); do
  OUT_STATUS=$(<"$CARD_PATH"/"$OUTPUT"/status)
  if [[ $OUT_STATUS == connected ]]
  then
    echo $OUTPUT connected
    case "$OUTPUT" in
      "card0-HDMI-A-1")
        AUDIO_OUTPUT="hdmi-stereo" # Digital Stereo (HDMI 1)
     ;;
      "card0-HDMI-A-2")
        AUDIO_OUTPUT="hdmi-stereo-extra1" # Digital Stereo (HDMI 2)
     ;;
    esac
  fi
done
echo selecting output $AUDIO_OUTPUT
sudo -u "$USER_NAME" pactl --server "$PULSE_SERVER" set-card-profile 0 output:$AUDIO_OUTPUT+input:analog-stereo

```

Make the script executable:  
`chmod +x /usr/local/bin/hdmi_sound_toggle.sh`  
Create a udev rule to run this script when the status of the HDMI change:

```
/etc/udev/rules.d/99-hdmi_sound.rules
KERNEL=="card0", SUBSYSTEM=="drm", ACTION=="change", RUN+="/usr/local/bin/hdmi_sound_toggle.sh"
```

To make the change effective don't forget to reload the udev rules:  
`udevadm control --reload-rules`  
A reboot might be required.

## 中文显示问题

当前最新版本(manjaro-i3-20.0-200426-linux56)在安装时中文无法正常显示，可以选择英文提示安装，我是选择的中文，然后看着一堆方块猜着输入信息，然后点下一步下一步完成的．安装完成后首次进入系统，你会发现桌面右上角日历上显示的＂x 月,星期 x"的中文字符显示为方块，你需要修改配置文件/usr/share/conky/conky_maia 把其中默认的字体＂Bitstream Vera＂改为＂anti＂还有浏览器的标题或其他软件显示中文还会有问题，这时就需要安装新的中文字体解决

## 安装完你发现 conky 貌似有口口口这样的缺少字体

`/usr/share/conky/conky_maia` #桌面右侧的状态栏  
`/usr/share/conky/conky1.10_shortcuts_maia` #桌面左侧的快捷键显示栏  
**修改 conky_maia 支持中文**  
`font = 'WenQuanYi Micro Hei:size=8',`或者  
`font = 'WenQuanYi Zen Hei:size=8',`  
vim 打开可以用以下命令直接替换  
`:%s/Bitstrea‘m Vera/WenQuanYi Micro Hei/`

## 修改键盘映射

xmodmap -pke -> ·/.xmodmap 输出键盘每个键位对应的
xev  
xmodmap ~/.xmodmap  
配置和文件 需要在 i3 config 文件中添加

```
exec_always sleep1; xmodmap ~/.xmodmap
exec_always variety
exec_always compton
```

## fzf 模糊文件

```plain
export FZF_DEFAULT_OPTS="--height 40% --layout=reverse --preview '(highlight -O ansi {} || cat {}) 2> /dev/null | head -500'"
```

## 小工具

下载：t-get (命令行工具， npm install -g t-get)  
虚拟机：VirtualBox  
模拟器：qemu, bochs
快速跳转工具: z 一个目录跳转工具，相当于加强版的`cd`，首先去[戳这里](https://github.com/rupa/z)下载，是一个脚本，接着添加到`~/.zshrc`中：

## 开发工具配置

[https://zhuanlan.zhihu.com/p/139388970](https://zhuanlan.zhihu.com/p/139388970)

## zsh,bash 添加环境变量:

```
nvim ~/.zshrc
nvim ~/.bashrc
```

添加以下内容:  
`export PATH=$PATH:~/hexo/node_modules/.bin`

## fish 添加环境变量

参考内容: https://stackoverflow.com/questions/26208231/modifying-path-with-fish-shell  
`set -U fish_user_paths ~/hexo/node_modules/.bin $fish_user_paths`

## google-chrome 浏览器无法在 manjaro 下设置代理

Firefox,可以设置全局代理,通过 qv2ray 进行代理即可正常访问 google  
Chrome、Chromium 无法设置系统代理,必须通过插件或者命令行

### 方式一:通过命令行启动代理上网 `google-chrome-stable --proxy-server=socks5://127.0.0.1:1089`

### 方式二:通过 switchy omega 插件进行科学上网

1. 通过 github 下载 google-access 谷访访问助手破解手,启动插件,登陆同步
2. 同步 Chrome 浏览器插件(包括 proxy switchy omega)
3. SwitchyOmega 设置:

```情景模式名称:v2ray
   网址协议:(默认)
   代理方式SOCKS
   代理服务器:127.0.0.1
   代理端口:1089
```

PAC 规则在 Qv2ray 进行设置,一开始两种方式都报错,始终不能代理访问成功.  
最终发现是**本地时间**不对,调整完成后就正常了.

## 蓝牙配对失败的问题 手动命令行配对

Manjaro 默认的蓝牙总会有点小问题！所以在安装好 Manjaro 后如下设置下：  
一、安装  
基本的包  
`pacman -S bluez bluez-utils`  
与音频有关的包

```
pacman -S pulseaudio-bluetooth pavucontrol pulseaudio-alsa pulseaudio-bluetooth-a2dp-gdm-fix
```

二、修改设置

```
sudo nano /etc/bluetooth/main.conf
```

修改 FastConnectable=false，取消#注释，改为 FastConnectable=true  
修改 AutoEnable=false，去掉前面的#注释，改为 AutoEnable=true
重启！！！

三、连接蓝牙设备  
不知道为什么 Manjaro 下的配对码显示不出来，所以我们的控制台下进行连接配对操作：  
1、进入蓝牙控制台  
`bluetoothctl`  
2、打开相关项

```
power on
agent on
default-agent
```

3、扫描蓝牙设备  
`scan on`  
4、根据上面来找出来的蓝牙 MAC 码，对设备配对 pair 键盘的 MAC 地址（可用 TAB 自动出来）屏幕可能会输出类似于以下的信息：

`[agent] Passkey: xxxxxx`  
这时候在你的蓝牙键盘上输入 6 位配对码后再回车即可完成配对！  
5、设备信任设备  
`trust 键盘的MAC地址`  
6、连接设备
connect 键盘的 MAC 地址
https://peng.likehere.me/index.php/archives/41/

## K380 蓝牙键盘连接到 manjaroF 键功能设置默功能

https://github.com/jergusg/k380-function-keys-conf

## 关于 efi 和 grub 启动失败修复的问题

参考文件:
Manjaro UEFI 启动修复  
https://blog.mynoee.com/archives/146/  
修复 UEFI 模式下 Manjaro Linux 启动问题  
https://blog.csdn.net/weixin_30826761/article/details/97554989  
UEFI - Install Guide  
https://serverfault.com/questions/3132/how-do-i-find-the-uuid-of-a-filesystem  
How do I find the UUID of a filesystem  
https://serverfault.com/questions/3132/how-do-i-find-the-uuid-of-a-filesystem

![20210106-v2U9yi](https://cdn.jsdelivr.net/gh/wppro85/blog_img@main/images/20210106-v2U9yi.png)
