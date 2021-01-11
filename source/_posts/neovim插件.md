---
title: neovim插件
date: 2021-01-11 22:34:18
tags: neovim,vimspector,fzf
---

##### vimspector插件安装
1. 修改~/.vim/setting-vim/vim-plug.vim文件,增加以下内容:
```
" VimSpector
Plug 'puremourning/vimspector', {'do':'./install_gadget.py --enable-python --enable-go --enable-bash'}
Plug 'junegunn/fzf', {'dir': '~/.vim/plugged/fzf', 'do': './install --all' }
```
2. 修改~/.vim/user.vim文件，增加以下内容:
```
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
这段主要是vimspector配置设置我没使用默认快捷键映射方案'HUMAN'或者'VISUAL_STUDIO'，F5等功能键键位在lin.vim已经有定义了  

通过使用 **'leader + vs'** 选择对应的模板文件在需要调试的文件夹下生成对应的配置文件  

3. 这里需要在~/.vim/下新建`vimspector_json`文件夹，配置bash,python,go对应的模板文件  
**python.json**:
```
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
```
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
```
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
生成vimspector.json配置文件后  
通过:`call vimspector#xxxx` 调用具体的函数 包括打断点,下一步,添加监控变量等等  

**参考文件**:  
https://blog.csdn.net/AI_Fanatic/article/details/104923610  
https://github.com/theniceboy/nvim/blob/master/init.vim  
