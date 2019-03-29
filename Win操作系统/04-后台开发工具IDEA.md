[TOC]

### 1 认识IntelliJ IDEA

##### 1.1 说明

如果说IntelliJ IDEA是一款现代化智能开发工具的话，Eclipse则称得上是石器时代的东西了。其实笔者也是一枚从Eclipse转IDEA的探索者，随着近期的不断开发实践和调试，逐步体会到这款智能IDE带来的巨大开发便利，在强大的插件功能支持下，诸如对Git和Maven的支持简直让人停不下来，各种代码提示，包括JS更是手到擒来，最终不得不被这款神奇的IDE所折服。为了让身边更多的小伙伴参与进来，决定写下这篇文章，与君共享。经过前期的不断练习，后端开发之前使用的是eclipse和STS，注意eclipse中使用时快捷键失效的（禁用搜狗输入法的快捷键）。但是后期开发过程中我会选择idea作为开发工具使用。Eclipse通常用于实际项目的开发，STS用于Spring Boot和Spring Cloud的项目开发和进行框架的搭建，IDEA用于练习相应的新技术（[JDK8](https://v2.fangcloud.com/share/b7b5cc34dd46d5b0a61f3d1d5a)、[JDK9](https://v2.fangcloud.com/share/af36de3c0e21c9ca257fda3b0f)）。如果想获得更多软件工具可以查看[开发工具](https://www.jianshu.com/p/fb0737c88083)文章。

##### 1.2 下载

> 高级传送门：[IDEA 终极版下载地址](https://www.jetbrains.com/idea/download/download-thanks.html?platform=windows)

##### 1.3 激活

> 更多激活方法可以参考[JetBrains破解](https://www.jianshu.com/p/a459c90f6a0e)文章。

### 2 IntelliJ IDEA & Eclipse

![对比](https://upload-images.jianshu.io/upload_images/8185387-459f402d8321801c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/648/format/webp)

##### 2.1 IntelliJ IDEA 



##### 2.2 Eclipse 



### 3 IntelliJ IDEA全局环境配置

##### 3.1 JDK

> File | Settings | SDKs

1. 指定JDK的版本号（maven的settings中指定）

```xml
<profiles>
    <profile>
        <id>jdk-1.8</id>
        <activation>
            <activeByDefault>true</activeByDefault>
            <jdk>1.8</jdk>
        </activation>
        <properties>
            <maven.compiler.source>1.8</maven.compiler.source>
            <maven.compiler.target>1.8</maven.compiler.target>
            <maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>
        </properties>
    </profile>
</profiles>
```

##### 3.2 Maven

> File | Settings | Build, Execution, Deployment | Build Tools

1. 官网下载maven

下载地址：[Apache](http://apache.org/)

2. 配置maven镜像地址

推荐一：阿里云的镜像站（首推，新站，速度暴快）

```xml
<!—1.配置jar包的仓库地址-->
<localRepository>D:/mavenrepository</localRepository>
<!—2.配置jar包的下载镜像地址-->
<mirror>
    <id>alimaven</id>
    <name>aliyun maven</name>
    <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
    <mirrorOf>central</mirrorOf>
</mirror>
<mirror>
    <id>nexus-aliyun</id>
    <mirrorOf>*</mirrorOf>
    <name>Nexus aliyun</name>
    <url>http://maven.aliyun.com/nexus/content/groups/public</url>
</mirror>
```

推荐二：spring-boot和spring-cloud等版本比较新，所以目前国内的一些仓库还没有新版本的jar

```xml
<mirror>
      <id>spring-milestones</id>
      <name>Spring Milestones</name>
      <url>https://repo.spring.io/libs-milestone</url>
      <mirrorOf>central</mirrorOf>
</mirror>
```

注意：Setting.xml文件镜像地址：通常将其复制出来一份到mavenRepository目录下。（方便版本更换）

[Jar包下载地址01](https://mvnrepository.com/)

[Jar包下载地址02](https://www.mvnjar.com/)

[Jar包下载地址03](http://search.maven.org/)

3. 插件配置

```xml
<plugins>
    <!-- maven插件 -->
    <plugin>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-maven-plugin</artifactId>
        <version>${spring-boot.version}</version>
    </plugin>
    <!-- 编译级别 -->
    <plugin>
        <artifactId>maven-compiler-plugin</artifactId>
        <configuration>
            <target>${java.version}</target>
            <source>${java.version}</source>
            <encoding>UTF-8</encoding>
        </configuration>
    </plugin>
    <!-- 跳过单元测试 -->
    <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-surefire-plugin</artifactId>
        <configuration>
            <skip>true</skip>
        </configuration>
    </plugin>
</plugins>
```

4. 项目打包配置

```xml


```



##### 3.3 编码UTF8

> ##### 工程代码乱码：File | Settings | Editor | File Encodings
>
> ##### main方法乱码：File | Settings | Build, Execution, Deployment | Compiler | Java Compiler Additiona command line parameters > 设置为：`-encoding utf-8`
>
> ##### tomcat运行控制台乱码：-Dfile.encoding=UTF-8

##### 3.4 启动视图

Run Dashboard运行视图，修改项目.idea文件夹下的workspace.xml文件，在接下来找到 <component name="RunDashboard">中添加下面代码。

```xml
<option name="configurationTypes">  
      <set>  
        <option value="SpringBootApplicationConfigurationType" />  
      </set>  
 </option>  
```



### 4 IntelliJ IDEA局部环境配置

##### 4.1 字体和[主题01](http://www.riaway.com/)和[主题02](http://color-themes.com/?view=index)

窗口字体：File | Settings | Appearance & Behavior | Appearance（勾选字体，选择自己的字体和大小12）

编辑文件字体：File | Settings | Editor | Font（勾选覆盖字体，选择自己的字体和大小14）

file –> import setttings –> 选中 1 中下载的主题jar文件 –> 一路确认 –> 重启

##### 4.2 自动编译

> File | Settings | Build, Execution, Deployment | Compiler（勾选build project automatically）

##### 4.3 智能导包

> File | Settings | Editor | General | Auto Import

##### 4.4 忽略大小写

> File | Settings | Editor | General | Code Completion

##### 4.5 悬浮提示

> File | Settings | Editor | General

##### 4.6 取消单行显示tabs

> File | Settings | Editor | General | Editor Tabs

##### 4.7 行号显示

> File | Settings | Editor | General | Editor Tabs

##### 4.8 插件下载去掉https

> File | Settings | Appearance & Behavior | System Settings | Updates

##### 4.9 mybatis的xml警告背景

> 第一步：File | Settings | Editor | Inspections，去掉SQL | No data sources configured和SQL | SQL dialect detection对应的对号。
>
> 第二步：File | Settings | Editor | Color Scheme | General，选择code的Injected language fragment然后去除background。

### 5 版本控制配置

#### 5.1 SVN

##### 5.1.1 基础配置

1. 安装SVN客户端

   特别注意：安装过程需要选中第二项command line tools的will be installed on local hard dirve

2. 屏蔽文件：.iml;.idea;.classpath;.project;.settings;target;logs;

   File | Settings | Editor | File Types

3. SVN Disconnect插件安装

#### 5.2 Git

##### 5.2.1 Gitlab

1. 安装插件

   >

2. 配置Gitlab服务地址

   >

3. 申请Token

   >

4. 创建项目提交

   >

5. 克隆项目

   > git clone projectName

6. 分支创建及切换

   > 右下角Git标志，点击New Branch，创建分支。
   >
   > 右下角Git标志，点击local Branchs，进行分支切换。

7. 分支合并

   > 右下角Git标志，先切换到master分支，点击Remote Branch，进行分支合并到master。

8. 创建tag标签

   > git push origin v1.0.0.0      //推送Tag到远程
   > git tag -d v1.0.0.1           //删除本地tag
   > git push origin :refs/tags/v1.0.0.1     //删除已经远程的tag
   >
   > 注意：可以通过提交代码时进行tag的提交。

##### 5.2.2 GitHub



##### 5.2.3 Gitee

### 6 快捷键配置

##### 6.1 eclipse快捷键

> File | Settings | Keymap

##### 6.2 同步eclipse

1. 快捷键的配置，习惯了eclipse快捷键设置。（completion、close）
   智能提示：File->Settings-> Keymap-> 搜索completion -> 给Basic添加快捷键为Alt+/。
   关闭窗口：File->Settings-> Keymap-> 搜索close-> 添加快捷键为Ctrl+W。
   格式代码：File->Settings-> Keymap-> 搜索Reformat Code-> 添加快捷键为Ctrl+Shift+F。

2. 快捷键的配置，习惯了Navicat Premium数据库管理工具的使用（open console、execute）
   File->Settings-> Keymap-> 搜索open console-> 添加快捷键为Ctrl+Q。
   File->Settings-> Keymap-> 搜索execute-> 添加快捷键为Ctrl+R。

3. 按鼠标中键快速打开智能提示，取代alt+enter 
   File->Settings-> Keymap-> 搜索 Show Intention Actions -> 添加快捷键为鼠标中键。

4. 按F2快速修改文件名，告别双手操作。
   File->Settings-> Keymap-> 搜索 Rename -> 将快捷键设置为F2 。

5. 按F3直接打开文件所在目录，浏览一步到位。
   File->Settings-> Keymap-> 搜索 Show In Explorer -> 将快捷键设置为F3 。

6. Ctrl+ E 来找到最近访问的文件和Ctrl+ Shift + E 来访问最近编辑的文件

7. Ctrl+ F 进行相应的搜索
   File->Settings-> Keymap-> 搜索 find -> 将快捷键设置为Ctrl+ F

8. 全局查询/替换操作

   Main menu | Edit | Find | Find in Path... -> 将快捷键设置为ctrl+h

   Main menu | Edit | Find | Replace in Path...  -> 将快捷键设置为ctrl+shift+h

9. File->Settings-> Keymap-> 搜索 Run context configuration-> 将快捷键设置为Ctrl+ F10 

10. 展开和折叠方法的快捷键

​       Main menu | Code | Folding | Expand All  折叠->alt+

​       Main menu | Code | Folding | Collapse All 展开->alt

11. 翻译快捷键

    Plug-ins | Translation | Translate 翻译->ctrl+1

12. 跟踪 Java 源码的技巧

    Main menu | Navigate | Type Hierarchy -> 类继承图关系，将快捷键设置为 F4

    Main menu | Navigate | Call Hierarchy  -> 方法的调用链关系，将快捷键设置为 ctrl+alt+h

    Main menu | Navigate | File Structure -> 类里面有那些方法，将快捷键设置为 ctrl+o

### 7 Live Template

##### 7.1 使用系统自带的模板

> File | Settings | Editor | Live Templates

##### 7.2 自定义模板

> live template自定义（常用设置：main、psfi、psfs、pi、ps、pm）

### 8 Postfix

##### 8.1 常用的postfix模板

> postfix（for、sout、field、return、nn）

### 9 插件安装

##### 9.1 Translation

>翻译工具。

##### 9.2 Maven Helper

>maven管理工具。

##### 9.3 lombok

>get、set等pojo简写。

##### 9.4 JRebel

>热部署，自动编译。
>
>http://139.199.89.239:1008/88414687-3b91-4286-89ba-2dc813b107ce（激活地址）

##### 9.5 RestfulToolkit

>快速查找rest full接口，sts中自带的有。

##### 9.6 MyBatisCodeHelperPro

> mybatis插件，[破解下载](https://github.com/pengzhile/MyBatisCodeHelper-Pro-Crack/releases/tag/v2.0.2)，赶紧体验吧！

##### 9.7 .ignore

> git提交文件。

##### 9.8 Gitee、Gitlab、GitHub

> git代码托管。

##### 9.9 GsonFormat、GenerateAllSetter

> 一键根据json文本生成java类 非常方便；一键调用一个对象的所有set方法并且赋予默认值 在对象字段多的时候非常方便。

##### 9.10 BashSupport 、CMD Support

> shell脚本开发。

##### 9.11 Stackoverflow

> 问题搜索。

##### 9.12 Grep Console

> 日志管理。

##### 9.13 Docker integration

> docker容器插件

##### 9.13 Alibaba Cloud Toolkit

> Cloud Toolkit 帮助开发者将本地应用程序一键部署到线下自有 VM，或阿里云 ECS、EDAS 和 Kubernetes 中去。

##### 9.14 SVN Disconnect

> 解除SVN的项目关联。

##### 9.15 VisualVM Launcher

>

