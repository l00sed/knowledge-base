## Environments

- [.bashrc](#bashrc) 
- [.vimrc](#vimrc) 

<br>

<a id='bashrc'></a> 
### ~/.bashrc 

##### 1. Pretty Bash Prompt 

```
RCol='\033[0m'
Gre='\033[32m';
Red='\033[31m';
Blu='\033[34m';
Yel='\033[33m';
PS1="${RCol}┌─[\`if [ \$? = 0 ]; then echo "${Gre}"; else echo "${Red}"; fi\`\T\[${Rcol}\] \[${Blu}\]\u@\h\[${RCol}\] \[${Yel}\]\w\[${RCol}\]\$(__git_ps1)]\n└───▶ "
```

##### 2. Quick and handy server alias 

```
npm install -g browser-sync
export ip4=$(ifdata -pa wlp2s0)
alias serve="browser-sync start -s -f . --no-notify --extensions 'html' --host $ip4 --port 9000"
```

##### 3. Tmux 

```
alias td='tmux detach'
alias t0='tmux attach -t 0'
alias t1='tmux attach -t 1'
alias t2='tmux attach -t 2'
alias tmain='tmux attach -t main'
```

##### 4. FFMpeg 

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

##### 5. Connected Android phone mirroring 

```
alias android='scrcpy --max-size 1080 --window-x 1920 --window-y 1080 --window-borderless -S'
alias androidF='android --fullscreen'
```

##### 6. Easy/beautiful low-quality image placeholders (LQIP) 

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
### ~/.vimrc 

##### 1. Vimwiki settings 

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

##### 2. Toggle wrap in vim 

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

##### 3. [Lightline](https://github.com/itchyny/lightline.vim) plugin settings 

```
let g:lightline = {
\ 'colorscheme' : 'seoul256'
\ }
```

##### 4. Other loaded plugins

```
call plug#begin('~/.vim/plugged')
Plug 'itchyny/lightline.vim'
Plug 'edkolev/tmuxline.vim'
Plug 'scrooloose/nerdtree'
Plug 'junegunn/goyo.vim'
Plug 'etdev/vim-hexcolor'
Plug 'Yggdroot/indentLine'
Plug 'junegunn/vim-emoji'
Plug 'vim-scripts/AutoComplPop'
Plug 'vimwiki/vimwiki'
Plug 'suan/vim-instant-markdown', {'for': 'markdown'}
call plug#end()
```

