---
title: Notes For Git
date: 2022-07-14 19:37:00
tags: Git
---

# Notes For Git

## Settings

```bash
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
```