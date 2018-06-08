# Version
- Docker Version : 18.03.1-ce
- Ubuntu Version : Ubuntu 16.04.4 LTS
- Openssh-server : OpenSSH_7.2p2
- Apr：1.5.2
- Apr-util：1.5.4
- Httpd：2.4.33 

# 一、使用`docker commit`创建含`apache`服务的image
#### 具体步骤：

- [Build_Tutorial](https://github.com/kangvcar/Docker-build-container/blob/master/apache_ubuntu/Build_Tutorial.md)

# 二、使用`dockerfile`构建image

#### 创建工作目录`apache_ubuntu`并编写如下文件：

- [Dockerfile](https://github.com/kangvcar/Docker-build-container/blob/master/apache_ubuntu/Dockerfile)：`Dockerfile`文件
- [sources.list](https://github.com/kangvcar/Docker-build-container/blob/master/apache_ubuntu/sources.list)：国内`apt`源

#### Build Image
```shell
# cd apache_ubuntu
# docker build -t apache_ubuntu .
```

#### Run Container
```shell
# 指定端口
# docker run -d -p 80:80 apache:ubuntu

# 指定站点目录
# docker run -d -p 80:80 -v /www:/usr/local/apache/htdocs apache:ubuntu
```
