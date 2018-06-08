FROM ubuntu:16.04

MAINTAINER kangvcar (kangvcar@126.com)

# 更换apt源为国内aliyun源
RUN mv /etc/apt/sources.list /etc/apt/sources.list.bak
ADD sources.list /etc/apt/sources.list

# 创建www-data用户和组
RUN groupadd www-data && \
	useradd -g www-data -s /bin/nologin -M www-data

# 更新apt缓存并安装nginx
RUN \
	apt-get update && \
	apt-get install -y nginx && \
	rm -rf /var/lib/apt/lists/* && \
	chown -R www-data:www-data /var/lib/nginx

EXPOSE 80

ENTRYPOINT nginx -g "daemon off;"