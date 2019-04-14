---
title: "Show git branch on shell prompt"
search: false
categories:
  - Ubuntu
last_modified_at: 2019-04-14T10:00+09:00
toc: false
---

This post shows how to show git branch on shell prompt.

#### 1) Download git-prompt.sh into $HOME
```console
cd ~
wget https://raw.githubusercontent.com/git/git/master/contrib/completion/git-prompt.sh -O .git-prompt.sh
```
#### 2) Edit ~/.bashrc
PS1 보다 위에 아래 내용을 추가
```console
source ~/.git-prompt.sh
```
