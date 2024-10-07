# Vim

## TL;DR
Vim/Neovim is a powerful tool for programmers. It can improve your working efficiency if you master it.

## Popular vim tools
- [lazyvim](https://www.lazyvim.org/)
- [lunarvim](https://www.lunarvim.org/)
- [vim plugin](https://marketplace.visualstudio.com/items?itemName=vscodevim.vim) in vscode
- [ideavim](https://lp.jetbrains.com/ideavim/) in jetbrains

## Basic Operations

There are a few ways for beginners to know what vim is.
[https://vim.rtorr.com](https://vim.rtorr.com)

## Advanced Operations

### \<action>i\<something>

* `ci(:`
* `ci{:`
* `ci[:`
* ....

### Split Window

`Ctrl-w s` and `Ctrl-w v` combinations open a new horizontal or vertical split window respectable for the current buffer

### Move to / Go to

* `gd`: move to local declaration
* `gD`: move to global declaration
* `ctrl-o`: go to older position in jump list
* `ctrl-i`: go to newer position in jump list



### Mark

* `:marks:`list of marks
* `ma` : set current position for mark A (local)
* \`mA\`: set current position for mark A (global)&#x20;
* \`a  : jump to position of mark A
* y\`a : yank text to position of mark A



### Search and replace with Vim (multiple files)

#### Search

```
 :vimgrep /foo/gj **/*
```

* The `**/*` says to search in all files recursively
* The `g` flag says to search for all occurrences in each line (this is actually overkilled here, but it does not hurt either)
* The `j` the flag prevents vim from automatically jumping to the first match

Find the search results:

```
:copen
```

#### Replace

```
:cfdo %s/foo/bar/gc | update
```

* The `%` is a line range that specifies _every_ line
* The `g` flag says to substitute _all_ occurrences in each line
* The `c` flag causes vim to ask you to confirm each replacement individually (you might want to leave this out)

### Low Level of Vim
[https://juejin.cn/post/6990928103023312927](https://juejin.cn/post/6990928103023312927)