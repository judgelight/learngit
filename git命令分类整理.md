# Git命令分类整理

### 初始化

* `git init`：把这个目录变成Git可以管理的仓库

### 文件管理

* `git add xxx.txt ` : add需要提交的文件，工作区->暂存区
* `git reset` : 放弃所有在暂存区的修改
  * `git reset <filename>` : 放弃add到暂存区<filename>的修改
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
* `git diff` : 查看两个版本之间的差异，可以查看工作区与仓库代码的差别
  * 按 `q` 退出

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

# 一个协同开发过程中的 Git 使用流程如下：

1. 克隆仓库：使用命令 `git clone [repository_url]` 将远程仓库克隆到本地。

2. 创建分支：使用命令 `git checkout -b [branch_name]` 创建一个新的分支并切换到该分支。

3. 修改代码：在本地修改代码并保存。

4. 提交代码：使用命令 `git add [file_name]` 将修改后的文件添加到暂存区，然后使用命令 `git commit -m "[commit_message]"` 将代码提交到当前分支。

5. 同步代码：使用命令 `git pull origin [branch_name]` 从远程仓库获取最新代码，并使用命令 `git push origin [branch_name]` 将本地代码推送到远程仓库。

6. 合并代码：使用命令 `git checkout [main_branch_name]` 切换到主分支，然后使用命令 `git merge [branch_name]` 将分支合并到主分支。

7. 推送代码：使用命令 `git push origin [main_branch_name]` 将合并后的代码推送到远程仓库。

这是一个大致的流程，根据实际需求可以做出相应的调整。

 # git pull使用技巧
 如果您不想在每次拉取时都输入用户名和密码，则可以使用以下步骤配置 Git 使用凭据存储：

1. 运行以下命令：`git config --global credential.helper store`。这将告诉 Git 将凭据存储在本地磁盘上。
2. 接下来，运行以下命令：`git pull`。当您首次运行此命令时，Git 会询问您的用户名和密码，并将其存储在磁盘上。
3. 以后，您就可以使用简单的 `git pull` 命令拉取更新，而不需要再次输入用户名和密码。

请注意，如果您的计算机被他人访问，他们将能够使用保存在磁盘上的凭据访问您的 Git 仓库。因此，请务必保护您的计算机，并在不再需要时删除存储的凭据。
