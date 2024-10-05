# git教程

## 创建版本库

1. 首先，选择一个合适的地方，创建一个空目录：

   ```bash
   $ mkdir gitlearning
   $ cd gitlearning
   $ pwd D:/note/gitlearning
   ```
   pwd命令用于显示当前目录。

2. 通过 `git init` 命令把这个目录变成Git可以管理的仓库

写一个文件 readme.txt ,用命令 `git add` 告诉Git，把文件添加到暂存区

   ```bash
   $ git add readme.txt
   ```

用命令`git commit`告诉Git，把文件提交到某一分支

```bash
$ git commit -m "啦啦啦随便写什么啦"
```

可add多个文件，再commit



## 很多很多有关版本变化

- 查看工作区状态使用 `git status` 命令

```bash
$ git status
```

- 查看上次修改 `git diff` 

```bash
$ git diff readme.txt 
```

- `HEAD` 指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭

使用命令  `git reset --hard *****` 。

上一版本

```bash
$ git reset --hard HEAD^
```
对应的那个版本（输入commit_id前几位即可）

```bash
$ git reset --hard 1234a
```

-  `git reflog` 用来记录每一次命令

```bash
$ git reflog
```

- 用 `git diff HEAD -- readme.txt` 命令可以查看工作区和版本库里面最新版本的区别

```bash
$ git diff HEAD -- readme.txt 
```

- 用 `git checkout -- file` 可以丢弃工作区的修改

```bash
$ git checkout -- readme.txt
```

- 用命令 `git reset HEAD <file>` 可以把暂存区的修改撤销掉，重新放回工作区

```bash
$ git reset HEAD readme.txt
```

- 查看历史记录用 `git log` 

```bash
$ git log
```

精简一点用 `git log --pretty=oneline` 

```bash
$ git log --pretty=oneline
```

## 设置SSH Key

1. 创建SSH Key

``` bash 
$ ssh-keygen -t rsa -C "youremail@example.com"
```

输入命令后一路回车。然后用户主目录找到.ssh目录，里面有 `id_rsa` 和 `id_rsa.pub` 两个文件，这两个就是SSH Key的秘钥对， `id_rsa` 是私钥，不能泄露出去， `id_rsa.pub` 是公钥，可以放心地告诉任何人。

2. 登陆github, "Add SSH Key", 在Key文本框里粘贴 `id_rsa.pub` 文件的内容

__就搞定啦！！！！！！！！！！__

为什么GitHub需要SSH Key呢？因为GitHub需要识别出你推送的提交确实是你推送的，而不是别人冒充的，而Git支持SSH协议，所以，GitHub只要知道了你的公钥，就可以确认只有你自己才能推送。

当然，GitHub允许你添加多个Key。假定你有若干电脑，你一会儿在公司提交，一会儿在家里提交，只要把每台电脑的Key都添加到GitHub，就可以在每台电脑上往GitHub推送了。