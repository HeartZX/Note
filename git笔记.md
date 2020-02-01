### Git使用前配置
##### 1. ==配置提交人姓名==：git config --global user.name  提交人姓名
##### 2.==配置提交人邮箱==：git config --global user.email 提交人邮箱
##### 3.==查看git配置信息==：git config --list
  

### 提交步骤
##### 1.git init    ==初始化git仓库==
##### 2. git status ==查看文件状态==
##### 3.git add文件列表 ==追踪文件==
##### 4.git commit -m 提交信息  ==向仓库中提交代码==
##### 5.git log ==查看提交记录==


### 撤销
##### 1.git checkout 文件  ==用暂存区中的文件覆盖工作目录中的文件==
##### 2.git rm --cached 文件  ==将文件从暂存区中删除==
##### 3.git rest --hard commitID ==将仓库中指定的更新记录恢复出来，并且覆盖暂存区和工作目录==


### git 分支命令
##### 1.git branch ==查看分支==
##### 2. git branch 分支名称  ==创建分支==
##### 3. git checkout 分支名称   ==切换分支==
##### 4.git merge 来源分支   ==合并分支==
##### 5.git branch -d 分支名称    ==删除分支 （分支被合并后才允许删除）（-D强制删除==

### 暂时保存更改
##### 1.==存储临时改动==  git stash
##### 2.==恢复改动==： git stash pop

### 本地仓推送到远程仓
##### 1.git push 远程仓库地址  分支名称
##### 2.git push 远程仓库地址别名  分支名称
##### 3.git push -u 远程仓库地址别名 分支名称   -u ==记住推送地址及分支，下次推送只需要输入git push 即可==
##### 4.git remote add 远程仓库地址别名（origin） 远程仓库地址   ==给仓库设置别名==

### 克隆仓库
##### ==克隆远端数据仓库到本地== ：git clone 仓库地址

### 拉取远程仓库中最新的版本
##### git pull 远程仓库地址 分支名称

### 解决冲突
##### 先pull最新的代码到本地 然后push

### GIT忽略清单
##### git忽略清单文件名称：.gitignore