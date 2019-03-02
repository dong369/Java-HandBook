## 1. 基础环境

### 1.1 部署工具

> [SecureCRT]()，
> [Xshell免费享用](https://www.netsarang.com/zh/free-for-home-school/)，方便使用普遍。
> [Putty]()，小巧，无需安装。

## 2. Tomcat服务器

6.	Tomcat服务器的常用命令
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

7.	Vi文本的常用命令
> vi编辑器有三种模式：命令模式，编辑模式，末行模式
> 保存退出（Escx=wq）、正常退出、不保存退出及强制退出
> 打开vi后首先是命令模式，用i,o,a等进入编辑模式，按esc退出编辑模式，回到命令模式。在命令模式下输入:wq表示保存退出，:wq!强制保存退出，:w表示保存，:w file表示保存在另一个文件中 :q表示退出，在命令模式下可以用用ZZ，ZQ这些指令直接保存退出。
> 8.	快捷键常用命令
>       Ctrl+c、Ctrl+z
>       Ctrl+c和ctrl+z都是中断命令，但是他们的作用却不一样。
>       Ctrl+c是强制中断程序的执行，而ctrl+z的是将任务中断,但是此任务并没有结束。

9.	日志的查看
> tail 从文件尾开始查看文件
> -n 行数 
> 查看最后多沙航
> -f 
> 阻塞时查看
> Example： tail -f -n 100 logs/catalina.out
> 静态日志查看
> Example： less logs/catalina.out（大写G（shift+g）切换到日志的最后；大写N（shift+n）向上查找，n向下查找。）



## 3. Jar服务器



```properties
ps -ef | grep java
netstat -tln                    // 查看端口信息
netstat -nlp | grep java
netstat -nlp | grep :8101
lsof -i:8101                    // 获取Pid
lsof -p pid                     // 根据Pid找服务路径
kill -9 PID                     // 强制关闭指定 PID所对应的进程
jobs
```

