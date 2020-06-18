# Introduction

This document is the personal [knowledge base](https://en.wikipedia.org/wiki/Knowledge_base) (KB) of Daniel Tompkins. More and more accounts of people (often programmers) using the KB structure to store frequently referenced information led me to begin building my own.

## VimWiki

For this KB, I'm using [VimWiki](https://github.com/vimwiki/vimwiki), a _**Personal Wiki for Vim**_, which provides a set of hotkeys in the Vim command-line text editor. This plugin allows you to instantly access your KB from anywhere in the terminal, and saves your notes in a .wiki file&mdash; or as common markdown with the following settings in your `~/.vimrc` file: 

```
let g:vimwiki_list = [{'path': '~/vimwiki/',
                      \ 'syntax': 'markdown', 'ext': '.md'}]
```

To search across the KB, you can use

```
:VWS <search pattern>
```

## vim-instant-markdown

To make this even more convenient, I've also installed [vim-instant-markdown](https://github.com/suan/vim-instant-markdown) which (as the name implies) instantly opens a browser window when opening a markdown file with a formatted preview of any italics, blockquotes, ordered-lists, images, code, etc.  

## markdown-styles

For Web, I'm using the _generate-md_ function that's included with *markdown-styles*: 

```
npm install -g markdown-styles
```

For my KB, I use a bash alias to quickly convert all ```.md``` files to ```.html```. The "loosed-template" is a custom variation on the included _jasonm23-dark_ theme: 

```
alias markdown-html='generate-md --input ./ --output ./converted/ --layout loosed-template'
``` 

## Mobile editor

If you want to commit edits to your KB remotely, I use the [Spck Editor](https://spck.io/). It's a nice mobile code editor with built-in GitHub functionality for pulling from or pushing to a remote repo.

If you'd like to setup a similar KB structure, consider checking out the links that are provided on this page. Additionally, here's a wonderful markdown [cheatsheet](https://www.markdownguide.org/cheat-sheet/) if you forget any syntax.
