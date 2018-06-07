# Version
- Docker Version : 18.03.1-ce
- Ubuntu Version : Ubuntu 16.04.4 LTS
- Openssh-server : OpenSSH_7.2p2

# Build
### 1. 运行容器并进入容器
```shell
# docker run -it --name sshd-base ubuntu:latest /bin/bash
```

### 2. 更换apt源为国内aliyun源
```shell
# mv /etc/apt/sources.list  /etc/apt/sources.list.bak
# echo 'deb http://mirrors.aliyun.com/ubuntu/ xenial main
deb-src http://mirrors.aliyun.com/ubuntu/ xenial main

deb http://mirrors.aliyun.com/ubuntu/ xenial-updates main
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-updates main

deb http://mirrors.aliyun.com/ubuntu/ xenial universe
deb-src http://mirrors.aliyun.com/ubuntu/ xenial universe
deb http://mirrors.aliyun.com/ubuntu/ xenial-updates universe
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-updates universe

deb http://mirrors.aliyun.com/ubuntu/ xenial-security main
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-security main
deb http://mirrors.aliyun.com/ubuntu/ xenial-security universe
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-security universe' > /etc/apt/sources.list
```

### 3. 更新apt缓存并安装openssh-server服务
```shell
# apt-get update
# apt-get install -y openssh-server
```

### 4. 启动SSH服务
```shell
# mkdir -p /var/run/sshd
# /usr/sbin/sshd -D &
```

### 5. 安装net-tools软件包并查看22号端口是否在监听状态
```shell
# apt-get install -y net-tools
# netstat -tlunp | grep 22
```

### 6. 修改SSH服务安全登录配置，取消pam登录限制
```shell
# sed -ri 's/session    required     pam_loginuid.so/#session    required     pam_loginuid.so/g' /etc/pam.d/sshd
```

### 7. 在宿主主机生成密钥对
```shell
# ssh-keygen -t rsa
# cat /root/.ssh/id_rsa.pub
```

### 8. 把宿主主机的公钥内容复制到容器的`/root/.ssh/authorized_keys`文件下
```shell
# mkdir -p /root/.ssh
# apt-get install -y vim
# vim /root/.ssh/authorized_keys
```

### 9. 创建SSH服务的启动脚本runsshd.sh
```shell
# vim /runsshd.sh
# chmod +x /runsshd.sh
```

`runsshd.sh`脚本的内容如下

```shell
#!/bin/bash
/usr/sbin/sshd -D
```

### 10. 退出容器
```shell
# exit
```

### 11. 使用commit子命令通过容器创建镜像
```shell
# docker commit sshd-base sshd:ubuntu
# docker images
```
至此已经创建完成含有`ssh`功能的容器，可以push到公共仓库共享镜像

### 12. 用刚刚创建含有`ssh`功能的镜像`sshd:ubuntu`启动容器
```shell
# docker run -d -p 1022:22 sshd:ubuntu /runsshd.sh
# docker ps
```

### 13. 在宿主主机ssh登录容器
```shell
# ssh 192.168.10.10 -p 1022
```

# Finish

