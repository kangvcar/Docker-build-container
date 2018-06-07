# Version
- Docker Version : 18.03.1-ce
- CentOS Version : CentOS Linux release 7.5.1804
- Openssh-server : OpenSSH_7.4p1 

# 一、使用`docker commit`创建含`sshd`服务的image
#### 具体步骤：

- [Build_Tutorial](https://github.com/kangvcar/Docker-build-container/blob/master/sshd_centos/Build_Tutorial.md)

# 二、使用`dockerfile`构建image

#### 创建工作目录`sshd_centos`并编写如下文件：

- [Dockerfile]()：`Dockerfile`文件
- [authorized_keys]()：宿主主机的公钥(使用`ssh-keygen -t rsa`生成，公钥文件`/root/.ssh/id_rsa.pub`)

#### Build Image
```shell
# cd sshd_centos
# docker build -t sshd:centos .
```

#### Run Container
```shell
# docker run -d -p 1022:22 sshd:centos
```

#### Use ssh connection container
```shell
# ssh localhost -p 1022
```

