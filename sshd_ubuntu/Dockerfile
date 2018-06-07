FROM ubuntu:16.04

MAINTAINER kangvcar (kangvcar@126.com)

# 更换apt源为国内aliyun源
RUN mv /etc/apt/sources.list /etc/apt/sources.list.bak
ADD sources.list /etc/apt/sources.list

# 更新apt缓存并安装openssh-server服务
RUN apt-get update
RUN apt-get install -y openssh-server

# 创建ssh服务启动所需的目录
RUN mkdir -p /var/run/sshd
RUN mkdir -p /root/.ssh

# 修改SSH服务安全登录配置，取消pam登录限制
RUN sed -ri 's/session    required     pam_loginuid.so/#session    required     pam_loginuid.so/g' /etc/pam.d/sshd

# 把宿主主机的公钥到容器的/root/.ssh/authorized_keys文件中
ADD authorized_keys /root/.ssh/authorized_keys

# 复制启动脚本到容器中并赋予执行权限
ADD runsshd.sh /runsshd.sh
RUN chmod +x /runsshd.sh

# 暴露22号端口
EXPOSE 22

# 启动容器是执行的命令
CMD ["/runsshd.sh"]
