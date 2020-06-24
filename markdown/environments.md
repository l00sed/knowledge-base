# Environments

Various dotfile setups and resources for setting up programming environments across various servers remote and local.

- [.bashrc](#bashrc) 
- [.vimrc](#vimrc) 
- [.tmux.conf](#tmux) 
- [.config/i3/config](#i3) 
- [.config/polybar/config](#polybar)
<br>

<a id='bashrc'></a> 
## ~/.bashrc 

### 1. Pretty Bash Prompt 

```
RCol='\033[0m'
Gre='\033[32m';
Red='\033[31m';
Blu='\033[34m';
Yel='\033[33m';
PS1="${RCol}┌─[\`if [ \$? = 0 ]; then echo "${Gre}"; else echo "${Red}"; fi\`\T\[${Rcol}\] \[${Blu}\]\u@\h\[${RCol}\] \[${Yel}\]\w\[${RCol}\]\$(__git_ps1)]\n└───▶ "
```

### 2. Quick and handy server alias 

```
npm install -g browser-sync
export ip4=$(ifdata -pa wlp2s0)
alias serve="browser-sync start -s -f . --no-notify --extensions 'html' --host $ip4 --port 9000"
```

### 3. Tmux 

```
alias td='tmux detach'
alias t0='tmux attach -t 0'
alias t1='tmux attach -t 1'
alias t2='tmux attach -t 2'
alias tmain='tmux attach -t main'
```

### 4. FFMpeg 

```
alias mkvid='ffmpeg -r 1 -f image2 -pattern_type glob -i "*.png" -vf scale=720:480 -filter:v "crop=in_w:480" -vcodec libx264 -crf 0 -pix_fmt yuv444p test.mp4'
alias convgif='ffmpeg -i test.mp4 -vf scale=iw/2:ih/2 -f gif test.gif'
```

```
function mkgif() {
  ffmpeg -pattern_type glob -r 1 -i "$1" -vf "scale='min(640,iw)':'min(480,ih)':force_original_aspect_ratio=decrease,pad=640:480:(ow-iw)/2:(oh-ih)/2" -pix_fmt yuv420p -f gif output.gif
}
export -f mkgif
```

### 5. Connected Android phone mirroring 

```
alias android='scrcpy --max-size 1080 --window-x 1920 --window-y 1080 --window-borderless -S'
alias androidF='android --fullscreen'
```

### 6. Easy/beautiful low-quality image placeholders (LQIP) 

- Must install [GO](https://medium.com/better-programming/install-go-1-11-on-ubuntu-18-04-16-04-lts-8c098c503c5f) 
- Uses [primitive](https://github.com/fogleman/primitive) command-line utility 

```
# polygonized lqip image for single file
function poly() {
  if [ ${1##*.} != "gif" ]
    then
      primitive -i $1 -o lqip/${1%.*}-lqip.${1##*.} -r 64 -s 64 -bg 'avg' -n 150 -v
  fi
  if [ ${1##*.} = "gif" ]
    then
      convert "$1[0]" temp.gif
      primitive -i temp.gif -o lqip/${1%.*}-lqip.jpg -bg 'avg' -r 64 -s 64 -n 150 -v
      rm temp.gif
  fi
}
export -f poly
```

```
# polygonized lqip images for all image in a directory
polyAll () {
  for image in *;
    do
      poly "$image"
  done
}
export -f polyAll
```

<br> 

<a id='vimrc'></a> 
## ~/.vimrc 

### 1. Vimwiki settings 

```
let g:vimwiki_list = [{'path': '~/vimwiki/',
      \ 'syntax': 'markdown',
      \ 'template_path': '~/vimwiki/templates/',
      \ 'template_default': 'default',
      \ 'ext': '.md',
      \ 'path_html': '/var/www/l-o-o-s-e-d/html/knowledge-base/',
      \ 'custom_wiki2html': 'vimwiki_markdown',
      \ 'template_ext': '.tpl'}]
```

### 2. Toggle wrap in vim 

```
noremap <silent> <Leader>tw :call ToggleWrap()<CR>
function ToggleWrap()
  if &wrap
    echo "Wrap OFF"
    setlocal nowrap
    set virtualedit=all
    silent! nunmap <buffer> <Up>
    silent! nunmap <buffer> <Down>
    silent! nunmap <buffer> <Home>
    silent! nunmap <buffer> <End>
    silent! iunmap <buffer> <Up>
    silent! iunmap <buffer> <Down>
    silent! iunmap <buffer> <Home>
    silent! iunmap <buffer> <End>
  else
    echo "Wrap ON"
    setlocal wrap linebreak nolist
    set virtualedit=
    setlocal display+=lastline
    noremap  <buffer> <silent> <Up>   gk
    noremap  <buffer> <silent> <Down> gj
    noremap  <buffer> <silent> <Home> g<Home>
    noremap  <buffer> <silent> <End>  g<End>
    inoremap <buffer> <silent> <Up>   <C-o>gk
    inoremap <buffer> <silent> <Down> <C-o>gj
    inoremap <buffer> <silent> <Home> <C-o>g<Home>
    inoremap <buffer> <silent> <End>  <C-o>g<End>
  endif
endfunction
```

### 3. [vim-airline](https://github.com/vim-airline/vim-airline) plugin settings 
```
let g:airline_powerline_fonts=1
let g:airline#extensions#tabline#enabled=1
let g:airline_theme='luna'
```

You might also checkout [Lightline](https://github.com/itchyny/lightline.vim): 

```
let g:lightline = {
\ 'colorscheme' : 'seoul256'
\ }
```

### 4. Other loaded plugins

```
call plug#begin('~/.vim/plugged')
Plug 'vim-airline/vim-airline'
Plug 'vim-airline/vim-airline-themes'
Plug 'edkolev/tmuxline.vim'
Plug 'scrooloose/nerdtree'
Plug 'junegunn/goyo.vim'
Plug 'etdev/vim-hexcolor'
Plug 'Yggdroot/indentLine'
Plug 'junegunn/vim-emoji'
Plug 'vim-scripts/AutoComplPop'
Plug 'vimwiki/vimwiki'
call plug#end()
```
### 5. Vim colorschemes

Checkout [~/.vim/colors](https://vimcolors.com/) for some awesome `.vim` colorschemes to spruce up your edits!

<br>

<a id='tmux'></a>
## ~/.tmux.conf

### 1. My tmux configuration file

```
# Colored tmux
set -g default-terminal "screen-256color"
set -g history-limit 5000
set -g mouse on
set-option -g status-position bottom
set-option -g status on
set-option -g status-interval 1
source ~/tmuxline
```

### 2. tmuxline file source

```
# This tmux statusbar config was created by tmuxline.vim
# on Tue, 23 Jun 2020

set -g status-justify "left"
set -g status "on"
set -g status-left-style "none"
set -g message-command-style "fg=colour231,bg=colour29"
set -g status-right-style "none"
set -g pane-active-border-style "fg=colour36"
set -g status-style "none,bg=colour23"
set -g message-style "fg=colour231,bg=colour29"
set -g pane-border-style "fg=colour29"
set -g status-right-length "100"
set -g status-left-length "100"
setw -g window-status-activity-style "none"
setw -g window-status-separator ""
setw -g window-status-style "none,fg=colour231,bg=colour23"
set -g status-left "#[fg=colour231,bg=colour36] #S #[fg=colour36,bg=colour23,nobold,nounderscore,noitalics]"
set -g status-right "#[fg=colour29,bg=colour23,nobold,nounderscore,noitalics]#[fg=colour231,bg=colour29] %Y-%m-%d  %H:%M #[fg=colour36,bg=colour29,nobold,nounderscore,noitalics]#[fg=colour231,bg=colour36] #h "
setw -g window-status-format "#[fg=colour231,bg=colour23] #I #[fg=colour231,bg=colour23] #W "
setw -g window-status-current-format "#[fg=colour23,bg=colour29,nobold,nounderscore,noitalics]#[fg=colour231,bg=colour29] #I #[fg=colour231,bg=colour29] #W #[fg=colour29,bg=colour23,nobold,nounderscore,noitalics]"
```

<br>

<a id='i3'></a> 
## ~/.config/i3/config 

### 1. Config file for [i3wm](https://i3wm.org/) 

```
set $mod Mod4

new_window pixel 1
new_float normal

hide_edge_borders none

bindsym $mod+u border none
bindsym $mod+y border pixel 1
bindsym $mod+n border normal

font xft:URWGothic-Book 11

floating_modifier $mod

bindsym $mod+Return exec i3-sensible-terminal

# Window kill command
bindsym $mod+Shift+q kill

# start program launcher
bindsym $mod+d exec --no-startup-id rofi -show run

# change focus
bindsym $mod+j focus left
bindsym $mod+k focus down
bindsym $mod+l focus up
bindsym $mod+semicolon focus right

bindsym $mod+Left focus left
bindsym $mod+Down focus down
bindsym $mod+Up focus up
bindsym $mod+Right focus right

# move focused window
bindsym $mod+Shift+j move left
bindsym $mod+Shift+k move down
bindsym $mod+Shift+l move up
bindsym $mod+Shift+semicolon move right

bindsym $mod+Shift+Left move left
bindsym $mod+Shift+Down move down
bindsym $mod+Shift+Up move up
bindsym $mod+Shift+Right move right

# workspace back and forth (with/without active container)
workspace_auto_back_and_forth yes
bindsym $mod+b workspace back_and_forth
bindsym $mod+Shift+b move container to workspace back_and_forth; workspace back_and_forth

# split orientation
bindsym $mod+h split h;exec notify-send 'tile horizontally'
bindsym $mod+v split v;exec notify-send 'tile vertically'
bindsym $mod+q split toggle

# toggle fullscreen mode for the focused container
bindsym $mod+f fullscreen toggle

# change container layout (stacked, tabbed, toggle split)
bindsym $mod+s layout stacking
bindsym $mod+w layout tabbed
bindsym $mod+e layout toggle split

# toggle tiling / floating
bindsym $mod+Shift+space floating toggle

# change focus between tiling / floating windows
bindsym $mod+space focus mode_toggle

# toggle sticky
bindsym $mod+Shift+s sticky toggle

# focus the parent container
bindsym $mod+a focus parent

# move the currently focused window to the scratchpad
bindsym $mod+Shift+minus move scratchpad

# Show the next scratchpad window or hide the focused scratchpad window.
# If there are multiple scratchpad windows, this command cycles through them.
bindsym $mod+minus scratchpad show

# navigate workspaces next / previous
bindsym $mod+Ctrl+Right workspace next
bindsym $mod+Ctrl+Left workspace prev

# workspaces
set $ws1 1
set $ws2 2
set $ws3 3
set $ws4 4
set $ws5 5
set $ws6 6
set $ws7 7
set $ws8 8

# switch to workspace
bindsym $mod+1 workspace $ws1
bindsym $mod+2 workspace $ws2
bindsym $mod+3 workspace $ws3
bindsym $mod+4 workspace $ws4
bindsym $mod+5 workspace $ws5
bindsym $mod+6 workspace $ws6
bindsym $mod+7 workspace $ws7
bindsym $mod+8 workspace $ws8

# Move focused container to workspace
bindsym $mod+Ctrl+1 move container to workspace $ws1
bindsym $mod+Ctrl+2 move container to workspace $ws2
bindsym $mod+Ctrl+3 move container to workspace $ws3
bindsym $mod+Ctrl+4 move container to workspace $ws4
bindsym $mod+Ctrl+5 move container to workspace $ws5
bindsym $mod+Ctrl+6 move container to workspace $ws6
bindsym $mod+Ctrl+7 move container to workspace $ws7
bindsym $mod+Ctrl+8 move container to workspace $ws8

# Move to workspace with focused container
bindsym $mod+Shift+1 move container to workspace $ws1; workspace $ws1
bindsym $mod+Shift+2 move container to workspace $ws2; workspace $ws2
bindsym $mod+Shift+3 move container to workspace $ws3; workspace $ws3
bindsym $mod+Shift+4 move container to workspace $ws4; workspace $ws4
bindsym $mod+Shift+5 move container to workspace $ws5; workspace $ws5
bindsym $mod+Shift+6 move container to workspace $ws6; workspace $ws6
bindsym $mod+Shift+7 move container to workspace $ws7; workspace $ws7
bindsym $mod+Shift+8 move container to workspace $ws8; workspace $ws8

# Open specific applications in floating mode
for_window [title="alsamixer"] floating enable border pixel 1
for_window [class="Calamares"] floating enable border normal
for_window [class="Clipgrab"] floating enable
for_window [title="File Transfer*"] floating enable
for_window [class="Galculator"] floating enable border pixel 1
for_window [class="GParted"] floating enable border normal
for_window [title="i3_help"] floating enable sticky enable border normal
for_window [class="Lightdm-gtk-greeter-settings"] floating enable
for_window [class="Lxappearance"] floating enable sticky enable border normal
for_window [title="MuseScore: Play Panel"] floating enable
for_window [class="Nitrogen"] floating enable sticky enable border normal
for_window [class="Oblogout"] fullscreen enable
for_window [class="octopi"] floating enable
for_window [title="About Pale Moon"] floating enable
for_window [class="Pamac-manager"] floating enable
for_window [class="Pavucontrol"] floating enable
for_window [class="qt5ct"] floating enable sticky enable border normal
for_window [class="Qtconfig-qt4"] floating enable sticky enable border normal
for_window [class="Simple-scan"] floating enable border normal
for_window [class="(?i)System-config-printer.py"] floating enable border normal
for_window [class="Thus"] floating enable border normal
for_window [class="Timeset-gui"] floating enable border normal
for_window [class="(?i)virtualbox"] floating enable border normal
for_window [class="Xfburn"] floating enable

# switch to workspace with urgent window automatically
for_window [urgent=latest] focus

# reload the configuration file
bindsym $mod+Shift+c reload

# restart i3 inplace (preserves your layout/session, can be used to upgrade i3)
bindsym $mod+Shift+r restart

# exit i3 (logs you out of your X session)
bindsym $mod+Shift+e exec "i3-nagbar -t warning -m 'You pressed the exit shortcut. Do you really want to exit i3? This will end your X session.' -b 'Yes, exit i3' 'i3-msg exit'"

# Set shut down, restart and locking features
bindsym $mod+0 mode "$mode_system"
set $mode_system (l)ock, (e)xit, switch_(u)ser, (s)uspend, (h)ibernate, (r)eboot, (Shift+s)hutdown
mode "$mode_system" {
    bindsym l exec --no-startup-id i3exit lock, mode "default"
    bindsym s exec --no-startup-id i3exit suspend, mode "default"
    bindsym u exec --no-startup-id i3exit switch_user, mode "default"
    bindsym e exec --no-startup-id i3exit logout, mode "default"
    bindsym h exec --no-startup-id i3exit hibernate, mode "default"
    bindsym r exec --no-startup-id i3exit reboot, mode "default"
    bindsym Shift+s exec --no-startup-id i3exit shutdown, mode "default"

    # exit system mode: "Enter" or "Escape"
    bindsym Return mode "default"
    bindsym Escape mode "default"
}

# Resize window (you can also use the mouse for that)
bindsym $mod+r mode "resize"
mode "resize" {
        # These bindings trigger as soon as you enter the resize mode
        # Pressing left will shrink the window’s width.
        # Pressing right will grow the window’s width.
        # Pressing up will shrink the window’s height.
        # Pressing down will grow the window’s height.
        bindsym j resize shrink width 5 px or 5 ppt
        bindsym k resize grow height 5 px or 5 ppt
        bindsym l resize shrink height 5 px or 5 ppt
        bindsym semicolon resize grow width 5 px or 5 ppt

        # same bindings, but for the arrow keys
        bindsym Left resize shrink width 10 px or 10 ppt
        bindsym Down resize grow height 10 px or 10 ppt
        bindsym Up resize shrink height 10 px or 10 ppt
        bindsym Right resize grow width 10 px or 10 ppt

        # exit resize mode: Enter or Escape
        bindsym Return mode "default"
        bindsym Escape mode "default"
}

# Autostart applications
exec --no-startup-id nitrogen --restore; sleep 1; compton -b
exec --no-startup-id nm-applet
# exec --no-startup-id xfce4-power-manager
exec --no-startup-id pamac-tray
exec --no-startup-id clipit
exec_always --no-startup-id ff-theme-util
exec_always --no-startup-id fix_xcursor
exec_always --no-startup-id $HOME/.config/polybar/i3wmthemer_bar_launch.sh

# Theme colors
client.focused #56737a #1e1e20 #56737a #56737a #56737a
client.focused_inactive #56737a #1e1e20 #56737a #2c5159 #2c5159
client.unfocused #56737a #1e1e20 #56737a #2c5159 #2c5159
client.urgent #56737a #1e1e20 #56737a #2c5159 #2c5159
client.placeholder #56737a #1e1e20 #56737a #2c5159 #2c5159

client.background #1e1e20

# Gaps
gaps inner 10
gaps outer -4

smart_gaps on

# Press $mod+Shift+g to enter the gap mode. Choose o or i for modifying outer/inner gaps. Press one of + / - (in-/decrement for current workspace) or 0 (remove gaps for current workspace). If you also press Shift with these keys, the change will be global for all workspaces.
set $mode_gaps Gaps: (o) outer, (i) inner
set $mode_gaps_outer Outer Gaps: +|-|0 (local), Shift + +|-|0 (global)
set $mode_gaps_inner Inner Gaps: +|-|0 (local), Shift + +|-|0 (global)
bindsym $mod+Shift+g mode "$mode_gaps"

mode "$mode_gaps" {
        bindsym o      mode "$mode_gaps_outer"
        bindsym i      mode "$mode_gaps_inner"
        bindsym Return mode "default"
        bindsym Escape mode "default"
}
mode "$mode_gaps_inner" {
        bindsym plus  gaps inner current plus 5
        bindsym minus gaps inner current minus 5
        bindsym 0     gaps inner current set 0

        bindsym Shift+plus  gaps inner all plus 5
        bindsym Shift+minus gaps inner all minus 5
        bindsym Shift+0     gaps inner all set 0

        bindsym Return mode "default"
        bindsym Escape mode "default"
}
mode "$mode_gaps_outer" {
        bindsym plus  gaps outer current plus 5
        bindsym minus gaps outer current minus 5
        bindsym 0     gaps outer current set 0

        bindsym Shift+plus  gaps outer all plus 5
        bindsym Shift+minus gaps outer all minus 5
        bindsym Shift+0     gaps outer all set 0

        bindsym Return mode "default"
        bindsym Escape mode "default"
}

# set power-manager and volume control
exec --no-startup-id mate-power-manager

bindsym XF86AudioRaiseVolume exec --no-startup-id amixer -D pulse sset Master 5%+ unmute
bindsym XF86AudioLowerVolume exec --no-startup-id amixer -D pulse sset Master 5%- unmute
bindsym XF86AudioMute exec --no-startup-id amixer -D pulse sset Master toggle

# touchpad on and off controller on laptop with Fn+<touchpad control functional key>
bindsym XF86TouchpadOn exec --no-startup-id synclient Touchpadoff=0
bindsym XF86TouchpadOff exec --no-startup-id synclient Touchpadoff=1
```
<br>

<a id='polybar'></a>
## ~/.config/polybar/config

### 1. [Polybar](https://github.com/polybar/polybar) is a customizable taskbar for i3wm

```
[bar/i3wmthemer_bar]
width = 100%
height = 27
radius = 0
fixed-center = true

background = #1e1e20
foreground = #c5c8c6

line-size = 3
line-color =

border-size = 0
border-color =

padding-left = 0
padding-right = 2

module-margin-left = 1
module-margin-right = 2

font-0 = "Source Code Pro Semibold:size=10;1"
font-1 = "Font Awesome 5 Free:style=Solid:size=10;1"
font-2 = "Font Awesome 5 Brands:size=10;1"

modules-left = i3
modules-center = date
modules-right = wlan battery powermenu

tray-position =
;tray-padding =
wm-restack = i3
override-redirect = true

cursor-click = pointer
cursor-scroll = ns-resize

[module/i3]
type = internal/i3
format = <label-state> <label-mode>
index-sort = true
wrapping-scroll = false

label-mode-padding = 2
label-mode-foreground = #1e1e20
label-mode-background = #56737a

label-focused = %index%
label-focused-background = #2c5159
label-focused-foreground = #6b7443
label-focused-padding = 2

label-unfocused = %index%
label-unfocused-background = #56737a
label-unfocused-foreground = #1e1e20
label-unfocused-padding = 2

label-visible = %index%
label-visible-background = #56737a
label-visible-foreground = #1e1e20
label-visible-padding = 2

label-urgent = %index%
label-urgent-background = #BA2922
label-urgent-padding = 2

[module/wlan]
type = internal/network
interface = wlp2s0
interval = 3.0

format-connected = <ramp-signal> <label-connected>
format-connected-foreground = #80969b
format-connected-background = #1e1e20
format-connected-padding = 2
label-connected = %essid%

format-disconnected =

ramp-signal-0 = 
ramp-signal-1 = 
ramp-signal-2 = 
ramp-signal-3 = 
ramp-signal-4 = 
ramp-signal-foreground = #1e1e20

[module/eth]
type = internal/network
interface = enp0s3
interval = 3.0

format-connected-padding = 2
format-connected-foreground = #80969b
format-connected-background = #1e1e20
format-connected-prefix = " "
format-connected-prefix-foreground = #80969b
label-connected = %local_ip%

format-disconnected =

[module/date]
type = internal/date
interval = 5

date =
date-alt = " %Y-%m-%d"

time = %I:%M
time-alt = %I:%M:%S

format-prefix = 
format-foreground = #1e1e20
format-background = #416269
format-padding = 2

label = %date% %time%

[module/powermenu]
type = custom/menu

expand-right = true

format-spacing = 1

label-open = 
label-open-foreground = #56737a
label-close =  cancel
label-close-foreground = #56737a
label-separator = |
label-separator-foreground = #80969b

menu-0-0 = reboot
menu-0-0-exec = menu-open-1
menu-0-1 = power off
menu-0-1-exec = menu-open-2
menu-0-2 = log off
menu-0-2-exec = menu-open-3

menu-1-0 = cancel
menu-1-0-exec = menu-open-0
menu-1-1 = reboot
menu-1-1-exec = reboot

menu-2-0 = power off
menu-2-0-exec = poweroff
menu-2-1 = cancel
menu-2-1-exec = menu-open-0

menu-3-0 = log off
menu-3-0-exec = i3 exit logout
menu-3-1 = cancel
menu-3-1-exec = menu-open-0

[module/battery]
type = internal/battery

; This is useful in case the battery never reports 100% charge
full-at = 99

battery = BAT0
adapter = AC

poll-interval = 5
format-charging = <animation-charging> <label-charging>
format-discharging = <animation-discharging> <label-discharging>
label-charging = %percentage%%
label-discharging = %percentage%%
label-full = Charged

animation-charging-0 = ▁ 
animation-charging-1 = ▂
animation-charging-2 = ▃
animation-charging-3 = ▄
animation-charging-4 = ▅
animation-charging-5 = ▆
animation-charging-6 = ▇ 
animation-charging-framerate = 1000

animation-discharging-0 = ▇ 
animation-discharging-1 = ▆
animation-discharging-2 = ▅
animation-discharging-3 = ▄
animation-discharging-4 = ▃
animation-discharging-5 = ▂
animation-discharging-6 = ▁ 
animation-discharging-framerate = 1000

[settings]
screenchange-reload = true

[global/wm]
margin-top = 0
margin-bottom = 0
```

### 2. Polybar reloading script

```
#!/bin/env sh

pkill polybar
sleep 1;
polybar i3wmthemer_bar &
```

