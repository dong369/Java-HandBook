# 1 JDK配置

## 1.1 官网下载

[163镜像中心下载地址](http://mirrors.163.com/)

演示使用版本：ubuntu-18.10-live-server-amd64.iso

## 1.2 安装配置

### 1.2.1 基础环境

第一步：打开vi /etc/profile

```properties
a.复制jdk-8u65-linux-x64.tar.gz到 ~/Downloads
$>cp /mnt/hgfs/downloads/bigdata/jdk-8u65-linux-x64.tar.gz ~/Downloads
b.tar jdk-8u65-linux-x64.tar.gz
$>cd ~/Downloads
$>ta tar -xzvf jdk-8u65-linux-x64.tar.gz
c.移动到jdk1.8.0_65到/soft下
$>mv ~/Downloads/jdk1.8.0_65 /soft
$>ln -s /soft/jdk-xxx jdk			//创建符号连接
d.配置环境变量
[/etc/environment]
JAVA_HOME=/soft/jdk
PATH="...:/soft/jdk/bin"
```

第二步：使环境变量生效

```properties
source /etc/environment
```

第三步：看看自己的配置是否都正确

```properties
echo $JAVA_HOME
java -version
```

### 1.2.2 服务名称管理

更改hostname

```properties
sudo vi /etc/hostname
```

配置hosts

```properties
vi /etc/hosts
```

# 2 防火墙配置

```properties
sudo ufw status
sudo ufw allow 80
sudo ufw enable
sudo ufw reload
```

