为了避免每次使用docker命令都需要加上sudo权限，可以将当前用户加入安装中自动创建的docker用户组

```bash
sudo usermod -aG docker $USER
```

执行完此操作后，需要退出服务器，再重新登录回来，才可以省去sudo权限。

```bash
docker images # 查看镜像
docker pull ubuntu:20.04 # 拉取镜像
docker rmi ubuntu:20.04 # 删除镜像
docker save -o ubuntu_20_04.tar ubuntu:20.04 # 镜像导出
docker load -i ubuntu_20_04.tar # 镜像导入

docker ps -a # 查看容器
docker create -it ubuntu:20.04 # 创建容器
docker start xxx # 启动容器
docker stop xxx # 停止容器
docker restart xxx # 重启容器
docker container prune # 删除所有已停止的容器。释放磁盘空间
docker top xxx # 查看容器内正在运行的进程
docker attach xxx # 附加到正在运行的容器
docker export -o myubuntu.tar xxx # 容器导出
docker import myubuntu.tar # 文件导入为 Docker 镜像
docker cp myubuntu.tar CONTAINER:/root
docker cp CONTAINER:/root/myubuntu.tar . # 在本地与容器之间复制文

docker rename xxx xxx # 容器重命名
docker update CONTAINER --memory 500MB # 修改容器限制
```

配置docker 环境

```bash
scp /var/lib/acwing/docker/images/docker_lesson_1_0.tar aliyun:  # 将镜像上传到自己租的云端服务器
ssh aliyun # 登录自己的服务器
docker load -i docker_lesson_1_0.tar # 镜像导入
docker run -p 20000:22 --name my_docker_server -itd docker_lesson:1.0  # 创建并运行docker_lesson:1.0容器
docker attach my_docker_server  # 进入创建的docker容器
passwd  # 设置root密码
```

去云平台控制台中修改安全组配置，放行端口20000

```bash
ssh root@123.57.180.12 -p 20000 # 远程登录到 docker 容器当中

```
