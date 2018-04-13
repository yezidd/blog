---
layout: article
title: 关于docker的使用
date: 2017-10-15 18:31:27
tags: docker
---

#### 什么是docker

```
	Docker 是一个开源的应用容器引擎，
	让开发者可以打包他们的应用以及依赖包到一个可移植的容器中，
	然后发布到任何流行的 Linux 机器上，也可以实现虚拟化。
	容器是完全使用沙箱机制，相互之间不会有任何接口。
```

#### docker的安装

> 这边我采用了daocloud国内的docker镜像安装

> 因为官方的docker pull镜像超级慢

> 安装方法:[详情请见daocloud官网](https://download.daocloud.io/Docker_Mirror/Docker)

> 截图:

> ![docker安装成功截图](/image/docker_1.png)

#### docker的hello world

> 首先要下载一个镜像文件

> 这边我们运行 sudo pull centos:7 等待下载完成

> 之后运行 docker run centos:7 /bin/echo "Hello world"

> docker是执行的二进制文件

> run是运行容器的命令

> centos:7是容器名字

> 之后会返回一串二进制文件,这便是我们开启的容器的唯一辨识ID

> ![docker运行的截图](/image/docker_2.png)

#### docker的命令行

> docker logs -f 容器Id

> ![命令截图](/image/docker_3.png)

> docker logs查看容器里面的日志输出 -f就是以容器为环境输出 没有则是以服务器为环境

> docker ps

> 输出当前所有运行中的容器

#### docker运行一个web服务器

> docker 查看监听的端口

> ![运行的截图](/image/docker_5.png)

> docker inspect会打印出当前容器所存在的所有的信息,以json的形式

> ![运行的截图](/image/docker_6.png)

#### docker创建容器的过程
##### 本地通过DockFile构建

> 先创建目录nginx用于存档后面的相关东西

> mkdir -p ~/nginx/www ~/nginx/logs ~/nginx/conf

> www目录是将映射为nginx容器配置的虚拟目录

> logs目录将映射为nginx容器的日志目录

> conf目录里的配置文件将映射为nginx容器的配置文件

> ```

FROM debian:jessie

MAINTAINER NGINX Docker Maintainers "docker-maint@nginx.com"

ENV NGINX_VERSION 1.10.1-1~jessie

RUN apt-key adv --keyserver hkp://pgp.mit.edu:80 --recv-keys 573BFD6B3D8FBC641079A6ABABF5BD827BD9BF62 \
        && echo "deb http://nginx.org/packages/debian/ jessie nginx" >> /etc/apt/sources.list \
        && apt-get update \
        && apt-get install --no-install-recommends --no-install-suggests -y \
                                                ca-certificates \
                                                nginx=${NGINX_VERSION} \
                                                nginx-module-xslt \
                                                nginx-module-geoip \
                                                nginx-module-image-filter \
                                                nginx-module-perl \
                                                nginx-module-njs \
                                                gettext-base \
        && rm -rf /var/lib/apt/lists/*

# forward request and error logs to docker log collector
RUN ln -sf /dev/stdout /var/log/nginx/access.log \
        && ln -sf /dev/stderr /var/log/nginx/error.log

EXPOSE 80 443

CMD ["nginx", "-g", "daemon off;"]

```
> 通过Dockerfile创建一个镜像，替换成你自己的名字

> docker build -t nginx

> ~/nginx$ docker images nginx可以找到当前创建的docker文件

> 后续继续更新

