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

## 添加及删除远程库

首先，登陆GitHub，然后，create a new repository 创建一个新的仓库

在本地仓库下运行命令

```bash
$ git remote add origin https://github.com/MrWittgenstein/git_learning.git
```

下一步，就可以把本地库的所有内容推送到远程库上（把当前分支main推送到远程）

```bash
$ git push -u origin main
```

从现在起，只要本地作了提交，就可以通过命令

```bash
$ git push origin main
```

把本地`main`分支的最新修改推送至GitHub



想删除远程库，可以用 `git remote rm <name>` 命令。使用前，建议先用 `git remote -v` 查看远程库信息

## 从远程库克隆

用命令 `git clone` 克隆一个本地库

```bash
$ git clone 地址
```

## 分支管理

### 创建合并分支

我们创建`dev`分支，然后切换到`dev`分支：

```bash
$ git checkout -b dev
```

相当于以下两条命令：

```bash
$ git branch dev
$ git checkout dev
```

__或__  创建并切换到新的 `dev` 分支，可以使用：

```bash
$ git switch -c dev
```



然后 `git add` 和 `git commit` 提交

`dev` 分支的工作完成，我们就可以切换回 `main` 分支：

```bash
$ git checkout main
```

__或__  直接切换到已有的 `main` 分支，可以使用：

```bash
$ git switch main
```



把`dev`分支的工作成果合并到 `main` 分支上：

`git merge` 命令用于合并指定分支到当前分支

```bash
$ git merge dev
Updating e621b84..9358e4c
Fast-forward
 readme.txt | 4 +++-
 1 file changed, 3 insertions(+), 1 deletion(-)
```

上面的`Fast-forward`信息，Git告诉我们，这次合并是“快进模式”，也就是直接把`master`指向`dev`的当前提交，所以合并速度非常快



删除 `dev` 分支了：

```bash
$ git branch -d dev
```



查看分支： `git branch`

### 解决冲突

发生冲突时，用 `cat <文件名>直接查看内容

Git用`<<<<<<<`，`=======`，`>>>>>>>`标记出不同分支的内容

修改解决冲突后再提交

用 `git log --graph` 命令可以看到分支合并图

### 分支管理策略

通常，合并分支时，如果可能，Git会用`Fast forward`模式，但这种模式下，删除分支后，会丢掉分支信息。

如果要强制禁用`Fast forward`模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。

合并`dev`分支时，注意`--no-ff`参数，表示禁用`Fast forward`

```bash
$ git merge --no-ff -m "merge with no-ff" dev
```

### Bug分支

Git提供了一个`stash`功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作

现在，用`git status`查看工作区，就是干净的（除非有没有被Git管理的文件），因此可以放心地创建分支来修复bug。

首先确定要在哪个分支上修复bug，假定需要在`main`分支上修复，就从`main`创建临时分支`issue101`

修改后提交，切换到`main`分支，并合并，删除分支`issue101`

用`git stash list`命令查看stash内容

恢复方法：一是用`git stash apply`恢复，但是恢复后，`stash`内容并不删除，你需要用`git stash drop`来删除

​	另一种方式是用`git stash pop`，恢复的同时把`stash`内容也删了



在`master`分支上修复了bug后，同样的bug，要在`dev`上修复，

Git专门提供了一个`cherry-pick`命令，让我们能复制一个特定的提交到当前分支

```bash
$ git cherry-pick <commit_id>
```

### Feature分支

每添加一个新功能，最好新建一个feature分支，在上面开发，完成后，合并，最后，删除该feature分支

如果要丢弃一个没有被合并过的分支，可以通过`git branch -D <name>`强行删除

## 标签

首先，切换到需要打标签的分支上

敲命令`git tag <name>`就可以打一个新标签

```bash
$ git tag <tagname>
```

给历史提交的commit打标签，找到历史提交的commit id，敲入命令：

```bash
$ git tag <tagname> <commit_id>
```

创建带有说明的标签，用`-a`指定标签名，`-m`指定说明文字：

```bash
$ git tag -a <tagname> -m "*****" <commit_id>
```

用命令`git show <tagname>`可以看到说明文字

命令`git tag`可以查看所有标签

删除标签

```bash
$ git tag -d <tagname>
```

推送某个标签到远程，使用命令`git push origin <tagname>`

一次性推送全部尚未推送到远程的本地标签：

```bash
$ git push origin --tags
```

如果标签已经推送到远程，

要删除远程标签就麻烦一点，先从本地删除：

```bash
$ git tag -d <tagname>
```

然后，从远程删除。删除命令也是push，但是格式如下：

```bash
$ git push origin :refs/tags/<tagname>
```