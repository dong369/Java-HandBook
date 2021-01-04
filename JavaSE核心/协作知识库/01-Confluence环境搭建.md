# 1 Confluence

## 1.1 认识

Confluence是atlassian公司的产品，是一个专业的**企业知识管理**与**协同软件**，也可以用于构建**企业wiki**。使用简单，但它强大的编辑和站点管理特征能够帮助团队成员之间共享信息、文档协作、集体讨论，信息推送。是个非常cool的东西，这里记录一下如何安装并破解Confluence。

## 1.2 软件版本

CentOS 7、MySQL5.7、JDK1.8、confluence-6.13.0

## 1.3 下载软件

https://downloads.atlassian.com/software/confluence/downloads/atlassian-confluence-6.7.1-x64.bin

https://downloads.atlassian.com/software/confluence/downloads/atlassian-confluence-6.9.1-x64.bin

https://downloads.atlassian.com/software/confluence/downloads/atlassian-confluence-6.13.0-x64.bin

# 2 安装

## 2.1 授权执行

```shell
# 下载安装程序
wget https://downloads.atlassian.com/software/confluence/downloads/atlassian-confluence-6.13.0-x64.bin
cd /opt
# 授权安装程序执行权限
chmod +x atlassian-confluence-6.13.0-x64.bin
# 执行安装程序，进行安装：
./atlassian-confluence-6.13.0-x64.bin
```

## 2.2 安装

```shell
# o确定安装，c取消
This will install Confluence 6.9.1 on your computer.
OK [o, Enter], Cancel [c]
o
Click Next to continue, or Cancel to exit Setup.

# 选择2自定义安装
Choose the appropriate installation or upgrade option.
Please choose one of the following:
Express Install (uses default settings) [1], 
Custom Install (recommended for advanced users) [2, Enter], 
Upgrade an existing Confluence installation [3]
2

# 指定安装的目录，会自动创建，不输入直接回车就是使用默认路径
Select the folder where you would like Confluence 6.9.1 to be installed,
then click Next.
Where should Confluence 6.9.1 be installed?
[/opt/atlassian/confluence]
/opt/soft/confluence

# 使用默认的路径存储数据
Default location for Confluence data
[/var/atlassian/application-data/confluence]



# 使用默认的端口，8090和8000
Configure which ports Confluence will use.
Confluence requires two TCP ports that are not being used by any other
applications on this machine. The HTTP port is where you will access
Confluence through your browser. The Control port is used to Startup and
Shutdown Confluence.
Use default ports (HTTP: 8090, Control: 8000) - Recommended [1, Enter], Set custom value for HTTP and Control ports [2]
1

# 安装为后台进程，可后台运行
Confluence can be run in the background.
You may choose to run Confluence as a service, which means it will start
automatically whenever the computer restarts.
Install Confluence as Service?
Yes [y, Enter], No [n]
y

Extracting files ...
                                                                           


Please wait a few moments while we configure Confluence.

# 确定开始安装
Installation of Confluence 6.9.1 is complete
Start Confluence now?
Yes [y, Enter], No [n]
y


Please wait a few moments while Confluence starts up.
Launching Confluence ...


Installation of Confluence 6.9.1 is complete
Your installation of Confluence 6.9.1 is now ready and can be accessed via
your browser.
Confluence 6.9.1 can be accessed at http://localhost:8090
Finishing installation ...

# 安装完成，访问本机的8090端口进行web端安装
# 开放防火墙端口
firewall-cmd --add-port=8090/tcp --permanent
firewall-cmd --add-port=8000/tcp --permanent
firewall-cmd --reload
```

# 3 破解

## 3.1 获取jar

```shell
# 关闭Confluence
cd /usr/local/atlassian/confluence    # 进入你安装的目录
bin/stop-confluence.sh    # 关闭confluence

# 将confluence 下面的一个atlassian-extras-decoder-v2-3.3.0.jar包复制一份出来
cp confluence/WEB-INF/lib/atlassian-extras-decoder-v2-3.4.1.jar ~/

# 将其改名为atlassian-extras-2.4.jar
mv ~/atlassian-extras-decoder-v2-3.4.1.jar ~/atlassian-extras-2.4.jar

# 将改名后的atlassian-extras-2.4.jar 传到本地
# 使用sftp传输到本地，具体方法不细说了
```

## 3.2 获取授权码



## 3.3 更换jar包

```shell
# 传回服务器后，将名称改回之前的名称
mv atlassian-extras-2.4.jar atlassian-extras-decoder-v2-3.4.1.jar

# 然后覆盖回原路径
mv atlassian-extras-decoder-v2-3.4.1.jar /opt/soft/confluence/confluence/WEB-INF/lib/
```



# 4 数据源

## 4.1 MySQL

### 4.1.1 上传jar包



### 4.1.2 编码设置



### 4.1.3 校对集









https://blog.csdn.net/weixin_41004350/article/details/80590421

