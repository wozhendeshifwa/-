[TOC]

# ssh是什么？

ssh secure shell 加密网络协议，用于在互联网上安全地访问远程计算机、执行命令、传输文件等

# 首次首次远程服务器

```bash
ssh user@HostName
# 第一次连接服务器，会出现提示选项信息，输入yes即可
# 接着，输入密码
# 本地 ~/.ssh/known_host 会多了新服务器信息
```

# config配置文件 —— ssh 个性化连接参数配置

```bash
cd ~/.ssh/
vim config
Host mysever1
	HostName 123.57.180.12 # 公网ip
	User root   # 根用户 
	Port 22 # 默认值
# 多个服务器配置
Host mysever2
	HostName XXX
	User XXX
	Port 22 # 默认值
...
```

# 远程服务器密钥登录

## 1.ssh 本地生成密钥对

```bash
ssh-keygen # 密钥生成
持续回车
# 本地 ~/.ssh/ 文件夹之下多了两个文件
id_rsa # 私钥  
id_rsa_pub # 公钥
```

## 2.将公钥部署到远程服务器

```bash
ssh-copy-id   # ssh-copy-id git@github.com
# 输入远程服务器密码
# 公钥会自动追加到 远程服务器 ~/.ssh/authorized_keys
```

# scp 基于 ssh 协议加密复制文件/目录

```bash
# 从本地 复制文件到 远程服务器
scp source1 mysever:~/xxx
scp source1 source2 mysever:~/xxx
# 递归上传
scp -r source destinaiton
# 从远程服务器 复制到 本地
scp mysever:~/xxx ~/xxx
```

# 利用scp 配置其他服务器的 vim tmux

```bash
ssh myserver "rm ~/.vimrc"
scp ~/.vimrc myserver

ssh myserver "rm ~/.tmuxconf"
scp ~/.tmuxconf myserver
```

# 相关的重要文件

```bash
~/.vimrc # 用户级 vim 运行时配置文件
~/.tmux.conf # 用户级 tmux 配置文件
~/.ssh/known_hosts # 尝试 ssh 过的 主机公钥 信息 
~/.ssh/id_rsa  id_rsa_pu # ssh密钥对 
~/.ssh/authorized_keys # 存放ssh客户端的公钥，实现免密登录

scp .vimrc
scp .tmuxconf
scp 
```

