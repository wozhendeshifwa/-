[TOC]

# 初始化本地库

# Linux 基础指令

## 文件管理命令

mv xx xx 
cp xx xx 
ls
pwd 展示当前工作目录
.. 表示父文件夹

cd  ../ 

进入评平级目录

```bash
$ for i in a b c
do 
mkdir dir_${i}
done
```

## 文件创建

## 文件修改

## 文件删除

# 查看本地库状态

# 版本查看

版本查看



# 版本穿梭

# 添加暂存区

# 提交本地库

# 分支创建

# 分支查看

# 分支转换

# 分支删除

# 分支合并

# 远程库创建

# 远程库查看

# 远程库拉取

# 远程库推送

# 远程库克隆



git 目前学到的和常用到的命令
初始化本地库 git init
创建新文件 touch <文件名.文件后缀>
编辑文件内容 vim <文件名.文件后缀>
添加暂存区 git add <文件名.文件后缀>
提交本地库 git commit -m “版本信息”
全部提交 git commit -a  （但这项命令执行完之后，往往需要手动输出版本信息）
另一个版本的全部提交 git commit -am "版本信息"
设置远程库 git remote add <远程库名称>  <github上的URL>
查看远程库 git remote -v 或者 git remote 

