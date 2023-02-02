# Git命令分类整理

### 初始化

* `git init`：把这个目录变成Git可以管理的仓库

### 文件管理

* `git add xxx.txt ` : add需要提交的文件，工作区->暂存区
* `git rm xxx.txt`  : 从版本库中删除xxx.txt文件
* `git mv` : 移动或重命名文件
* `git checkout -- filename` : 撤销修改或回到某一个版本
* `git reset --hard HEAD^` : 版本回退，回到上一个版本，上上一个版本用HEAD^^，或直接加版本号；重置当前分支的指针到指定的commit

### 版本提交

* `git commit -m "message"` : 提交到当前分支，暂存区->版本库
* `git stash` : 存储当前工作现场，可存储未完成的工作，可以暂时在其他分支上工作
  * `git stash pop` : 回复之前存储的工作线程
  * `git stash lish` ：查看stash存储的所有工作现场
  * `git stash apply stash@{0}` : 回复工作现场 stash@{0}
  * `git stash drop` : 对齐stash存储的工作现场

### 查看状态

* `git status` : 查看当前Git仓库的状态
* `git log` : 查看项目的版本历史记录
* `git diff` : 查看两个版本之间的差异

### 远程仓库管理

* `git clone git@github.com:mynam/learngit.git` :  从远程服务器克隆一个Git仓库到本地

* `git fetch` : 从远程仓库获取最新版本到本地，但不合并到当前分支

* `git pull` : 从远程仓库拉去最新的更改到本地并合并到当前分支

  ​	如果在远程库中进行了在线编辑，但没有同步本地库，之后push会报错

  ​	`error: failed to push some refs to` 

  ​	因此这里需要线把远程库中的更新合并到本地苦衷，`--rebase` 的作用是取消掉本地库中刚刚的`commit` ，并把它们接到更新后的版本库中

  ​	`git pull --rebase origin master`  之后再重新 `push` 

* `git push origin master` : 把本地的master分支push到远程库中
* `git remote` ：显示远程库信息
  * `git remote -v` ：显示远程库详细信息
  * `git remote add origin https://github/myname/learngit.git` 添加远程库仓库地址
  * `git remote rm origin https://github/myname/learngit.git` 删除远程仓库地址
  * `git remote show` ： 查看远程仓库信息
  * `git remote rename` ： 重命名远程仓库名称

### 分支管理

* `git branch` ：显示所有分支信息
  * `git branch dev` ：创建dev分支
  * `git branch -d dev` :  删除dev分支
  * `git branch -D dev` :  强行删除一个未被合并过的分支
* `git switch dev` ：切换到dev分支
* `git merge dev` : 把dev分支内容merge（更改合并）到当前分支上
  * `git merge --no-ff -m "merge with no-off" dev` 合并dev，merge到当前分支上，`--no-ff` 表示禁用 Fast forward

### 标签

* `git tag` ：显示所有标签
  * `git tag <tagname>` ：新建一个标签，默认为HEAD，也可以指定一个commit id
  * `git tag v1.0 f52c633` ：指定commit f52c633 建立一个标签 v1.0
  * `git tag -a <tagname> -m "message"` ：`-m` 指定标签信息
  * `git tag -d v1.0` ：删除标签v1.0
  * `git push orgin <tagname> ` ：推送一个本地标签
  * `git push origin --tags` ：推送全部未推送的本地标签
  * `git push origin :refs/tags/<tagname>` ：删除一个远程标签（必须现在本地删除此标签）

### 其他

* `git blame`：查看每一行代码的最后一次修改人和时间
* `git grep`：搜索已经提交到版本库中的文件中的特定字符串
* `git ls-files`：列出所有已跟踪的文件
* `git gc`：压缩Git仓库并除去无效对象

# 搭建git服务器

`sudo apt-get install git` 安装git

`sudo adduser git` 创建一个用户 git

把所有用户的公钥信息都添加到`/home/git/.ssh/authorized_keys` 中，一行一个

`sudo git init --bare sample.git` 在当前目录下初始化git仓库sample.git

`sudo chown -R git:git sample.git` 为了不让用户直接登录到服务器上改工作区，把owner改为git

`git clone git@server:/srv/sample.git` 在自己的电脑上用`git clone`克隆远程仓库

# git忽略文件

git根目录下创建一个特殊的`.gitignore` 文件，把你需要忽略的文件名放进去，git会自动忽略这些文件

常用配置文件表： https://github.com/github/gitignore

