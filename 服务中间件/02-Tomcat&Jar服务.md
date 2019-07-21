## 1. 基础环境

### 1.1 部署工具

> [SecureCRT]()
> [Xshell免费享用](https://www.netsarang.com/zh/free-for-home-school/)，方便使用普遍。
> [Putty]()，小巧，无需安装。

## 2. Tomcat服务器

### 2.1 常用命令

> bin/catalina.sh run、bin/startup.sh、bin/shutdown.sh、ps -ef | grep tomcat、kill -9 pid
> 进程查看：
> [root@etcshop ~]# netstat -unltp|grep 8080
> [root@etcshop ~]# ps -ef | grep 9080
> [root@etcshop ~]# ps -ef | grep tomcat
> -t：仅显示TCP相关的选项
> -u：仅显示UDP相关的选项
> -n：拒绝显示别名，能显示数字的全部显示成数字
> -l：仅列出在listen（监听）的服务状态
> -p：显示相关链接的程序名
> Ctrl+c、Ctrl+z
> Ctrl+c和ctrl+z都是中断命令，但是他们的作用却不一样。
> Ctrl+c是强制中断程序的执行，而ctrl+z的是将任务中断,但是此任务并没有结束。

### 2.2 日志的查看

> tail 从文件尾开始查看文件
> -n 行数 
> 查看最后多沙航
> -f 
> 阻塞时查看
> Example： tail -f -n 100 logs/catalina.out
> 静态日志查看
> Example： less logs/catalina.out（大写G（shift+g）切换到日志的最后；大写N（shift+n）向上查找，n向下查找。）

### 2.3 Tomcat服务调优

关于TOMCAT端口的一些事情

一、Tomcat有三个端口号
​        1. 8080 ---- 服务访问端口
​        2. 8005 ---- 8005是远程关闭tomcat
​        3. 8009 ---- Apache代理时用的

二、Tomcat的优化配置

```xml
<Server port ="8005" shutdown ="SHUTDOWN">改为<Server port ="-1" shutdown ="SHUTDOWN">
telnet到8005端口可以关闭tomcat，存在安全漏洞，需要关闭。
    
<Connector port="8080" protocol ="HTTP/1.1" conne ctionT imeout ="20000"  redirectPort="8443" useBodyEncodingForURI="true" URIEncoding="UTF-8"/>

<Connector port="8009" protocol="AJP/ 1.3" redirectPort="8443" />
本行删除，这样做原因是端口监听的多，Tomcat服务器越慢！
```

## 3. Jar服务器



```properties
ps -ef | grep java
netstat -tln                    // 查看端口信息
netstat -antlp | grep LISTEN
netstat -antlp | grep java
netstat -nlp | grep java
netstat -nlp | grep :8101
lsof -i:8101                    // 获取Pid
lsof -p pid                     // 根据Pid找服务路径
kill -9 PID                     // 强制关闭指定 PID所对应的进程
jobs
```

