# 1 基本概念

## 1.1 镜像（Images）

>  我们都知道，操作系统分为内核和用户空间。对于Linux而言，内核启动后，会挂载root文件系统为其提供用户空间支持。而Docker镜像（Image），就相当于是一个root文件系统。比如官方镜像ubuntu:14.04就包含了完整的一套Ubuntu14.04最小系统的root文件系统。
>  Docker镜像是一个特殊的文件系统，除了提供容器运行时所需的程序、库、资源、配置等文件外，还包含了一些为运行时准备的一些配置参数（如匿名卷、环境变量、用户等）。镜像不包含任何动态数据，其内容在构建之后也不会被改变。
----
> 镜像拉取的提供商地址
> + [Docker官网](https://hub.docker.com/explore/)
> + [网易蜂巢官网](https://c.163.com/)
> + [阿里云官网](https://dev.aliyun.com/search.html)

## 1.2 容器（Container）
> 镜像（Image）和容器（Container）的关系，就像是面向对象程序设计中的类和实例一样，镜像是静态的定义，容器是镜像运行时的实体。容器可以被创建、启动、停止、删除、暂停等。容器的实质是进程，但与直接在宿主执行的进程不同，容器进程运行于属于自己的独立的命名空间。

## 1.3 仓库（Repository）
> 镜像构建完成后，可以很容易的在当前宿主上运行，但是，如果需要在其它服务器上使用这个镜像，我们就需要一个集中的存储、分发镜像的服务，Docker Registry就是这样的服务。一个Docker Registry中可以包含多个仓库（Repository）；每个仓库可以包含多个标签（Tag）；每个标签对应一个镜像。通常，一个仓库会包含同一个软件不同版本的镜像，而标签就常用于对应该软件的各个版本。我们可以通过<仓库名>:<标签>	的格式来指定具体是这个软件哪个版本的镜像。如果不给出标签，将以latest作为默认标签。

# 2 Docker的安装

[官方文档](https://docs.docker.com/install/linux/docker-ce/centos/#install-docker-ce)


## 2.1 下载Docker

> [win10以外的系统](https://www.docker.com/products/docker-toolbox)
> [win10的系统](https://www.docker.com/products/docker#/windows)
> [资料一](http://www.imooc.com/article/16448)
> [资料二](http://get.daocloud.io/#install-docker)

## 2.2 Windows安装过程
> 安装安装流程就行nest安装即可，当安装完成时，启动docker时，有可能出现boot2docker下载不下来的问题，此时的解决方法有两中：第一是启动翻墙，然后重新下载；第二是在网上下载[boot2docker](https://github.com/boot2docker/boot2docker/releases/tag/v17.07.0-ce
> )，然后放到C:\Users\guod\.docker\machine\cache路径下。![图-01](http://upload-images.jianshu.io/upload_images/8185387-3ab5942d2e5de6d7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 2.3 Ubuntu下的安装配置

> 其实如果想学习docker的话，不建议使用ContOS作为环境，如果想使用，请一定选择ContOS7的版本。


## 2.4 ContOS7下的安装配置

### 2.4.1 环境要求

> 需要centos7及以上的正式版,不支持测试版或者预览版等

```properties
cat /etc/redhat-release
uname -a | awk '{split($3, arr, "-"); print arr[1]}'
```

### 2.4.2 卸载已有版本

```properties
sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-selinux \
                  docker-engine-selinux \
                  docker-engine
```

> 如果安装过.还要清理`/var/lib/docker/`下的镜像,容器,容器卷,网络 等内容

### 2.4.3 安装依赖包

```properties
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
```

### 2.4.4 设置存储库,更新索引

> 由于国外的速度太慢.这里更换到阿里云镜像.
> 我们去`http://mirrors.aliyun.com/`镜像站下载

```properties
sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
yum makecache fast
```

### 2.4.5 安装docker-ce

```properties
sudo yum -y install docker-ce
sudo systemctl start docker
docker version
```

### 2.4.6 修改镜像源

> 由于墙的原因,镜像源也要改.常用的镜像源有 网易,有道,daocloud,时速云

```properties
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://woawf2sg.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

### 2.4.7 运行hello world

```properties
docker run hello-world
```

> 出现内容中如果有`Hello from Docker!`就算是成功安装了

### 2.4.8 卸载

```properties
sudo yum remove docker-ce            # 移除docker
sudo rm -rf /var/lib/docker          # 移除镜像，容器，卷，网络，自定义文件等
```

### 2.4.9 开机启动

```properties
systemctl enable docker.service
```

## 2.5 Docker-composer

### 2.5.1 下载授权

```properties
curl -L https://get.daocloud.io/docker/compose/releases/download/1.22.0/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose    // 文件授权
docker-compose --version    // 安装版本查看
```

### 2.5.2 启动/关闭/拉取镜像

```properties
启动：docker-compose up -d
关闭：docker-compose down
重新拉取镜像：docker-compose pull
```

## 2.5 安装过程问题

### 2.5.1 安装过程中

```
(default) Latest release for github.com/boot2docker/boot2docker is v17.06.2-ce
(default) Downloading /Users/jessie/.docker/machine/cache/boot2docker.iso from https://github.com/boot2docker/boot2docker/releases/download/v17.06.2-ce/boot2docker.iso...
```

```properties
docker: Error response from daemon: OCI runtime create failed: unable to retrieve OCI runtime error (open /run/docker/containerd/daemon/io.containerd.runtime.v1.linux/moby/6602807a7f1a03689a847741b0a10171788f7bed2aa92a4dff35c429b02248d1/log.json: no such file or directory): docker-runc did not terminate sucessfully: docker-runc: symbol lookup error: docker-runc: undefined symbol: seccomp_version
```

执行以下指令，更新`libseccomp`依赖(最好以后还是升级高版本centos)

```properties
yum install -y http://mirror.centos.org/centos/7/os/x86_64/Packages/libseccomp-2.3.1-3.el7.x86_64.rpm
```

### 2.5.2 配置[加速器](https://www.daocloud.io/)

```
sudo sed -i "s|EXTRA_ARGS='|EXTRA_ARGS='--registry-mirror=curl -sSL https://get.daocloud.io/daotools/set_mirror.sh | sh -s http://3c679070.m.daocloud.io |g" /var/lib/boot2docker/profile
```

### 2.5.3 Docker-Machine

> windows下安装好docker后，Docker-Machine环境自动已经配置好。
> docker-machine -h
> docker-machine ls
> 创建Docker命令：docker-machine create <name>
> docker-machine ssh <name>

### 2.5.4 IPv4 forwarding

```properties
Docker报错 WARNING: IPv4 forwarding is disabled. Networking will not work.

解决
vim  /usr/lib/sysctl.d/00-system.conf
net.ipv4.ip_forward=1
systemctl restart network
```

问题：
ERROR: for nacos Cannot start service nacos: driver failed programming external connectivity on endpoint sc-nacos-standalone (36358a2ef7be82e0234b962be541b065d6e303b3d5c4d89590b6ec771393afca): (iptables failed: iptables --wait -t nat -A DOCKER -p tcp -d 0/0 --dport 8848 -j DNAT --to-destination 172.20.0.2:8848 ! -i br-0e24c543a72a: iptables: No chain/target/match by that name.

解决办法：
systemctl restart docker

# 3 命令操作

## 3.1 基础命令

```properties
Docker版本：docker version（查看安装版本）
启动Docker：systemctl start docker（service docker start）
停止Docker：systemctl stop docker（service docker stop）
重启Docker：systemctl restart docker（service docker restart）
拉取镜像：docker pull [OPTIONS] NAME[:TAG] NAME：代表拉取镜像的名称
查看镜像：docker images [OPTIONS][REPOSITORY] [:TAG]
删除镜像：docker rmi [容器名称&ID]
运行镜像：docker run [OPTIONS] IMAGE[:TAG][COMMAND] [ARG…]
前台运行：docker run [容器名称&ID]
后台运行：docker run --name some-* -p 宿主机端口:容器端口 –d [容器名称&ID]
查看运行的容器：docker ps
查看所以的容器：docker ps -a（docker stop CONTAINER ID、docker rm CONTAINER ID）
进入容器内部：docker exec -it containerId /bin/bash（docker exec -it [容器名称] bash）
退出容器：exit
制作自己的镜像：docker build –t name:latest .（指定名称）
推送镜像：docker push registry.cn-hangzhou.aliyuncs.com/namespace/keepalived:[镜像版本号]
查看容器日志：docker logs -f [容器名称]
```

# 4 服务安装

## 4.1 安装vim

```
在使用docker容器时，有时候里边没有安装vim，敲vim命令时提示说：vim: command not found，这个时候就需要安装vim，可是当你敲apt-get install vim命令时，提示：  
Reading package lists... Done  
Building dependency tree         
Reading state information... Done  
E: Unable to locate package vim  
这时候需要敲：apt-get update，这个命令的作用是：同步 /etc/apt/sources.list 和 /etc/apt/sources.list.d 中列出的源的索引，这样才能获取到最新的软件包。  
等更新完毕以后再敲命令：apt-get install vim命令即可。  
```

## 4.2 MySQL服务
```shell
sudo docker pull mysql  # 拉取最新版本的镜像，当前为 MySQL 8 版本，tag 为 latest
sudo docker pull mysql:5.7 # 指定拉取 MySQL 5.7 版本

sudo docker run -p 3306:3306 \
    --name mysql \
    -v /home/mysql/conf:/etc/mysql/conf.d \
    -v /home/mysql/logs:/logs \
    -v /home/mysql/data:/var/lib/mysql \
    -e MYSQL_ROOT_PASSWORD=your-password \
    -d mysql
```
> `-p` 3306:3306：将容器的3306端口映射到主机的3306端口
>  `-v` 将主机~/mysql/logs目录挂载到容器的/logs
>  `-v` 将主机mysql/data目录挂载到容器的/mysql_data
>  `-e` MYSQL_ROOT_PASSWORD=123456：初始化root用户的密码
>
> `-d`后台启动

## 4.3 Oracle服务

```shell
docker pull wnameless/oracle-xe-11g

docker run --name oracle -d -p 1521:1521 \
           -v /home/docker/data/oracle_data:/data/oracle_data \
           -e ORACLE_ALLOW_REMOTE=true \
           --restart=always wnameless/oracle-xe-11g
                     
docker exec -it 容器id /bin/bash

su oracle
cd $ORACLE_HOME
cd /u01/app/oracle
mkdir test
chmod 777 test

create tablespace TEST datafile '/u01/app/oracle/oradata/test/test.dbf' size 100M;  
// 创建表空间

create user TEST identified by TEST123 default tablespace TEST;		// 创建用户

grant connect,resource to TEST;
grant connect,resource,dba to system;

grant dba to TEST; // 授予dba权限后，这个用户能操作所有用户的表

drop user TEST cascade; // 删除用户

// 进行连接
sid: xe
service name: xe
username: system
password: oracle
```

## 4.4 Tomcat服务

```shell
docker pull tomcat
docker run -it --rm -p 8888:8080 [REPOSITORY&IMAGE ID]
```
## 4.5 Nginx服务
```shell
docker run --name nginx -p 81:80 -d pid

mkdir -p /home/service/nginx/{conf,conf.d,static,log,ssl}
docker cp nginx:/etc/nginx/nginx.conf /home/service/nginx/conf/nginx.conf
docker cp nginx:/etc/nginx/conf.d/default.conf /home/service/nginx/conf.d/default.conf
docker cp nginx:/usr/share/nginx/html/index.html /home/service/nginx/static/index.html
docker stop nginx
docker rm nginx

docker run -p 443:443 -p 81:80 --name dev-nginx \    
    -v /home/service/nginx/static:/usr/share/nginx/html  \
    -v /home/service/nginx/conf/nginx.conf:/etc/nginx/nginx.conf \    
    -v /home/service/nginx/conf.d:/etc/nginx/conf.d \
    -v /home/service/nginx/ssl:/ssl \
	-v /home/service/nginx/log:/var/log/nginx  \
    -d nginx
```

## 4.6 Redis服务
```
docker pull nginx
docker run --name some-redis -p 6379:6379 -v redis-data:/data -d [REPOSITORY&IMAGE ID]
```
## 4.7 Mongo服务
```
docker pull nginx
docker run --name some-mongo -p 27017:27017 -d [REPOSITORY&IMAGE ID]
```
## 4.8 RabbitMQ服务
```
docker pull rabbitmq:management
docker run -d --hostname my-rabbit --name rabbit -p 5672:15672 rabbitmq:management
```
# 5 IDEA整合Docker

## 5.1 安装插件

>Docker integration

高版本的idea自带了docker的插件

## 5.2 修改Docker配置

1. 修改vim /usr/lib/systemd/system/docker.service

```properties
[Service]
ExecStart=/usr/bin/dockerd -H tcp://0.0.0.0:2375 -H unix:///var/run/docker.sock
```

2. 重启

```properties
sudo systemctl daemon-reload
sudo systemctl restart docker      # 进行
netstat -tulp                      # 检查2375端口
curl 127.0.0.1:2375/info           # 验证是否成功
```

## 5.3 IDEA连接操作

1. 连接地址

```properties
tcp://192.168.100.13:2375
```


<img src="..\..\插图\Linux操作系统\Docker连接配置.png"/>

## 5.4 Spring Boot部署

1. 项目编写Dockerfile文件

```properties
# 容器的依赖
FROM hub.c.163.com/wuxukun/maven-aliyun:3-jdk-8

# 校正容器时间
RUN /bin/cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
RUN echo "Asia/Shanghai" > /etc/timezone

# 获取打包后的jar
COPY target/*.jar app.jar

RUN bash -c 'touch ./app.jar'

# 指定端口
EXPOSE 8080

# 进行启动
CMD ["java", "-jar", "app.jar", "--spring.profiles.active=prod"]
```



```properties
FROM hub.c.163.com/wuxukun/maven-aliyun:3-jdk-8

ADD pom.xml /tmp/build/

ADD src /tmp/build/src
        #构建应用
RUN cd /tmp/build && mvn clean package \
        #拷贝编译结果到指定目录
        && mv target/*.jar /guod.jar \
        #清理编译痕迹
        && cd / && rm -rf /tmp/build

VOLUME /tmp
EXPOSE 8080
ENTRYPOINT ["java","-jar","/guod.jar"]
```

2. 项目部署操作

> 运行Dockerfile文件，项目发布成功！

<img src="../..\插图\Linux操作系统\Dockerfile配置文件.png"/>

