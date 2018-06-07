# Version
- Docker Version : 18.03.1-ce
- CentOS Version : CentOS Linux release 7.5.1804
- Openssh-server : OpenSSH_7.4p1

# Build
### 1. 运行容器并进入容器
```shell
# docker run -it --name sshd-base centos:7 /bin/bash
```

### 2. 更换yum源为国内aliyun源
```shell
# mkdir /etc/yum.repos.d/repo.bak
# mv /etc/yum.repos.d/CentOS-* /etc/yum.repos.d/repo.bak/
# curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
```

### 3. 更新yum缓存并安装openssh-server服务
```shell
# yum -y update
# yum install -y openssh-server
```

### 4. 生成密钥对
```shell
# ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key -N ""
# ssh-keygen -t ecdsa -f /etc/ssh/ssh_host_ecdsa_key -N ""
# ssh-keygen -t ed25519 -f /etc/ssh/ssh_host_ed25519_key -N ""
```

### 5. 在宿主主机生成密钥对
```shell
# ssh-keygen -t rsa
# cat /root/.ssh/id_rsa.pub
```

### 6. 把宿主主机的公钥内容复制到容器的`/root/.ssh/authorized_keys`文件下
```shell
# mkdir -p /root/.ssh
# vi /root/.ssh/authorized_keys
```

### 7. 启动SSH服务
```shell
# /usr/sbin/sshd -D &
```

### 8. 安装net-tools软件包并查看22号端口是否在监听状态
```shell
# yum install -y net-tools
# netstat -tlunp | grep 22
```

### 9. 退出容器
```shell
# exit
```

### 10. 使用commit子命令通过容器创建镜像
```shell
# docker commit sshd-base sshd:centos
# docker images
```
至此已经创建完成含有`ssh`功能的容器，可以push到公共仓库共享镜像

### 11. 用刚刚创建含有`ssh`功能的镜像`sshd:centos`启动容器
```shell
# docker run -d -p 1022:22 sshd:centos /usr/sbin/sshd -D
# docker ps
```

### 12. 在宿主主机ssh登录容器
```shell
# ssh 192.168.10.10 -p 1022
```

# Finish