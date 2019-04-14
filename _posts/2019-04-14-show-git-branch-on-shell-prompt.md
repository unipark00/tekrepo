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
PS1 보다 위에 아래 내용을 추가
```console
source ~/.git-prompt.sh
```
