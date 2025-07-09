---
layout: post
title: "My Terminal Configs"
date: 2021-01-13
categories: blogposts
---

There's a couple configs that I've gotten used to, so add these when using a new computer! 



## Bash 

```bash
# Safe rm 
alias rm='rm -i'  

# Recursively touch every file in current directory.
# Useful for HPC systems that scrub old files. -mtime flag makes more efficient when it errors out. 
alias touchall='find . -mtime +1 -exec touch {} +'


### These may already exist on Unix systems, check for them! ####

# History settings 
# don't put duplicate lines or lines starting with space in the history.
HISTCONTROL=ignoreboth

# append to the history file, don't overwrite it
shopt -s histappend

# for setting history length see HISTSIZE and HISTFILESIZE in bash(1)
HISTSIZE=10000
HISTFILESIZE=20000

# After each command, append to the history file and reread it
PROMPT_COMMAND="${PROMPT_COMMAND:+$PROMPT_COMMAND$'\n'}history -a; history -c; history -r"


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

# color user@host:dir$ prompt  (put this BEFORE conda init!) 
PS1='\[\033[1;36m\]\u\[\033[1;31m\]@\[\033[1;32m\]\h:\[\033[1;35m\]\W\[\033[1;31m\]\$\[\033[0m\] '

# function to make copying directory hierarchies easy! Useful for collecting ML results :-) 
copytree () {
    # requires 2 args: SRC and DST 
    python3.8 -c "import shutil; from os.path import expanduser; \
    shutil.copytree(expanduser('${1}'), expanduser('${2}'), dirs_exist_ok=True)"
}

# These change the process tracing permissions, 
#   useful for attaching processes during debugging. 
enable_debugging() {
    echo 0 | sudo tee /proc/sys/kernel/yama/ptrace_scope
    echo "ptrace_scope set to 0. Debugging enabled."
}
disable_debugging() {
    echo 1 | sudo tee /proc/sys/kernel/yama/ptrace_scope
    echo "ptrace_scope set to 1. Debugging disabled."
}

# Often need to sync results from offline HPC nodes.
#     Run this within a repo's "wandb/" dir to sync all offline runs
#     (with backgrounding so it doesn't take forever). 
wandb-sync-offline() {
    LOG_FILE="sync_log.txt" 
    for FILE in offline-run-*; do  
        wandb sync "$FILE" >> "$LOG_FILE" 2>&1 & 
        sleep 5 
    done 
    wait 
    echo "Finished syncing offline runs" 
}

# Prints all dirs under CWD with size less than 5KB. 
# Useful for finding failed tensorboard runs. 
find-small-dirs() {
    find . -type d -print0 | while IFS= read -r -d $'\0' dir; do
        size=$(du -sb "$dir" | awk '{print $1}');
        if (( size < 5000 )); then
            echo "${dir}";
        fi; 
    done
}
```




## Vim 

I like the "monokain" color scheme for Vim. To install it: 
```bash 
mkdir -p ~/.vim/colors
cd ~/.vim/colors
wget https://raw.githubusercontent.com/flazz/vim-colorschemes/master/colors/monokain.vim
```

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

Add these to `~/.gitconfig` to save login info on the current machine for this user, and to visualize the branches with `git tree`: 

```bash
[credential]
    helper = cache
[alias]
    tree = log --graph --decorate --pretty=oneline --abbrev-commit
```
