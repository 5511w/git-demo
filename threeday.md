### 1.安装git后验证
```git
$ git --version  #查看版本号
```
### 2.配置git
* 全局配置
```
#配置用户名
$ git config --global user.name "Your Name"
# 配置邮箱
$ git config --global user.email "yourname@example.com"
# 查看所有配置
$ git config --list
```
* 局部配置（针对单个仓库）
```
# 进入仓库目录
$ cd git-demo
# 配置局部用户名
$ git config user.name "Your local Name"
# 配置局部邮箱
$ git config user.email "yourlocalname@example.com"
# 查看局部配置
$ git config --local --list
```
## git流程
### 3.1 初始化仓库
```
# 创建一个项目目录
$ mkdir git-demo
$ cd git-demo
# 初始化git仓库
$ git init
```
* 执行后，当前目录会生成一个隐藏的 .git 文件夹，里面存储了 Git 的所有配置和版本历史
* 你可以用 ls -la 命令查看这个文件夹
### 3.2 查看仓库状态
```
# 创建一个测试文件
$ echo "Hello Git" > test.txt
# 查看仓库状态
$ git status
```
* Untracked files：表示 test.txt 是「未跟踪」的文件，Git 不会管理它的版本
### 3.3 添加文件到暂存区
```
# 将文件添加暂存区
$ git add test.txt
# 再次查看状态
$ git status
```
* Changes to be committed：表示 test.txt 已在暂存区，准备提交
* 你可以用 git add . 将所有修改添加到暂存区
* 用 git add file1 file2 将多个指定文件添加到暂存区
### 3.4 提交文件到本地仓库
```
# 提交文件到本地仓库
$ git commit -m "first commit: add test.txt"
[master (root-commit) 8a7b3c4] first commit: add test.txt
 1 file changed, 1 insertion(+)

```
* -m 后面的是提交信息，必须清晰描述这次提交做了什么（比如「修复登录功能的 bug」）
* 8a7b3c4 是这个提交的唯一 ID（哈希值），用于标识这个版本
### 3.5 查看提交历史
```
$ got log
commit 8a7b3c489e7d345f6a8b9c0d1e2f3a4b5c6d7e8f (HEAD -> master)
Author: Your Name <yourname@example.com>
Date:   Tue Jun 11 14:30:00 2024 +0800
 
    first commit: add test.txt
```
* HEAD -> master 表示当前 HEAD 指向 master 分支的这个提交
* 你可以用 git log --oneline 查看简洁的历史
* 用 git log --graph 查看分支合并的图形化历史
## Git 分支管理
### 4.1 分支基本操作
| 命令 | 功能 |
|------|------|
| git branch | 查看所有分支 |
| git branch `<branch-name>` | 创建新分支|
| git checkout `<branch-name>`|切换分支|
|git switch `<branch-name>`|切换分支（Git 2.23+ 新增，更直观）|
| git branch -d `<branch-name>`|删除已合并的分支|
| git branch -D `<branch-name>`|强制删除未合并的分支|
### 4.2 分支操作实例
```
# 查看当前分支
$ git branch
# * master    * ... 表示当前分支
# 创建一个新的分支 featuer/add-function
$ git branch featuer/add-function
# 切换到新的分支
$ git checkout feature/add-function
# 或 git switch feature/add-function
# 在分支上修改文件
$ echo "def hello():\n  print('hello Git')" > main.py
$ git add main.py
$ git commit -m "add hello function"
# 切换回master分支
$ git checkout master
# 查看master分支的文件
$ cat main.py  # 提示：No such file or directory，因为 main.py 只在 feature 分支
# 将featuer分支合并到master
$ git merge feature/add-function
```
* Fast-forward：表示快速合并，因为 master 分支在 feature 分支创建后没有修改
* 合并后，master 分支包含了 feature 分支的所有修改
### 4.3 分支冲突解决
```
# 合并 feature 分支到 master，出现冲突
$ git checkout master
$ git merge feature/add-function
Auto-merging main.py
CONFLICT (content): Merge conflict in main.py
Automatic merge failed; fix conflicts and then commit the result.
```
步骤
1.
```
$ cat main.py
<<<<<<< HEAD
def hello(name):
=======
def hello(user):
>>>>>>> feature/add-functio
```
* <<<<<<< HEAD：表示当前分支的内容
* =======：分隔符
* `>>>>>>>` feature/add-function：表示要合并的分支的内容

2.手动修改冲突，比如统一为 def hello(name):

3.提交冲突
```
$ git add main.py
$ git commit -m "resolve conflict :use name as parameter"
```
### 4.4 分支的最佳实践
* master/main：主分支，只用于发布稳定版本
* develop：开发分支，集成所有功能分支
* feature/xxx：新功能分支，从 develop 分支创建
* bugfix/xxx：修复 bug 分支，从 develop 或 master 分支创建
* release/xxx：版本发布分支，从 develop 分支创建，用于测试和修复最终 bug
# 注  
学习自csdn玄同765 [链接](https://blog.csdn.net/Yunyi_Chi/article/details/156547092)