# VIM & NEOVIM

## vimspector 插件安装

1. 修改~/.vim/setting-vim/vim-plug.vim 文件,增加以下内容:

```shell
" VimSpector
Plug 'puremourning/vimspector', {'do':'./install_gadget.py --enable-python --enable-go --enable-bash'}
Plug 'junegunn/fzf', {'dir': '~/.vim/plugged/fzf', 'do': './install --all' }
```

2. 修改~/.vim/user.vim 文件，增加以下内容:

```shell
" 12.Vimspector
" let g:vimspector_enable_mappings = 'HUMAN'
function! s:read_template_into_buffer(template)
	" has to be a function to avoid the extra space fzf#run insers otherwise
	execute '0r ~/.vim/vimspector_json/'.a:template
endfunction
command! -bang -nargs=* LoadVimSpectorJsonTemplate call fzf#run({
			\   'source': 'ls -1 ~/.vim/vimspector_json',
			\   'down': 20,
			\   'sink': function('<sid>read_template_into_buffer')
			\ })
noremap <leader>vs :tabe .vimspector.json<CR>:LoadVimSpectorJsonTemplate<CR>
sign define vimspectorBP text=☛ texthl=Normal
sign define vimspectorBPDisabled text=☞ texthl=Normal
sign define vimspectorPC text=🔶 texthl=SpellBad
```

这段主要是 vimspector 配置设置我没使用默认快捷键映射方案'HUMAN'或者'VISUAL_STUDIO'，F5 等功能键键位在 lin.vim 已经有定义了

通过使用 **'leader + vs'** 选择对应的模板文件在需要调试的文件夹下生成对应的配置文件

3. 这里需要在~/.vim/下新建`vimspector_json`文件夹，配置 bash,python,go 对应的模板文件  
   **python.json**:

```json
{
  "adapters": {
    "debugpy": {
      "command": ["python", "-m", "debugpy.adapter"],

      "name": "debugpy",

      "configuration": {
        "python": "python"
      }
    }
  },

  "configurations": {
    "run - debugpy": {
      "adapter": "debugpy",

      "configuration": {
        "request": "launch",

        "type": "python",

        "cwd": "${workspaceRoot}",

        "program": "${file}",

        "stopOnEntry": true,

        "console": "integratedTerminal"
      },

      "breakpoints": {
        "exception": {
          "raised": "N",

          "uncaught": ""
        }
      }
    }
  }
}

```

**shell.json**:

```json
{
  "configurations": {
    "run": {
      "adapter": "vscode-bash",
      "configuration": {
        "type": "shell",
        "request": "launch",
        "cwd": "${workspaceRoot}",
        "program": "${file}",
        "stopOnEntry": true,
        "console": "integratedTerminal"
      }
    }
  }
}

```

**go.json**:

```json
{
  "configurations": {
    "run": {
        "adapter": "vscode-go",
        "configuration": {
        "request": "launch",
        "program": "${fileDirname}",
        "mode": "debug",
        "dlvToolPath": "$HOME/go/bin/dlv"
      }
    }
  }
}

```

4. 简单的使用方式:  
   生成 vimspector.json 配置文件后  
   通过:`call vimspector#xxxx` 调用具体的函数 包括打断点,下一步,添加监控变量等等

**参考文件**:  
https://blog.csdn.net/AI_Fanatic/article/details/104923610  
https://github.com/theniceboy/nvim/blob/master/init.vim

## 服务器上的 vim 配色显示不对

1.在 vimrc 文件加入下面的内容

```shell
Syntax enable
set background=light
colorscheme gruvbox
```
<<<<<<< HEAD
2.修改.bashrc文件
=======

2.修改.bashrc 文件
>>>>>>> 712c3af5f29f3a01a92a18d2099c864612c708a7

```shell
TERM=xterm-256color
export TERM
```


## lazyvim 2024配置
**参考视频**:  
https://space.bilibili.com/1334071567/channel/collectiondetail?sid=1938094  
https://space.bilibili.com/35298669/channel/collectiondetail?sid=1572668  
