## Environments

### ~/.bashrc

#### 1. Pretty Bash Prompt 

```
RCol='\033[0m'
Gre='\033[32m';
Red='\033[31m';
Blu='\033[34m';
Yel='\033[33m';
PS1="${RCol}┌─[\`if [ \$? = 0 ]; then echo "${Gre}"; else echo "${Red}"; fi\`\T\[${Rcol}\] \[${Blu}\]\u@\h\[${RCol}\] \[${Yel}\]\w\[${RCol}\]\$(__git_ps1)]\n└───▶ "
```

#### 2. Quick and handy server alias 

```
npm install -g browser-sync
export ip4=$(ifdata -pa wlp2s0)
alias serve="browser-sync start -s -f . --no-notify --extensions 'html' --host $ip4 --port 9000"
```

#### 3. Tmux 

```
alias td='tmux detach'
alias t0='tmux attach -t 0'
alias t1='tmux attach -t 1'
alias t2='tmux attach -t 2'
alias tmain='tmux attach -t main'
```

#### 4. FFMpeg 

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

#### 5. Connected Android phone mirroring 

```
alias android='scrcpy --max-size 1080 --window-x 1920 --window-y 1080 --window-borderless -S'
alias androidF='android --fullscreen'
```

