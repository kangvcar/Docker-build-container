FROM ubuntu:16.04

#######################
##   编译安装 Apache  ##
#######################

MAINTAINER kangvcar (kangvcar@126.com)

# 更新apt源
RUN mv /etc/apt/sources.list  /etc/apt/sources.list.bak
ADD sources.list /etc/apt/sources.list

# 安装依赖环境
RUN apt-get update
RUN apt-get install -y vim net-tools iputils-ping openssh-server openssh-client 
RUN apt-get install -y gcc libpcre3 libpcre3-dev make openssl libssl-dev libxml2 libxml2-dev zip unzip libexpat1-dev libnghttp2-dev

# 下载源码包
WORKDIR /usr/local/src
RUN wget http://archive.apache.org/dist/apr/apr-1.5.2.tar.gz
RUN wget http://archive.apache.org/dist/apr/apr-util-1.5.4.tar.gz
RUN wget http://mirrors.hust.edu.cn/apache//httpd/httpd-2.4.33.tar.gz

RUN tar -xvf apr-1.5.2.tar.gz 
RUN tar -xvf apr-util-1.5.4.tar.gz 
RUN tar -xvf httpd-2.4.33.tar.gz

RUN mv -f apr-1.5.2 httpd-2.4.33/srclib/apr
RUN mv -f apr-util-1.5.4 httpd-2.4.33/srclib/apr-util

# 编译安装apache
RUN mkdir -p /usr/local/apache
WORKDIR /usr/local/src/httpd-2.4.33
RUN ./configure --prefix=/usr/local/apache --with-included-apr --with-mpm=worker --enable-so --enable-nonportable-atomics=yes --enable-ssl --enable-include --enable-cgi --enable-expires --enable-status --enable-info --enable-rewrite --enable-speling
RUN make && make install

# 修改apache配置文件
RUN sed -ri 's/^#ServerName www.example.com:80/ServerName localhost/g' /usr/local/apache/conf/httpd.conf

# 启动apache
WORKDIR /usr/local/apache/bin
RUN ./apachectl start

EXPOSE 80

CMD ["/usr/local/apache/bin/apachectl","-D","FOREGROUND"]