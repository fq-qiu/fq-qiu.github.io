
写在前面：使用git merge，完美合并，皆大欢喜，若遇到冲突也不用呼天抢地。此时使用vimdiff这款强大的工具，解决conflict手到擒来，如德芙巧克力般丝滑

## 配置merge工具

```sh
git config --global merge.tool vimdiff
git config --global merge.conflictstyle diff3
git config --global mergetool.prompt false

#让git mergetool不再生成备份文件(*.orig)  
git config --global mergetool.keepBackup false
```

配置设置在`~/.gitconfig`

## merge时出现conflict冲突

### 开始解决冲突

```sh
git mergetool
```
对于每个需要merge的文件，会弹出
+--------------------------------+
| LOCAL  |     BASE     | REMOTE |
+--------------------------------+
|             MERGED             |
+--------------------------------+

- `LOCAL` buffer: 当前分支
- `BASE` buffer: 两个分支共同祖先，代表两个分支修改前
- `REMOTE` buffer: 需要合并到当前分支的分支
- `MERGED` buffer: 合并后的，即有冲突的

### vimdiff使用

鼠标移动到`MERGED`窗口，
```sh
:diffget RE # 获取REMOTE的修改到MERGED文件, 忽略大小写
:diffg ba # get from base
:diffg lo # get from local
```

**注意**：通过`diffget`只能选取local, base, remote三种的一种，要想都需要3种或者两种，只能通过修改`MERGED`文件

修改完成后， 保存
```sh
:wqa
```

### 冲突解决完，commit
```sh
git commit
```

删除orig文件
```sh
find . -name "*.orig" | xargs rm
```

### 快捷键，解决敲太多命令的问题

觉得麻烦，配置快捷键, 配置到`~/.vimrc`
```sh
if &diff
    " let mapleader=','
    " let g:mapleader=','
    let g:solarized_diffmode="high"
    map ] ]c
    map [ [c
    map <leader>1 :diffget LOCAL<CR>
    map <leader>2 :diffget BASE<CR>
    map <leader>3 :diffget REMOTE<CR>
    hi DiffAdd    ctermfg=233 ctermbg=LightGreen guifg=#003300 guibg=#DDFFDD gui=none cterm=none
    hi DiffChange ctermbg=white  guibg=#ececec gui=none   cterm=none
    hi DiffText   ctermfg=233  ctermbg=yellow  guifg=#000033 guibg=#DDDDFF gui=none cterm=none
endif
```

### vimdiff的其他命令cheet sheet

```sh
]c      # nect difference
[c      # previous difference
zo      # open folded text
zc      # close folded text
zr      # open all folds
zm      # close all folds
:diffupdate     # re-scan the file for difference
do      # diff obtain
dp      # diff put
:set diffopt+=iwhite    # to avoid whitespace comparison
Ctrl+W+W                # toggle between the diff columns
Ctrl+W+h/j/k/l          # 移动鼠标到不同窗口
:set wrap               # wrap line
:set nowrap
:syn off                # remove colors
```
## 遇到问题

### local分支删除了文件，remote分支修改了文件
```sh
Deleted merge conflict for 'msg/MessageFragment.java':
  {local}: deleted
  {remote}: modified file
Use (m)odified or (d)eleted file, or (a)bort?
```
保留修改后的文件，待merge状态结束后，再处理

## 参考
- [vimdiff合并冲突](https://zhuanlan.zhihu.com/p/67690508)
- [vimdiff颜色晃瞎眼，怎么解决](https://stackoverflow.com/questions/2019281/load-different-colorscheme-when-using-vimdiff)
