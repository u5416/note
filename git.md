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

git branch  查看有什么分支，并且在当前分支前带有*

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

git cherry-pick ，复制一个特定的提交到当前分支
