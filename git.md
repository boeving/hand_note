设置用户信息

```
git config --global user.email "hello@itcp.name"

git config --global user.name "itcp"

git config --list   # 查看配置信息
```



---



创建新仓库

```
mkdir myreq
cd myreq
git init --bare     # 加--bare 是创建为远程服务端的祼仓库，不能进行其他git操作，只能在客户端进行操作
# ls -a
.. .git
```



克隆

`git clone git@github.com:michaelliao/gitskills.git`



拉取远程仓库最新版本

```
git pull              # 拉取之前 checkout 的分支上，默认是 master\main
git pull origin dev   # 如果你需要 指定分支拉取可以这样
```



分支管理

```
git branch -a          # 查看当前分支情况

git branch dev         # 创建分支 （git branch 分支名）
git checkout dev       # 切换分支（git checkout 分支名）
git checkout -b dev    # 以上两条命令的简写

分支的合并 
git checkout master    # 先切换回master，只有在 master/main 主分支才能进行合并
git merge dev          # 把dev分支合并到master
git log                # 查看合并的版本情况

git branch -d dev      # 删除分支
```





---



查看现项目目录下的状态（是否有修改要添加或提交的文件）

`git status`



查看文件被修改的情况 (仅用于修改但还未保存到暂存区前可以查看)

`git diff `

添加文件到本地暂存区

```
git add readme.txt  # 指定文件保存
git add --all       # 保存所有的更改文件
```



将暂存区的文件提交到版本仓库中（本地仓库）

```
git commit -m "提交的说明"
git push origin main             # 注意你的远程分支名
```



查看对远程仓库的提交日志

`git log`



----



回退版本

```
git reset --hard HEAD^        # 回退到上个版本
git reset --hard 3628164      # 回退到指定版本，这个版本的commit id是3628164...
```



现在总结一下：



- `HEAD`指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令`git reset --hard commit_id`。

  

- 穿梭前，用`git log`可以查看提交历史，以便确定要回退到哪个版本。

  

- 要重返未来，用`git reflog`查看命令历史，以便确定要回到未来的哪个版本。

```
git reflog
```



---





