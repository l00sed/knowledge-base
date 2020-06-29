## Full ~/.bashrc file:

Feel free to copy bits and pieces. The full file may be unusable to people other than myself. This is more for my own benefit / backup. I recommend taking a look the [environments](environments) page to get a better breakdown of the useful bits and pieces.


```
# ~/.bashrc: executed by bash(1) for non-login shells.
# see /usr/share/doc/bash/examples/startup-files (in the package bash-doc)
# for examples

# If not running interactively, don't do anything
case $- in
    *i*) ;;
      *) return;;
esac

# don't put duplicate lines or lines starting with space in the history.
# See bash(1) for more options
HISTCONTROL=ignoreboth

# append to the history file, don't overwrite it
shopt -s histappend

# for setting history length see HISTSIZE and HISTFILESIZE in bash(1)
HISTSIZE=1000
HISTFILESIZE=2000

# check the window size after each command and, if necessary,
# update the values of LINES and COLUMNS.
shopt -s checkwinsize

# If set, the pattern "**" used in a pathname expansion context will
# match all files and zero or more directories and subdirectories.
#shopt -s globstar

# make less more friendly for non-text input files, see lesspipe(1)
[ -x /usr/bin/lesspipe ] && eval "$(SHELL=/bin/sh lesspipe)"

# set variable identifying the chroot you work in (used in the prompt below)
if [ -z "${debian_chroot:-}" ] && [ -r /etc/debian_chroot ]; then
    debian_chroot=$(cat /etc/debian_chroot)
fi

# set a fancy prompt (non-color, unless we know we "want" color)
case "$TERM" in
    xterm-color|*-256color) color_prompt=yes;;
esac

# uncomment for a colored prompt, if the terminal has the capability; turned
# off by default to not distract the user: the focus in a terminal window
# should be on the output of commands, not on the prompt
#force_color_prompt=yes

if [ -n "$force_color_prompt" ]; then
    if [ -x /usr/bin/tput ] && tput setaf 1 >&/dev/null; then
	# We have color support; assume it's compliant with Ecma-48
	# (ISO/IEC-6429). (Lack of such support is extremely rare, and such
	# a case would tend to support setf rather than setaf.)
	color_prompt=yes
    else
	color_prompt=
    fi
fi

#if [ "$color_prompt" = yes ]; then
#    PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '
#else
#    PS1='${debian_chroot:+($debian_chroot)}\u@\h:\w\$ '
#fi
#unset color_prompt force_color_prompt
RCol='\033[0m'
Gre='\033[32m';
Red='\033[31m';
Blu='\033[34m';
Yel='\033[33m';
PS1="${RCol}┌─[\`if [ \$? = 0 ]; then echo "${Gre}"; else echo "${Red}"; fi\`\T\[${Rcol}\] \[${Blu}\]\u@\h\[${RCol}\] \[${Yel}\]\w\[${RCol}\]\$(__git_ps1)]\n└───▶ "

# If this is an xterm set the title to user@host:dir
case "$TERM" in
xterm*|rxvt*)
    PS1="\[\e]0;${debian_chroot:+($debian_chroot)}\u@\h: \w\a\]$PS1"
    ;;
*)
    ;;
esac

# enable color support of ls and also add handy aliases
if [ -x /usr/bin/dircolors ]; then
    test -r ~/.dircolors && eval "$(dircolors -b ~/.dircolors)" || eval "$(dircolors -b)"
    alias ls='ls --color=auto 2>/dev/null'
    #alias dir='dir --color=auto'
    #alias vdir='vdir --color=auto'

    alias grep='grep --color=auto'
    alias fgrep='fgrep --color=auto'
    alias egrep='egrep --color=auto'
fi

# colored GCC warnings and errors
#export GCC_COLORS='error=01;31:warning=01;35:note=01;36:caret=01;32:locus=01:quote=01'

# some more ls aliases
alias ll='ls -alF'
alias la='ls -A'
alias l='ls -CF'

# Add an "alert" alias for long running commands.  Use like so:
#   sleep 10; alert
alias alert='notify-send --urgency=low -i "$([ $? = 0 ] && echo terminal || echo error)" "$(history|tail -n1|sed -e '\''s/^\s*[0-9]\+\s*//;s/[;&|]\s*alert$//'\'')"'

# Alias definitions.
# You may want to put all your additions into a separate file like
# ~/.bash_aliases, instead of adding them here directly.
# See /usr/share/doc/bash-doc/examples in the bash-doc package.

if [ -f ~/.bash_aliases ]; then
    . ~/.bash_aliases
fi

# enable programmable completion features (you don't need to enable
# this, if it's already enabled in /etc/bash.bashrc and /etc/profile
# sources /etc/bash.bashrc).
if ! shopt -oq posix; then
  if [ -f /usr/share/bash-completion/bash_completion ]; then
    . /usr/share/bash-completion/bash_completion
  elif [ -f /etc/bash_completion ]; then
    . /etc/bash_completion
  fi
fi

export ip4=$(ifdata -pa wlp2s0)
alias serve="browser-sync start -s -f . --extensions 'html' --no-notify --host $ip4 --port 9000"

alias apacheStart='sudo service apache2 start'
alias apacheStop='sudo service apache2 stop'
alias apacheRestart='sudo service apache2 restart'
alias mysqlStart='sudo service mysql start'
alias mysqlStop='sudo service mysql stop'
alias mysqlRestart='sudo service mysql restart'
alias lstart='sudo service apache2 start && sudo service mysql start'
alias lhome='cd "/var/www/l-o-o-s-e-d/html"'
alias dhome='cd "/home/dan/Documents/dato.work"'
alias loosed='ssh dan@l-o-o-s-e-d.net'
alias loosedtv='ssh dan@loosed.tv'
alias loosedstore='ssh dan@loosed.store'
alias dato='ssh dan@dato.work'

alias home='cd "/home/dan/Documents"'
alias tvhome='cd "/home/dan/Documents/loosed.tv/node_modules/node-media-server"'
# Virtual Env Wrapper
export WORKON_HOME='~/.virtualenvs'
source /usr/local/bin/virtualenvwrapper.sh

# Tmux
alias td='tmux detach'
alias t0='tmux attach -t 0'
alias t1='tmux attach -t 1'
alias t2='tmux attach -t 2'
alias tmain='tmux attach -t main'
alias tirss='tmux attach -t irssi'
alias tslack='tmux attach -t slack'

#alias phome='cd "/home/dan/Documents/p5"'
alias tball='tar xopf'
alias wethr='wethr --imperial'

# Cool-Retro-Terminal
alias crt='cool-retro-term'

# Wireguard
alias wgup='sudo wg-quick up wg0'
alias wgdown='sudo wg-quick down wg0'

# Android Studio
export ANDROID_HOME=/home/dan/Android/Sdk
export PATH=$PATH:$ANDROID_HOME/platform-tools
export PATH=$PATH:$ANDROID_HOME/emulator
export PATH=$PATH:$ANDROID_HOME/tools
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
alias emulatorUp='adb start-server && $ANDROID_HOME/emulator/emulator @Nexus_5_API_28'
alias streamStart='pm2 start pm2.config.js --env sit'

# FFMpeg
alias mkvid='ffmpeg -r 1 -f image2 -pattern_type glob -i "*.png" -vf scale=720:480 -filter:v "crop=in_w:480" -vcodec libx264 -crf 0 -pix_fmt yuv444p test.mp4'
alias convgif='ffmpeg -i test.mp4 -vf scale=iw/2:ih/2 -f gif test.gif'
function mkgif() {
  ffmpeg -pattern_type glob -r 1 -i "$1" -vf "scale='min(640,iw)':'min(480,ih)':force_original_aspect_ratio=decrease,pad=640:480:(ow-iw)/2:(oh-ih)/2" -pix_fmt yuv420p -f gif output.gif
}
export -f mkgif

# Android
alias android='scrcpy --max-size 1080 --window-x 1920 --window-y 1080 --window-borderless -S'
alias androidF='android --fullscreen'

# OBS
alias obs='obs-studio --scene "screen" --startstreaming &'

# Trash-CLI
alias rm='trash -v'

# Docker
alias Dls='docker-machine ls'
function Dstart() {
  local env="${1:-dev}"
  docker-machine start "$env"
}
export -f Dstart
function Deval() {
  local env="${1:-dev}"
  eval $(docker-machine env "$env")
}
function Dstop() {
  local env="${1:-dev}"
  docker-machine stop "$env"
}
export -f Dstop
export -f Deval
function Dbuild() {
  local yml="${1:-local.yml}"
  sudo docker-compose -f "$yml" build
}
export -f Dbuild
function Dup() {
  local yml="${1:-local.yml}"
  sudo docker-compose -f "$yml" up
}
export -f Dup
function Dcreate() {
  local env="${1:-dev}"
  docker-machine create $env
}
export -f Dcreate
function Drm() {
  local env="${1:-dev}"
  docker-machine rm $env
}
export -f Drm

# polygonized lqip image for single file
function poly() {
  if [ ${1##*.} != "gif" ]
    then
      primitive -i $1 -o lqip/${1%.*}-lqip.${1##*.} -r 32 -s 32 -bg 'avg' -n 150 -v
  fi
  if [ ${1##*.} = "gif" ]
    then
      convert "$1[0]" temp.gif
      primitive -i temp.gif -o lqip/${1%.*}-lqip.jpg -bg 'avg' -r 32 -s 32 -n 150 -v
      rm temp.gif
  fi
}
export -f poly

# polygonized lqip images for all image in a directory
polyAll () {
  for image in *;
    do
      poly "$image"
  done
}
export -f polyAll

alias wiki='cd /var/www/l-o-o-s-e-d/html/knowledge-base/markdown/'
alias markdown-html='generate-md --input ./ --output ../ --layout ../loosed-template'

alias term-phase='raco terminal-phase'
alias cutstream='sudo rsync -avz --remove-source-files dan@loosed.tv:/home/dan/nms/node_modules/node-media-server/public/static/stream/test/* ~/Documents/streams'

# Commands to run at terminal startup:
# ====================================
home && tmux new -s main 2>/dev/null

export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion

alias fire='aafire'
alias pbcopy='xclip -selection clipboard'
alias pbpaste='xclip -selection clipboard -o'
alias fing='sudo fing'
alias keyfinder='xev -event keyboard  | egrep -o "keycode.*\)"'
alias i3config='sudo vim ~/.config/i3/config'

export GOROOT=/usr/local/go
export GOPATH=$HOME/go
export PATH=$GOPATH/bin:$GOROOT/bin:$PATH
```
