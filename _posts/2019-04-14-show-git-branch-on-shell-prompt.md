---
title: "Show git branch on shell prompt"
search: false
categories:
  - Git
last_modified_at: 2019-04-14T10:00+09:00
---

This post shows how to show git branch on shell prompt.

## 1) Download git-prompt.sh into $HOME
![git-shell](https://github.com/unipark00/tekrepo/blob/master/_posts/20190414_151029.png?raw=true)  
```console
cd ~
wget https://raw.githubusercontent.com/git/git/master/contrib/completion/git-prompt.sh -O .git-prompt.sh
```
## 2) Edit ~/.bashrc
1) PS1 보다 위에 아래 내용을 추가
```console
source ~/.git-prompt.sh
```
2) PS1 설정 변경 : $(__git_ps1) 추가
```console
if [ "$color_prompt" = yes ]; then
    PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$$(__git_ps1) '
else
    PS1='${debian_chroot:+($debian_chroot)}\u@\h:\w\$$(__git_ps1) '
fi
```
## 3) Result
```console
junho85@junho85:~/git_branch_test$ (my-branch) git checkout master
Switched to branch 'master'
junho85@junho85:~/git_branch_test$ (master)
```
