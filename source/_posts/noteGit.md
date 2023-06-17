---
title: Notes For Git
date: 2022-07-14 19:37:00
tags: Git notes
---

# Notes For Git

## Settings

``` bash
$ git config --list --show-origin
file:D:/Git/etc/gitconfig       diff.astextplain.textconv=astextplain
file:D:/Git/etc/gitconfig       filter.lfs.clean=git-lfs clean -- %f
file:D:/Git/etc/gitconfig       filter.lfs.smudge=git-lfs smudge -- %f
file:D:/Git/etc/gitconfig       filter.lfs.process=git-lfs filter-process
file:D:/Git/etc/gitconfig       filter.lfs.required=true
file:D:/Git/etc/gitconfig       http.sslbackend=openssl
file:D:/Git/etc/gitconfig       http.sslcainfo=D:/Git/mingw64/ssl/certs/ca-bundle.crt
file:D:/Git/etc/gitconfig       core.autocrlf=true
file:D:/Git/etc/gitconfig       core.fscache=true
file:D:/Git/etc/gitconfig       core.symlinks=false
file:D:/Git/etc/gitconfig       pull.rebase=false
file:D:/Git/etc/gitconfig       credential.helper=manager
file:C:/Users/[]/.gitconfig        user.name=[]
file:C:/Users/[]/.gitconfig        user.email=[]@[]

$ git config --global user.name "a"
$ git config --global user.email "@"


$ git config --global core.editor "code --wait"
$ git config --global core.editor "code --wait"

$ git config --list
$ git config user.name

$ git config --global init.defaultBranch main

$ git help <verb>
$ git <verb> --help
$ man git-<verb>
```

There levels: `--local` > `--global` > `--system`

On Windows systems, Git looks for the `.gitconfig` file in the $HOME directory (C:\Users\$USER for most people). It also still looks for `[path]/etc/gitconfig`, although it’s relative to the MSys root, which is wherever you decide to install Git on your Windows system when you run the installer. 

## Basics

### initializing

```bash
$ cd D:/my_project
$ git init
```

to control a repository with git and this will create a file named `.git`

we can initialize a repository like this:

```bash
$ git add *.c
$ git add LICENSE
$ git commit -m 'Initial project version'
```

### cloning

we can clone a repository like this:

```bash
$ git clone https://github.com/
```

if want a new name 

```bash
$ git clone <url> name
```

and there are other protocols that can be chosen(SSH)

### recording changes

two states : tracked or untracked. To CHECK this :

```bash
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
nothing to commit, working tree clean

$ git status -s
M README
MM Rakefile
A lib/git.rb
M lib/simplegit.rb
?? LICENSE.txt
```

There are two columns to the output — the left-hand column indicates the status of the staging area and the right-hand column indicates the status of the working tree. 

### Ignoring Files

```bash
- .gitignore
$ cat .gitignore
*.[oa]
*~
# ignore all .a files
*.a
# but do track lib.a, even though you're ignoring .a files above
!lib.a
# only ignore the TODO file in the current directory, not subdir/TODO
/TODO
# ignore all files in any directory named build
build/
# ignore doc/notes.txt, but not doc/server/arch.txt
doc/*.txt
# ignore all .pdf files in the doc/ directory and any of its subdirectories
doc/**/*.pdf
```

### Viewing Your Staged and Unstaged Changes

`git diff` shows you the exact lines added and removed — the patch, as it were

### Commit Your Changes

`git commit` 这样需要打开编辑器输入

`git commmit -m "There is message"`

```bash
$ git commit -m "Story 182: fix benchmarks for speed"
[master 463dc4f] Story 182: fix benchmarks for speed
2 files changed, 2 insertions(+)
create mode 100644 README
```

`git commit -a` 可以跳过stage阶段

### Removing Files

remove it from your tracked files, and also removes the file from your working directory so you don't see it as an untracked file the next time around

`rm` 直接删除，但是不stage

`git rm` 删除，但是已经stage

If you modified the file or had already added it to the staging area, you must force the removal with the `-f` option. 

to keep the file in your working tree but remove it from your staging area.

`git rm --cached README`

### Moving Files

重命名

```bash
$ git mv README.md README
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes to be committed:
(use "git reset HEAD <file>..." to unstage)
renamed: README.md -> README
```

相当于

```bash
$ mv README.md README
$ git rm README.md
$ git add README
```

### Viewing the Commit History

`git log` 查看commit history

`git log -patch` 显示每次的commit的不同之处

`git log -2` 只看最近两条