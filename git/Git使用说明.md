# <font color="red">Git使用详细教程</font>
本文主要介绍在Windows上使用git命令的操作方法。

[原文地址](https://www.cnblogs.com/seven-ahz/p/7712125.html)

## 一、windows上使用git
启动git:开始菜单->Git->Git Bash。


```
git config --global user.name "username"
git config --global user.email "useremail"
```
因为Git是分布式版本控制系统，所以需要填写用户名和邮箱作为一个标识。

注意：git config --global表示这台机器上所有的Git仓库都会使用这个配置，也可以对某个仓库指定不同的用户名和邮箱。

## 二、如何操作
### 一：创建版本库
**版本库**：又名仓库，英文名repository。可以简单将仓库理解为一个目录，目录里的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻将文件还原。

**创建版本库**：创建目录->cd到目录中->输入指令git init

```
mkdir notebooks    //创建目录notebooks
cd notebooks       //转到notenooks目录
git init           //将notebooks变成Git可以管理的仓库
```


注意：所有版本控制系统，只能追踪文本文件的改动，比如txt文件，网页，所有程序的代码等，Git也不例外。版本控制系统可以告诉你每次的改动，但是图片，视频等二进制文件，无法跟踪文件内容的变化。

1. 把文件添加到版本库
   - 在notebooks目录中新建记事本文件readme.txt
   - 在readme.txt中添加内容：111111111111
   - 使用命令git add readme.txt将文件添加到暂存区
   - 使用git commit -m "此次提交说明"将文件提交到仓库
   - 使用git status查看是否还有文件未提交
   - 在read.txt中添加新行22222222222
   - 再使用git status会显示readme.txt文件已被修改，但未被提交
   - 使用git diff readme.txt查看文件改了什么
   - 知道修改了什么可以放心提交到仓库了，再使用git add和git commit提交
2. 版本回退
   - 再在readme.txt中添加一行333333333
   - 然后提交到仓库
   - 使用git log查看历史纪录
   - 使用git log --pretty=oneline显示简洁的历史纪录
   - 使用git reset --hard HEAD^把当前版本回退到上一个版本
   - 使用git reset --hard HEAD^^把当前版本回退到上一个版本，以此类推
   - 使用git reset --hard HEAD~100把当前版本回退到前100个版本
   - 当我们向前回退一个版本后，readme.txt中没有3333333这一行了，如何回退到最新版本？
   - 使用git reflog查询所有版本号
   - 使用git reset --hard 版本号，回退到最新版本
   - 注意：git reflog显示版本号是每个版本最前面的十六进制码
   - 注意：git reflog显示版本号从上到下为从新版本到旧版本
3. 理解工作区和暂存区的区别
   - 工作区：就是你电脑上看到的目录
   - 版本库：工作区内的隐藏目录.git就是版本库。版本库中存了很多东西，最重要的就是stage（暂存区），还有Git自动创建的第一个分支master，以及指向master的一个指针HEAD
   - Git提交文件到版本库的两步：
   - git add：把文件添加到暂存区
   - git commit：把暂存区的所有内容提交到当前分支
4. 撤销修改和删除文件操作
   - 撤销操作：
   - 在readme.txt中添加555555555555555
   - git checkout -- readme.txt丢弃工作区的修改：1.若修改后的文件还没有放入暂存区，撤销修改就回到和版本库一模一样的状态；2.若修改后的文件已经放入暂存区，撤销修改就回到添加暂存区后的状态
   - 删除文件：
   - 若在工作去删除一个文件，同样使用git add和git commit来提交
   - 若未提交，也可使用git checkout -- 文件名来撤销删除操作，达到恢复文件的目的
5. 远程仓库
   - 本地Git仓库和github仓库之间的传输是通过SS和加密的，需要一点设置
   - 创建SSH Key
   - 在用户目录下，看有没有.ssh目录及其中是否有id_rsa和id_rsa.pub这两个文件
   - 如果没有，打开命令行，输入：ssh-keygen -t rsa -C "youremail"
   - 生成的文件：id_rsa是私钥，id_rsa.pub是公钥
   - 网页登陆github，setting->SSH Keys->Add SSH Key
   - 填写Title，将id_rsa.pub中的所有内容粘贴到Key中
   - 最后点击Add Key按钮
6. 添加远程库
   - 情景：本地创建一个Git仓库后，又想在github创建一个Git仓库，并希望这两个仓库进行远程同步，这样github的仓库既可以作为备份，又可以和其他人通过该仓库来协作。
   - 登陆github->create a new repo->填写仓库名->create repository
   - 注：现在可以从这个仓库克隆出新的仓库，也可以把一个已有的本地仓库与之关联，然后把本地仓库的内容推送到GitHub仓库
   - 在本地的仓库下运行下面命令关联仓库
   - git remote add origin github仓库的url
   - 使用git push -u origin master把本地库的内容（实际是当前分支master）推送到远程
   - 由于远程库是空的，第一次推送master分支时，加上-u参数，git不但会把本地master分支内容推送到远程库新的master分支，还会把本地master分支和远程的master分支关联起来（此次推送还会要求输入GitHub的用户名和密码）
   - 接下来使用git push origin master就可以完成推送了
   - git clone github远程库的url
7. 创建与合并分支
   - git checkout -b 分支名 表示创建并切换到分支
   - git branch 分支名 创建分支
   - git checkout 分支名 转到分支
   - git branch 查看仓库的分支
   - git branch -d 分支名 删除分支
   - git branch -D 分支名 （分支未合并到master时）删除分支
   - 在分支dev上往readme.txt中添加一行777777777
   - 然后在分支dev上提交
   - 切换到master分支，会发现readme.txt中没有77777777这一行
   - 使用git merge dev可以把dev分支上的内容合并到master分支上
   - 解决分支冲突：
   - 创建分支fenzhi1，并在fenzhi1和master分别在readme.txt中添加不同内容，分别提交
   - 在master分支上合并fenzhi1，会提示冲突信息
   - 使用git status查看状态
   - 解决冲突，重新提交合并
   - 分支管理策略：
   - bug分支
8. 多人协作
   - 当你从远程库克隆时，Git自动把本地master分支和远程master分支对应起来，并且远程库默认名字是origin
   - 使用git remote查看远程库信息
   - 使用git remote -v查看远程库的详细信息
   - 推送分支：
   - 推送分支指把该分支上所有本地提交到远程库中，推送时要指定本地分支，Git就会把该分支推送到远程库对应的远程分支上
   - git push origin master 推送master分支
   - git push origin dev 推送dev分支
   - master分支是主分支，因此要时刻与远程同步
   - 一些修复bug分支一般不推送到远程，可以先合并到主分支上，然后将master推送到远程去
   - 抓取分支：
   - pc1:把dev分支推送到远程
   - pc2:为了在dev分支上工作，使用命令git checkout -b dev origin/dev 创建本地dev分支并把远程origin的分支抓取下来。这一步实际也将两个分支建立链接，后面可以直接使用git pull抓取
   - pc2:在dev分支上修改，使用git push origin dev上传分支
   - pc1:使用git pull origin dev抓取origin上的dev分支到本地
   - pc1:1.使用git branch --set-upstream-to=origin/dev dev建立本地分支和远程分支的链接；2.使用git pull抓取分支（抓所在分支还是抓所有链接分支？）

## <font color="red">Git指令集合</font>
**一、新建仓库**

```
# 在当前目录新建一个Git代码库
$ git init

# 新建一个目录，将其初始化为Git代码库
$ git init [project-name]

# 下载一个项目和它的整个代码历史
$ git clone [url]
```

**二、配置**

```
# 显示当前的Git配置
$ git config --list

# 编辑Git配置文件
$ git config -e [--global]

# 设置提交代码时的用户信息
$ git config [--global] user.name "[name]"
$ git config [--global] user.email "[email address]"
```

**三、增加/删除文件**

```
# 添加指定文件到暂存区
$ git add [file1] [file2] ...

# 添加指定目录到暂存区，包括子目录
$ git add [dir]

# 添加当前目录的所有文件到暂存区
$ git add .

# 添加每个变化前，都会要求确认
# 对于同一个文件的多处变化，可以实现多次提交
$ git add -p

# 删除工作区文件，并且将这次删除放入暂存区
$ git rm [file1] [file2] ...

# 停止追踪指定文件，但该文件会保留在工作区
$ git rm --cached [file]

# 改名文件，并且将这个改名放入暂存区
$ git mv [file-original] [file-renamed]
```

**四、代码提交**
<pre><code>
# 提交暂存区到仓库区
$ git commit -m [message]

# 提交暂存区的指定文件到仓库区
$ git commit [file1] [file2] ... -m [message]

# 提交工作区自上次commit之后的变化，直接到仓库区
$ git commit -a

# 提交时显示所有diff信息
$ git commit -v

# 使用一次新的commit，代替上一次提交
# 如果代码没有任何新的变化，则用来改写上一次commit的提交信息
$ git commit --amend -m [message]

# 重做上一次commit，并包括指定文件的新变化
$ git commit --amend [file1] [file2] ...
</code></pre>

**五、分支**
<pre><code>
# 列出所有本地分支
$ git branch

# 列出所有远程分支
$ git branch -r

# 列出所有本地分支和远程分支
$ git branch -a

# 新建一个分支，但依然停留在当前分支
$ git branch [branch-name]

# 新建一个分支，并切换到该分支
$ git checkout -b [branch]

# 新建一个分支，指向指定commit
$ git branch [branch] [commit]

# 新建一个分支，与指定的远程分支建立追踪关系
$ git branch --track [branch] [remote-branch]

# 切换到指定分支，并更新工作区
$ git checkout [branch-name]

# 切换到上一个分支
$ git checkout -

# 建立追踪关系，在现有分支与指定的远程分支之间
$ git branch --set-upstream [branch] [remote-branch]
$ git branch --set-upstream-to=[branch] [remote-branch]

# 合并指定分支到当前分支
$ git merge [branch]

# 选择一个commit，合并进当前分支
$ git cherry-pick [commit]

# 删除分支
$ git branch -d [branch-name]
$ git branch -D [branch-name]

# 删除远程分支
$ git push origin --delete [branch-name]
$ git branch -dr [remote/branch]
</code></pre>

**六、标签**
<pre><code>
# 列出所有tag
$ git tag

# 新建一个tag在当前commit
$ git tag [tag]

# 新建一个tag在指定commit
$ git tag [tag] [commit]

# 删除本地tag
$ git tag -d [tag]

# 删除远程tag
$ git push origin :refs/tags/[tagName]

# 查看tag信息
$ git show [tag]

# 提交指定tag
$ git push [remote] [tag]

# 提交所有tag
$ git push [remote] --tags

# 新建一个分支，指向某个tag
$ git checkout -b [branch] [tag]
</pre></code>

**七、查看信息**
<pre><code>
# 显示有变更的文件
$ git status

# 显示当前分支的历史版本
$ git log
$ git log --pretty=oneline

# 显示commit历史，以及每次commit发生变更的文件
$ git log --stat

# 搜索提交历史，根据关键字
$ git log -S [keyword]

# 显示某个commit之后的所有变动，每个commit占据一行
$ git log [tag] HEAD --pretty=format:%s

# 显示某个commit之后的所有变动，其“提交说明”必须符合搜索条件
$ git log [tag] HEAD --grep feature

# 显示某个文件的版本历史，包括文件改名
$ git log --follow [file]
$ git whatchanged [file]

# 显示指定文件相关的每一次diff
$ git log -p [file]

# 显示过去5次提交
$ git log -5 --pretty --oneline

# 显示所有提交过的用户，按提交次数排序
$ git shortlog -sn

# 显示指定文件是什么人在什么时间修改过
$ git blame [file]

# 显示暂存区和工作区的差异
$ git diff

# 显示暂存区和上一个commit的差异
$ git diff --cached [file]

# 显示工作区与当前分支最新commit之间的差异
$ git diff HEAD

# 显示两次提交之间的差异
$ git diff [first-branch]...[second-branch]

# 显示今天你写了多少行代码
$ git diff --shortstat "@{0 day ago}"

# 显示某次提交的元数据和内容变化
$ git show [commit]

# 显示某次提交发生变化的文件
$ git show --name-only [commit]

# 显示某次提交时，某个文件的内容
$ git show [commit]:[filename]

# 显示当前分支的最近几次提交
$ git reflog
</pre></code>

**八、远程同步**
<pre><code>
# 下载远程仓库的所有变动
$ git fetch [remote]

# 显示所有远程仓库
$ git remote -v

# 显示某个远程仓库的信息
$ git remote show [remote]

# 增加一个新的远程仓库，并命名
$ git remote add [shortname] [url]

# 取回远程仓库的变化，并与本地分支合并
$ git pull [remote] [branch]

# 上传本地指定分支到远程仓库
$ git push [remote] [branch]

# 强行推送当前分支到远程仓库，即使有冲突
$ git push [remote] --force

# 推送所有分支到远程仓库
$ git push [remote] --all
</pre></code>

**九、撤销**
<pre><code>
# 恢复暂存区的指定文件到工作区
$ git checkout [file]

# 恢复某个commit的指定文件到暂存区和工作区
$ git checkout [commit] [file]

# 恢复暂存区的所有文件到工作区
$ git checkout .

# 重置暂存区的指定文件，与上一次commit保持一致，但工作区不变
$ git reset [file]

# 重置暂存区与工作区，与上一次commit保持一致
$ git reset --hard

# 重置当前分支的指针为指定commit，同时重置暂存区，但工作区不变
$ git reset [commit]

# 重置当前分支的HEAD为指定commit，同时重置暂存区和工作区，与指定commit一致
$ git reset --hard [commit]

# 重置当前HEAD为指定commit，但保持暂存区和工作区不变
$ git reset --keep [commit]

# 新建一个commit，用来撤销指定commit
# 后者的所有变化都将被前者抵消，并且应用到当前分支
$ git revert [commit]

# 暂时将未提交的变化移除，稍后再移入
$ git stash
$ git stash pop
</pre></code>