# 1.基本概念

```
工作区：
暂存区
版本库：
版本结构：树结构，树中每个节点代表一个代码版本
```

# 2.git常用命令

```bash
git config --global user,name xxx  # ~/.gitconfig
git config --global user.email xxx # ~/.gitconfig
git init # 初始化
git add xxx #提交暂存区
git add . #全部提交
git status # 查看状态
git commit -m <版本信息> #
git commit -a # 省去 git add xxx 将所有已跟踪的文件 提交版本库
git log # 当前分支历史版本
git log --pretty=oneline # 精简一点
git reset --hard HEAD^ # 回滚到上一个版本，舍弃当前工作区
git reset --hard HEAD^^ # 向上回滚两次
git reset --hard 版本号 # 回滚特定版本
git reflog # 回滚之后可以通过reflog 查看版本号 找回
git reset --hard HEAD^
git restore xxx # 将文件从工作区中中撤回
git restore --staged xxx # 将文件从暂存区中撤回
-----------------------------------------
在使用远程库之前，还需要连接远程库服务器
通过ssh密钥免密登录
---------------- 远程库命令----------------
git remote # 查看远程库
git remote add 别名 URL/SSH # 添加远程库
git push -u <远程库名> <分支> # 本分支和远程库会建立 上游跟踪关系
# 之后在该分支 推送 拉去只需输入 git push/pull
git remote -v # 查看远程库信息
---------------- 分支管理 -----------------
git branch  # 查看所有分支和当前分支
git branch <branch_name> # 分支创建
git merge <branch_name> # 将 <branch_name> 合并当当前分支上
git branch -d <branch_name> # 分支删除
----------------远程库分支管理------------------
git push -d <origin> <branch_name> # 将远程库origin 的 branch_name 分支删除
git pull <origin> <branch_name> # 将远程库 origin 的branch_name 分支与当前分支合并
git branch -r # 查看远程库分支
git pull <origin> <branch_name> # 拉取远程库分支
-------------- 远程库
```

·
