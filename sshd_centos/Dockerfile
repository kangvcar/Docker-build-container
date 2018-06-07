FROM centos:7

MAINTAINER kangvcar (kangvcar@126.com)

# 更换yum源为国内aliyun源
RUN rm -rf /etc/yum.repos.d/*
RUN curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo

# 更新yum缓存并安装openssh-server服务
RUN yum -y update
RUN yum install -y openssh-server

# 生成密钥对
RUN ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key -N ""
RUN ssh-keygen -t ecdsa -f /etc/ssh/ssh_host_ecdsa_key -N ""
RUN ssh-keygen -t ed25519 -f /etc/ssh/ssh_host_ed25519_key -N ""

# 把宿主主机的公钥到容器的/root/.ssh/authorized_keys文件中
RUN mkdir -p /root/.ssh
ADD authorized_keys /root/.ssh/authorized_keys

# 暴露22号端口
EXPOSE 22

# 启动容器是执行的命令
CMD ["/usr/sbin/sshd","-D"]