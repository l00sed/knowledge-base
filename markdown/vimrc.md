## Full ~/.vimrc file:

Feel free to copy bits and pieces. The full file has many aliases and functions that will be unusable to people other than myself. This is more for my own benefit / backup. I recommend taking a look the [environments](environments) page to get a breakdown of the useful bits and pieces.


```
set omnifunc=emoji#complete

map <C-c> "+y

noremap <silent> <Leader>e :call Emoji()<CR>
function Emoji()
  execute '%s/:\([^:]\+\):/\=emoji#for(submatch(1), submatch(0))/g'
  echo 'Switched out emojis!'
endfunction

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

set clipboard=unnamedplus

map <silent> <C-n> :NERDTreeFocus<CR>

colo corvine
" colo vwilight
" colo anderson

set nocompatible
syntax on
syntax enable
filetype on
filetype plugin indent on

set mouse+=a

if &term =~ '^screen'
  set ttymouse=xterm2
endif

set encoding=utf8
set spelllang=en
set spell
set number
set noswapfile
set softtabstop=2 tabstop=2 shiftwidth=2 expandtab
set wrap
set incsearch
set smartcase
set smartindent
set smarttab
set ignorecase
set laststatus=2
set noshowmode

let g:vimwiki_list = [{'path': '/var/www/l-o-o-s-e-d/html/knowledge-base/markdown/',
      \ 'syntax': 'markdown',
      \ 'template_path': '/var/www/l-o-o-s-e-d/html/knowledge-base/loosed-template/',
      \ 'template_default': 'default',
      \ 'ext': '.md',
      \ 'path_html': '/var/www/l-o-o-s-e-d/html/knowledge-base/',
      \ 'custom_wiki2html': 'vimwiki_markdown',
      \ 'template_ext': '.tpl'}]

let g:tmuxline_powerline_separators=1
let g:airline_powerline_fonts=1
let g:airline#extensions#tabline#enabled=1
let g:airline_theme='luna'

call plug#begin('~/.vim/plugged')
Plug 'vimwiki/vimwiki'
Plug 'vim-airline/vim-airline'
Plug 'vim-airline/vim-airline-themes'
Plug 'edkolev/tmuxline.vim'
Plug 'junegunn/goyo.vim'
Plug 'scrooloose/nerdtree'
Plug 'inkarkat/vim-SpellCheck'
Plug 'inkarkat/vim-ingo-library'
Plug 'etdev/vim-hexcolor'
Plug 'chrisbra/Colorizer'
Plug 'junegunn/vim-emoji'
Plug 'vim-scripts/AutoComplPop'
call plug#end()
```

