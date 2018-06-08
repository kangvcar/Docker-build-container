# Version
- Docker Version : 18.03.1-ce
- Ubuntu Version : Ubuntu 16.04.4 LTS
- Openssh-server : OpenSSH_7.2p2
- Apr：1.5.2
- Apr-util：1.5.4
- Httpd：2.4.33

# Build
### 1. 运行容器并进入容器
```shell
# docker run -ti --name web -p 80:80 ubuntu:16.04 /bin/bash
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

### 3. 更新apt缓存并安装编译依赖环境
```shell
# apt-get update
# apt-get install -y vim net-tools iputils-ping openssh-server openssh-client 
# apt-get install -y gcc libpcre3 libpcre3-dev make openssl libssl-dev libxml2 libxml2-dev zip unzip libexpat1-dev libnghttp2-dev
```

### 4. 下载源码包并解压
```shell
# mkdir -p /usr/local/src
# cd /usr/local/src
# wget http://archive.apache.org/dist/apr/apr-1.5.2.tar.gz
# wget http://archive.apache.org/dist/apr/apr-util-1.5.4.tar.gz
# wget http://mirrors.hust.edu.cn/apache//httpd/httpd-2.4.33.tar.gz

# tar -xvf apr-1.5.2.tar.gz 
# tar -xvf apr-util-1.5.4.tar.gz 
# tar -xvf httpd-2.4.33.tar.gz

# mv -f apr-1.5.2 httpd-2.4.33/srclib/apr
# mv -f apr-util-1.5.4 httpd-2.4.33/srclib/apr-util
```

### 5. 编译安装apache
```shell
# mkdir -p /usr/local/apache
# cd /usr/local/src/httpd-2.4.33
# ./configure --prefix=/usr/local/apache --with-included-apr --with-mpm=worker --enable-so --enable-nonportable-atomics=yes --enable-ssl --enable-include --enable-cgi --enable-expires --enable-status --enable-info --enable-rewrite --enable-speling
# make
# make install
```

### 6. 修改apache配置文件
```shell
# sed -ri 's/^#ServerName www.example.com:80/ServerName localhost/g' /usr/local/apache/conf/httpd.conf
```

### 7. 启动apache服务
```shell
# cd /usr/local/apache/bin
# ./apachectl start
```

### 8. 退出容器
```shell
# exit
```

### 9. 使用`docker commit`创建 `image`
```shell
# docker commit web apache:ubuntu
```

### 10. 使用镜像启动容器
```shell
# 指定端口
# docker run -d -p 80:80 apache:ubuntu /usr/local/apache/bin/apachectl -D FOREGROUND

# 指定站点目录
# docker run -d -p 80:80 -v /www:/usr/local/apache/htdocs apache:ubuntu /usr/local/apache/bin/apachectl -D FOREGROUND
```

# Finish



