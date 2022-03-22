# MacOS 系统使用纪录

## iTerm2

在 macOS 中，我们[iTerm2](https://www.iterm2.com/)中的 terminal color 一般都设置为`xterm-256color`，这没毛病，但问题是它不能在 tmux 环境中使用:`$TERM should be "screen-256color" or "tmux-256color" in tmux. Colors might look wrong.`

为此，我们可以在[zshrc](https://github.com/at7h/dots/blob/master/zsh/zshrc#L109)中加上这样一段配置：

```plain
if [[ $TMUX != "" ]] then
export TERM="tmux-256color"
else
export TERM="xterm-256color"
fi
```

而在 tmux 环境中一般默认使用系统自带的`screen-256color`，这在大多数情况是够用的，但是它不支持任何斜体字体样式，所以在 Vim 中类似代码高亮这种就会很有问题。
为此，我们可以在[tmux.conf](https://github.com/at7h/dots/blob/master/tmux/tmux.conf.local#L7)中加上设置：

```plain
set -g default-terminal "tmux-256color"
set-option -a terminal-overrides ",*256col*:RGB"
```

## Mac 系统 vim 中文输入法

输入中文后 退出插入模式后自动切换成英文输入法
参考内容:https://github.com/hiberabyss/smartim
但是对于我的 VIM 和 MAC 来说还有两步需要设置 1.把 im-select 拷贝到/usr/local/bin  
`sudo cp ~/.vim/plugged/smartim/plugin/im-select /usr/local/bin/`  
确保在终端下可以直接使用 im-select  
2.修改 lin.vim 的配置  
`nvim ~/.vim/user.vim`  
加入以下内容:

```
" 13. im-select配置
let g:smartim_default = 'com.apple.keylayout.ABC'
```

## Mac 安装 Yabai 的过程

- https://github.com/itgoyo/500Days-Of-Github/issues/224
- unixporn www.reddit.com/r/unixporn
