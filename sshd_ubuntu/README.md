# Version
- Docker Version : 18.03.1-ce
- Ubuntu Version : Ubuntu 16.04.4 LTS
- Openssh-server : OpenSSH_7.2p2 

# 一、使用`docker commit`创建含`sshd`服务的image
#### 具体步骤：

- [Build_Tutorial](https://github.com/kangvcar/Docker-build-container/blob/master/sshd_ubuntu/Build_Tutorial.md)

# 二、使用`dockerfile`构建image

#### 创建工作目录`sshd_ubuntu`并编写如下文件：

- [Dockerfile](https://github.com/kangvcar/Docker-build-container/blob/master/sshd_ubuntu/Dockerfile)：`Dockerfile`文件
- [runsshd.sh](https://github.com/kangvcar/Docker-build-container/blob/master/sshd_ubuntu/runsshd.sh)：启动`sshd`服务脚本
- [authorized_keys](https://github.com/kangvcar/Docker-build-container/blob/master/sshd_ubuntu/authorized_keys)：宿主主机的公钥(使用`ssh-keygen -t rsa`生成，公钥文件`/root/.ssh/id_rsa.pub`)
- [sources.list](https://github.com/kangvcar/Docker-build-container/blob/master/sshd_ubuntu/sources.list)：国内`apt`源

#### Build Image
```shell
# cd sshd_ubuntu
# docker build -t sshd:ubuntu .
```

#### Run Container
```shell
# docker run -d -p 1022:22 sshd:ubuntu
```

#### Use ssh connection container
```shell
# ssh localhost -p 1022
```
