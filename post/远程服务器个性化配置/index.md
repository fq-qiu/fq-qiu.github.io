
## 写在前面
我们在开发中经常需要临时使用服务器，服务器没有个性化配置，使用起来很不方面，比如最常见的vim配置，bashrc配置都是默认的。仿佛一下子从电气时代回到了石器时代，那种感觉苦不堪言。下面，就是我解决这个问题的办法。
 
 
## ssh免密登录
具体怎么操作，可以查询ssh的使用。我只列出我的配置`~/.ssh/config`中添加如下
```
Host google
HostName 35.201.201.185
User qfq
Port 22
Identityfile ~/.ssh/cloud.google #私钥
```
 
## 打包本地vim，上传到服务器
我们使用vim大神的一个打包工具[myvim](https://github.com/junegunn/myvim)进行该操作。
该工具不用下载，只要本地机可以连接互联网。该工具打包本地的vim配置和home配置到一个二进制文件，copy该文件到任意系统就可以直接现在本地使用vim一样。我的配置打包后有70M, 通过`scp`上传到服务器用了5min作用，可以接受。
```
bash <(curl -fL https://raw.githubusercontent.com/junegunn/myvim/master/myvim)
```
在当前目录下，会得到一个`vim.$(whoami)`, 我的是vim.muqing。然后上传
```
scp ./vim.muqing google:/home/qfq/vim #在远程服务器重命名为vim
```
其中google为`~/.ssh/config`中的Host，我这里使用了ssh免密登录，当然可以直接输入ip和user，如下
```
scp ./vim.muqing qfq@35.201.201.185:/home/qfq/vim #输入密码即可
```
最后，登录远程服务器，把下列配置添加到远程服务器的`~/.bashrc`中
```
alias vim='~/qfq/vim'
```
可以在远程服务器直接使用vim了
 
## 上传我的bash配置
在本地上传我的配置`.mybashrc`中，操作如下
```
scp ./.mybashrc google:/home/qfq/.mybashrc
```
然后登录到远程服务器，添加`source /home/qfq/.mybashrc`到`~/.bashrc`
 
**附件**: 我的`.mybashrc`内容
```
alias -g ...='../..'
alias -g ....='../../..'
alias -g .....='../../../..'
alias -g ......='../../../../..'
 
alias md='mkdir -p'
alias rd=rmdir
alias d='dirs -v | head -10'
 
# List directory contents
alias lsa='ls -lah'
alias l='ls -lah'
alias ll='ls -lh'
alias la='ls -lAh'
 
# git
alias g='git'
alias ga='git add'
alias gst='git status'
alias gd='git diff'
alias gc='git commit -v'
alias gc!='git commit -v -amend'
alias gp='git push'
alias gl='git pull'
alias gcf='git config --list'
alias gco='git checkout'
alias gcm='git checkout master'
alias gr='git remote'
alias gb='git branch'
alias gm='git merge'
alias glo='git log --oneline --decorate --color'
alias glog='git log --oneline --decorate --color ---graph'
```
 
 
 
 
 
 
## Todo
- 1 把所有的操作集成到一个bash中，做到一键上传操作

