---
layout: post
title: "My Terminal Configs"
date: 2021-01-13
categories: blogposts
---

There's a couple configs that I've gotten used to, so add these when using a new computer! 



## Bash 

Just some aliases: 

```bash
alias rm='rm -i'  # safe rm 


### These may already exist on Unix systems, check for them! ####

# enable color support of ls and also add handy aliases
if [ -x /usr/bin/dircolors ]; then
    test -r ~/.dircolors && eval "$(dircolors -b ~/.dircolors)" || eval "$(dircolors -b)"
    alias ls='ls --color=auto'
    #alias dir='dir --color=auto'
    #alias vdir='vdir --color=auto'

    alias grep='grep --color=auto'
    alias fgrep='fgrep --color=auto'
    alias egrep='egrep --color=auto'
fi

# some more ls aliases
alias ll='ls -alF'
alias la='ls -A'
alias l='ls -CF'

# color user@host:dir$ prompt  (put this before conda init!) 
PS1='\[\033[1;36m\]\u\[\033[1;31m\]@\[\033[1;32m\]\h:\[\033[1;35m\]\w\[\033[1;31m\]\$\[\033[0m\] '
```




## Vim 

I like the "monokain" color scheme for Vim. To install it, copy the file to `~/.vim/colors/monkain.vim`. It can be found [here](https://github.com/flazz/vim-colorschemes/blob/master/colors/monokain.vim). 

`~/.vimrc`: 

```bash
set autoindent 
set tabstop=4 
set softtabstop=4  " number of spaces in tab 
set expandtab      " tabs are spaces 
set number         " use line numbers 
" set mouse=a        " xterm mouse (ignore line #s) 
" set clipboard=unnamed  " yank to clipboard

colorscheme monokain 
set showmatch      " highlight matching [{()}] 

" This lets you paste without bad indentation 
let &t_SI .= "\<Esc>[?2004h"
let &t_EI .= "\<Esc>[?2004l"
inoremap <special> <expr> <Esc>[200~ XTermPasteBegin()
function! XTermPasteBegin()
  set pastetoggle=<Esc>[201~
  set paste
  return ""
endfunction
```





## Git 

Run `git config --global credential.helper cache` to save login info on the current machine for this user. This writes to `~/.gitconfig`. Now also add this alias to that gitconfig file to visualize the branches with `git tree`: 

```bash
[alias]
    tree = log --graph --decorate --pretty=oneline --abbrev-commit
```
