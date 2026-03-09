# git

## 创建版本库

git init    把当前目录初始化为本地仓库

git add 文件名     把文件添加进暂存区
git add .   所有文件

git commit -m "改动说明"    把暂存区保存到本地仓库

+ Git命令必须在Git仓库目录内执行（git init除外），在仓库目录外执行是没有意义的。
+ 添加某个文件时，该文件必须在当前目录下存在，用ls或者dir命令查看当前目录的文件，看看文件是否存在。

## 时光机穿梭

git status  查看仓库状态
git diff 文件名   查看不同

### 版本回退

git log 查看提交日志
如果嫌输出信息太多，看得眼花缭乱的，可以试试加上`pretty=oneline`参数

在Git中，用HEAD表示当前版本，上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100。

git reset XX1 XX2

XX1:

|选项|功能|
|:---:|:---:|
|--hard|回退到上个版本的已提交状态|
|--soft|回退到上个版本的未提交状态|
|--mixed|回退到上个版本已添加但未提交的状态|

XX2: HEAD之类的或者GPL

git reflog  查看历史命令

### 工作区与暂存区

### 管理修改

### 撤销修改

git checkout -- 文件名  把文件在工作区的修改全部撤销

+ 一种是自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；
+ 一种是已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

总之，就是让这个文件回到最近一次git commit或git add时的状态。

git reset HEAD 文件名 可以把暂存区的修改撤销掉，重新放回工作区。

### 删除文件

1. git rm 文件名
1. git commit

## 远程仓库

由于你的本地Git仓库和GitHub仓库之间的传输是通过SSH加密的，所以，需要一点设置：

第1步：创建SSH Key。在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有id_rsa和id_rsa.pub这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：

$ ssh-keygen -t rsa -C "`youremail@example.com`"
你需要把邮件地址换成你自己的邮件地址，然后一路回车，使用默认值即可.

如果一切顺利的话，可以在用户主目录里找到.ssh目录，里面有id_rsa和id_rsa.pub两个文件，这两个就是SSH Key的秘钥对，id_rsa是私钥，不能泄露出去，id_rsa.pub是公钥，可以放心地告诉任何人。

第2步：登陆GitHub，打开“Account settings”，“SSH Keys”页面：

然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴id_rsa.pub文件的内容：

点“Add Key”，你就应该看到已经添加的Key。

为什么GitHub需要SSH Key呢？因为GitHub需要识别出你推送的提交确实是你推送的，而不是别人冒充的，而Git支持SSH协议，所以，GitHub只要知道了你的公钥，就可以确认只有你自己才能推送。

### 添加远程库

git remote add origin 仓库名  建立联系

git push 推送

git push -u origin 分支名

-u:--set-upstream   设置上游分支

如果不-u的话，后续要写全(origin 分支名),自动补全与状态信息缺失。

如果忘记，可在之后的任意时间手动设置
> git branch -u origin/分支名

~~由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。~~

git pull 拉取

### 删除远程库

git remote rm origin

### 克隆远程库

git clone 仓库名

## 分支管理

### 创建与合并分支

git checkout -b 分支名
创建并切换到分支
相当于

1. git branch 分支名
1. git checkout 分支名

git switch  -c 分支名
创建并切换到分支
相当于

1. git branch 分支名
1. git switch 分支名

git branch  查看本地自己的分支，并且在当前分支前带有*
git branch -r 查看本地缓存的origin分支
git branch -a 查看本地全部分支

git merge 分支名    命令用于合并指定分支到当前分支

Fast-forward: 快进模式，也就是直接把master指向dev的当前提交

git branch -d 分支名    删除分支

### 解决冲突

解决冲突就是把Git合并失败的文件手动编辑为我们希望的内容，再提交。

用git log --graph命令可以看到分支合并图

### 分支管理策略

通常，合并分支时，如果可能，Git会用Fast forward模式，但这种模式下，删除分支后，会丢掉分支信息。

如果要强制禁用Fast forward模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。

git merge --no-ff -m "XXX"  分支名

### bug分支

git stash   暂存

git stash list  看存在哪些stash

+   1. git stash apply（恢复）
    1. git stash drop（删除）
+   git stash pop（恢复并删除）

后面加上 "stash@{<index>}"

git cherry-pick ，复制一个特定的提交到当前分支

### feature分支

如果要丢弃一个没有被合并过的分支，可以通过git branch -D 分支名强行删除

### 多人协作

查看远程库 git remote 或者用git remote -v显示更详细的信息

显示了可以抓取和推送的origin的地址。如果没有推送权限，就看不到push的地址。

#### 推送分支

推送分支，就是把该分支上的所有本地提交推送到远程库。推送时，要指定本地分支，这样，Git就会把该分支推送到远程库对应的远程分支上。

本地新建的分支如果不推送到远程，对其他人就是不可见的。

#### 抓取分支

git pull

#### 小结

多人协作的工作模式通常是这样：

首先，可以尝试用git push origin <branch-name>推送自己的修改；
如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；
如果合并有冲突，则解决冲突，并在本地提交；
没有冲突或者解决掉冲突后，再用git push origin <branch-name>推送就能成功！

如果git pull提示no tracking information，则说明本地分支和远程分支的链接关系没有创建，用命令git branch --set-upstream-to <branch-name> origin/<branch-name>。

### Rebase

git rebase 

rebase操作可以把本地未push的分叉提交历史整理成直线；
rebase的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比。

缺点是本地的分叉提交已经被修改过了。

## 标签管理

### 创建标签

1. 切换到要打标签的分支
1. git tag 标签名

默认标签是打在最新提交的commit上的。

如果忘了打标签，只需要找到要打标签的commit id。
git tag 标签名 commit id

git tag 查看所有标签（字母排列）

git show tag_name   查看标签信息

还可以创建带有说明的标签，用-a指定标签名，-m指定说明文字：

$ git tag -a v0.1 -m "version 0.1 released" commit id

### 操作标签

因为创建的标签都只存储在本地，不会自动推送到远程。所以，打错的标签可以在本地安全删除。

#### 推送标签

+ git push origin 标签名
+ git push origin --tag

#### 删除标签

##### 删除本地标签

git tag -d 标签名

##### 删除远程标签

1. 先删本地
1. 删远程：git push origin :refs/tags/标签名

## 自定义git

<!-- ### 颜色

git config --global color.ui true -->

### 忽略特殊文件

在Git工作区的根目录下创建一个特殊的.gitignore文件，然后把要忽略的文件名填进去，Git就会自动忽略这些文件。

不需要从头写.gitignore文件，GitHub已经为我们准备了各种配置文件，只需要组合一下就可以使用了。所有配置文件可以直接在线浏览：[GitHub/gitignore](https://github.com/github/gitignore)

忽略文件的原则是：

1. 忽略操作系统自动生成的文件，比如缩略图等；
1. 忽略编译生成的中间文件、可执行文件等，也就是如果一个文件是通过另一个文件自动生成的，那自动生成的文件就没必要放进版本库，比如Java编译产生的.class文件；
1. 忽略你自己的带有敏感信息的配置文件，比如存放口令的配置文件。

git add -f 文件名   强制添加文件

git check-ignore -v 文件名  查看冲突在哪里发生

```
# 排除所有.开头的隐藏文件:
.*
# 排除所有.class文件:
*.class

# 不排除.gitignore和App.class:
!.gitignore
!App.class
```

.gitignore文件放在哪个目录下，就对哪个目录（包括子目录）起作用。

### 配置别名

git config --global alias.别名 操作(如有空格则带上一对单引号)

--global 针对当前用户 不加则只针对当前仓库

#### 配置文件

每个仓库的Git配置文件都放在.git/config文件。

而当前用户的Git配置文件放在用户主目录下的一个隐藏文件.gitconfig。

别名就在[alias]后面，要删除别名，直接把对应的行删掉即可。

## 搭建git服务器

https://liaoxuefeng.com/books/git/customize/server/index.html

搭建Git服务器需要准备一台运行Linux的机器（Ubuntu或Debian）
有sudo权限的用户账号

1. 安装git：sudo apt install git
1. 创建一个git用户，用来运行git服务：sudo adduser git
1. 创建证书登录：收集所有需要登录的用户的公钥，就是他们自己的id_rsa.pub文件，把所有公钥导入到/home/git/.ssh/authorized_keys文件里，一行一个。
1. 初始化Git仓库：
    1. 先选定一个目录作为Git仓库，假定是/srv/sample.git，在/srv目录下输入命令：sudo git init --bare sample.git。Git就会创建一个裸仓库，裸仓库没有工作区，因为服务器上的Git仓库纯粹是为了共享，所以不让用户直接登录到服务器上去改工作区，并且服务器上的Git仓库通常都以.git结尾。
    2. 然后，把owner改为git：sudo chown -R git:git sample.git
1. 禁用shell登录：
    出于安全考虑，第二步创建的git用户不允许登录shell，这可以通过编辑/etc/passwd文件完成。

    找到类似下面的一行：git:x:1001:1001:,,,:/home/git:/bin/bash
    改为：git:x:1001:1001:,,,:/home/git:/usr/bin/git-shell

    这样，git用户可以正常通过ssh使用git，但无法登录shell，因为我们为git用户指定的git-shell每次一登录就自动退出。
1. 克隆远程仓库：现在，可以通过git clone命令克隆远程仓库了，在各自的电脑上运行：git clone git@server:/srv/sample.git

### 管理密钥

如果团队很小，把每个人的公钥收集起来放到服务器的/home/git/.ssh/authorized_keys文件里就是可行的。如果团队有几百号人，就没法这么玩了，这时，可以用[Gitosis](https://github.com/res0nat0r/gitosis)来管理公钥。

## https://git-scm.com/