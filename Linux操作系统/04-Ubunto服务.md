# 1 Java环境

## 1 JDK配置

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

# 2 Python环境

## 2.1 Python

```shell
# python依赖环境
yum -y groupinstall "Development tools"
yum -y install zilb zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gdbm-devel db4-devel libpcap-devel xz-devel postgresql-devel* gcc-c++

# python安装
wget https://www.python.org/ftp/python/3.8.0/Python-3.8.0.tgz
mkdir /usr/local/python3 
tar -zxvf Python-3.8.0.tgz
cd Python-3.8.0
./configure --prefix=/usr/local/python3
make && make install
ln -s /usr/local/python3/bin/python3 /usr/bin/python3
ln -s /usr/local/python3/bin/pip3 /usr/bin/pip3
pip3 install --upgrade pip3
python -V

# 依赖包
pip3 install urllib3 Crypto pycrypto  PyMySQL sqlalchemy
```

## 2.2 Pip

1、进入/home/guodd/pip目录下，需要手动创建。

2、创建pip.ini文件，指定源。

```ini
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
# 这边的网址可以替换成下面任意一个，替换网址记得加/simple
# http://pypi.douban.com/
# http://pypi.hustunique.com/
# http://pypi.sdutlinux.org/
# http://pypi.mirrors.ustc.edu.cn/
[install]
trusted-host=pypi.tuna.tsinghua.edu.cn
```

3、安装依赖包

## 2.3 Anaconda

1、创建文件/home/guodd/.condarc

2、指定源

```ini
channels:
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
  - defaults
show_channel_urls: true
ssl_verify: true
```

3、安装依赖包

# 3 防火墙配置

```properties
sudo ufw status
sudo ufw allow 80
sudo ufw enable
sudo ufw reload
```

