# 1 认识IntelliJ IDEA

## 1.1 说明

如果说IntelliJ IDEA是一款现代化智能开发工具的话，Eclipse则称得上是石器时代的东西了。其实笔者也是一枚从Eclipse转IDEA的探索者，随着近期的不断开发实践和调试，逐步体会到这款智能IDE带来的巨大开发便利，在强大的插件功能支持下，诸如对Git和Maven的支持简直让人停不下来，各种代码提示，包括JS更是手到擒来，最终不得不被这款神奇的IDE所折服。为了让身边更多的小伙伴参与进来，决定写下这篇文章，与君共享。经过前期的不断练习，后端开发之前使用的是eclipse和STS，注意eclipse中使用时快捷键失效的（禁用搜狗输入法的快捷键）。但是后期开发过程中我会选择idea作为开发工具使用。Eclipse通常用于实际项目的开发，STS用于Spring Boot和Spring Cloud的项目开发和进行框架的搭建，IDEA用于练习相应的新技术（[JDK8](https://v2.fangcloud.com/share/b7b5cc34dd46d5b0a61f3d1d5a)、[JDK9](https://v2.fangcloud.com/share/af36de3c0e21c9ca257fda3b0f)）。如果想获得更多软件工具可以查看[开发工具](https://www.jianshu.com/p/fb0737c88083)文章。

## 1.2 下载

> 高级传送门：[IDEA 终极版下载地址](https://www.jetbrains.com/idea/download/download-thanks.html?platform=windows)，本文中所以的操作和实列都是在2019-02版本下！

## 1.3 配置默认路径

> 很多软件默认安装路径都是C盘符，但是本人实在看不下去软件东西都放在C盘。

进入到idea的安装目录，idea.properties文件，添加自己指定的路径地址。

```properties
idea.config.path=D:/dev/2019dev/soft/IDE/ideaIU-2019.2.win/user/config
idea.system.path=D:/dev/2019dev/soft/IDE/ideaIU-2019.2.win/user/system
```

## 1.4 激活

> 更多激活方法可以参考[JetBrains破解](https://www.jianshu.com/p/a459c90f6a0e)文章。

# 2 IntelliJ IDEA & Eclipse

![对比](https://upload-images.jianshu.io/upload_images/8185387-459f402d8321801c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/648/format/webp)

## 2.1 IntelliJ IDEA 

> 谁用谁知道，话不多说！

## 2.2 Eclipse (STS)

>初学者可以多用，提高代码的编写能！

# 3 IntelliJ IDEA全局环境配置

## 3.1 JDK

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

## 3.2 Maven

> File | Settings | Build, Execution, Deployment | Build Tools

1. 官网下载maven
2. 配置环境变量

下载地址：[Apache](http://apache.org/)

2. settings.xml中配置maven镜像地址

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
<build>
	<finalName>${project.artifactId}</finalName>
	<plugins>
		<plugin>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-maven-plugin</artifactId>
			<executions>
				<execution>
					<goals>
						<goal>repackage</goal>
					</goals>
				</execution>
			</executions>
		</plugin>
	</plugins>
</build>
```

5. 镜像地址

```xml
<repositories>
    <repository>
        <id>alimaven</id>
        <name>aliyun maven</name>
        <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
    </repository>
</repositories>
```



## 3.3 Gradle

1. 下载安装
2. 配置环境变量（GRADLE_HOME、GRADLE_USER_HOME）
3. 安装检验：gradle -v
4. 配置IDEA

## 3.4 SVN

1. 下载客户端（服务端，非必须）
2. 安装客户端（注意打勾，否则找不到exe文件）
3. 安装检查：右键鼠标
4. 配置IDEA

## 3.5 Git

1. 下载
2. 安装配置
3. 安装检查：git version
4. 配置IDEA

## 3.6 编码乱码

> 项目源代码中文乱码：Settings > Editor > File Encodings > Global Encodings & Project Encodings 设置为：`UTF-8`



> Main方法运行，控制台中文乱码：Settings > Build, Execution, Deployment > Compile > Java Compiler > Additional command line parameters > 设置为：`-encoding utf-8`



> Tomcat运行，控制台中文乱码：Edit Configurations > Tomcat Server > server > VM options > 设置为：`-Dfile.encoding=UTF-8`。



> 如果还乱码，继续往下设置：idea > Help 菜单 > Edit Custom VM Options…菜单，编辑配置文件，在末尾添加：`-Dfile.encoding=UTF-8`。

## 3.7 启动视图

Run Dashboard运行视图，修改项目.idea文件夹下的workspace.xml文件，在接下来找到 <component name="RunDashboard">中添加下面代码。

```xml
<option name="configurationTypes">  
      <set>  
        <option value="SpringBootApplicationConfigurationType" />  
      </set>  
 </option>  
```



# 4 IntelliJ IDEA局部环境配置

## 4.1 字体和[主题01](http://www.riaway.com/)和[主题02](http://color-themes.com/?view=index)

窗口字体：File | Settings | Appearance & Behavior | Appearance（勾选字体，选择自己的字体和大小12）

编辑文件字体：File | Settings | Editor | Font（勾选覆盖字体，选择自己的字体和大小14）

file –> import setttings –> 选中 1 中下载的主题jar文件 –> 一路确认 –> 重启

## 4.2 自动编译

> File | Settings | Build, Execution, Deployment | Compiler（勾选build project automatically）

## 4.3 智能导包

> File | Settings | Editor | General | Auto Import

## 4.4 忽略大小写

> File | Settings | Editor | General | Code Completion | 将右侧的match case去掉打勾

## 4.5 悬浮提示开关

> File | Settings | Editor | General  |（Show quick documentation on mouse move）

## 4.6 取消单行显示tabs

> File | Settings | Editor | General | Editor Tabs=>去掉Show tabs in one row的勾选

## 4.7 行号显示

> File | Settings | Editor | General | Editor Tabs

## 4.8 插件下载去掉https

> File | Settings | Appearance & Behavior | System Settings | Updates

## 4.9 mybatis的xml警告背景

> 第一步：File | Settings | Editor | Inspections，去掉SQL | No data sources configured和SQL | SQL dialect detection对应的对号。
>
> 第二步：File | Settings | Editor | Color Scheme | General，选择code的Injected language fragment然后去除background。

## 4.10 用\*标识编辑过的文件 

```properties
1.Editor–>General –> Editor Tabs
2.在IDEA中，你需要做以下设置, 这样被修改的文件会以*号标识出来，你可以及时保存相关的文件。
3.“Mark modifyied tabs with asterisk
```

## 4.11 关闭自动代码提示

Preferences => IDE Settings => Editor => Code Completion => Autopopup documentation in (ms)

# 5 版本控制配置

## 5.1 SVN

### 5.1.1 基础配置

1. 安装SVN客户端

   特别注意：安装过程需要选中第二项command line tools的will be installed on local hard dirve

2. 屏蔽文件：*.iml;.idea;.classpath;.project;.settings;target;logs;

   File | Settings | Editor | File Types

3. SVN Disconnect插件安装

## 5.2 Git

### 5.2.1 Gitlab

1. 安装插件

   >Gitlab

2. 配置Gitlab服务地址

   >进行Gitlab服务地址的配置

   ![配置Gitlab服务地址](D:\dev\2019dev\code\idea-workspace\Java-HandBook\插图\Windows操作系统\配置GitLab服务器地址.png)

3. 申请Token

   >

4. 创建项目提交

### 5.2.2 GitHub



### 5.2.3 Gitee



### 5.2.4 分支/合并

```
右下角Git标志，点击New Branch，创建分支。
右下角Git标志，点击local Branchs，进行分支切换。
右下角Git标志，先切换到master分支，点击Remote Branch，进行分支合并到master。
```

### 5.2.5 Tag标签

```properties
// 推送Tag到远程
git push origin v1.0.0.0      
// 删除本地tag
git tag -d v1.0.0
// 删除已经远程的tag
git push origin :refs/tags/v1.0.0
注意：可以通过提交代码时进行tag的提交。
```

# 6 快捷键配置

## 6.1 eclipse快捷键

> File | Settings | Keymap

## 6.2 同步eclipse

1. 快捷键的配置，习惯了eclipse快捷键设置。（completion、close）
   智能提示：File->Settings-> Keymap-> 搜索completion basic -> 给Basic添加快捷键为Alt+/。
   关闭窗口：File->Settings-> Keymap-> 搜索editor tabs close-> 添加快捷键为Ctrl+W。
   格式代码：File->Settings-> Keymap-> 搜索Reformat Code-> 添加快捷键为Ctrl+Shift+F。

2. 快捷键的配置，习惯了Navicat Premium数据库管理工具的使用（open console、execute）
   File->Settings-> Keymap->Other-open console-> 添加快捷键为Ctrl+Q。
   File->Settings-> Keymap->Other-Execute Current Statement in Multiline Console-> 添加快捷键为Ctrl+R。File | Settings | Languages & Frameworks | SQL Dialects->设置IDEA的数据库连接对象。

3. 按鼠标中键快速打开智能提示，取代alt+enter 
   File->Settings-> Keymap-> 搜索 Show Intention Actions -> 添加快捷键为鼠标中键。

4. 按F2快速修改文件名，告别双手操作。
   File->Settings-> Keymap-> 搜索 Rename -> 将快捷键设置为F2 。

5. 按F3直接打开文件所在目录，浏览一步到位。
   File->Settings-> Keymap-> 搜索 Show In Explorer -> 将快捷键设置为F3 。

6. Ctrl+ E 来找到最近访问的文件和Ctrl+ Shift + E 来访问最近编辑的文件

7. Ctrl+ F 进行相应的搜索
   File->Settings-> Keymap-> 搜索find（edit下的）-> 将快捷键设置为Ctrl+ F

8. 全局查询/替换操作

   Main menu | Edit | Find | Find in Path... -> 将快捷键设置为ctrl+h

   Main menu | Edit | Find | Replace in Path...  -> 将快捷键设置为ctrl+shift+h

9. File->Settings-> Keymap-> 搜索 Run context configuration-> 将快捷键设置为Ctrl+ F10 

10. 展开和折叠方法的快捷键

​       Main menu | Code | Folding | Expand All  折叠->alt+

​       Main menu | Code | Folding | Collapse All 展开->alt-

11. 翻译快捷键

    Plug-ins | Translation | Translate 翻译->ctrl+1

12. 跟踪 Java 源码的技巧

    Main menu | Navigate | Type Hierarchy -> 类继承图关系，将快捷键设置为 F4

    Main menu | Navigate | Call Hierarchy  -> 方法的调用链关系，将快捷键设置为 ctrl+alt+h

    Main menu | Navigate | find usages  -> 方法的调用位置，将快捷键设置为 ctrl+g

    Main menu | Navigate | File Structure -> 类里面有那些方法，将快捷键设置为 ctrl+o

# 7 Live Template

## 7.1 使用系统自带的模板

> File | Settings | Editor | Live Templates

## 7.2 自定义模板

> live template自定义（常用设置：main、psfi、psfs、pi、ps、pm）

## 7.3 自定义类头

```properties
/**
 * Project -
 *
 * @Create by ${USER}
 * @Version 1.0
 * @Date 日期:2019/1/3 时间:14:50
 * @JDK 1.8
 * @Description 功能模块：构造函数注入
 */
```

# 8 Postfix

## 8.1 常用的postfix模板

> postfix（for、sout、field、return、nn）

# 9 插件安装

## 9.1 Translation

>翻译工具。全局配置快捷键为ctrl+1，配置有道的翻译，音标的字体设置为Arial。

## 9.2 Maven Helper
>maven管理工具。

![Maven Helper](D:\dev\2019dev\code\idea-workspace\Java-HandBook\插图\Windows操作系统\Idea-Maven-Helper.jpg)

## 9.3 lombok

>get、set等pojo简写。

## 9.4 JRebel

>热部署，自动编译。
>
>http://jrebel.pyjuan.com/c95f8c2b-9e97-4bd4-b9bf-48ba24fc3a10
>https://jrebel.qekang.com/bb25c9bf-7695-48d6-b1a0-baf893ca7631（激活地址）
>
>注意配置：一定要设置！！！离线工作（work offline）

## 9.5 RestfulToolkit

>快速查找rest full接口，sts中自带的有。

## 9.6 MyBatisCodeHelperPro

> mybatis插件，[破解下载](https://github.com/pengzhile/MyBatisCodeHelper-Pro-Crack/releases/tag/v2.0.2)，赶紧体验吧！

## 9.7 .ignore

> git提交文件。

## 9.8 Gitee、Gitlab、GitHub

> git代码托管。

## 9.9 对象赋值

> 一键根据json文本生成java类 非常方便；一键调用一个对象的所有set方法并且赋予默认值 在对象字段多的时候非常方便。
>
> GsonFormat、GenerateAllSetter

## 9.10 脚本语言

> shell脚本开发和bat脚本，2019后的idea虽然自带的也，但是不好用。
>
> 推荐安装：Batch Scripts Supports（执行bat脚本）、BashSupport（执行shell脚本，需要禁用自带的shell）

## 9.11 Stackoverflow

> 问题搜索。

## 9.12 Grep Console

> 日志管理。

## 9.13 Docker integration

> docker容器插件，高版本中已经内置，无需安装！！！

## 9.13 Alibaba Cloud Toolkit

> Cloud Toolkit 帮助开发者将本地应用程序一键部署到线下自有 VM，或阿里云 ECS、EDAS 和 Kubernetes 中去。

## 9.14 SVN Disconnect

> 解除SVN的项目关联。

## 9.15 VisualVM Launcher



## 9.15 CodeGlance



## 9.16 IdeaJad



## 9.17 Material Theme UI Plugin

工具的颜值也很重要，好的主题让人赏心悦目，有码代码的欲望。今天推荐一个IDEA颜值类插件：Material Theme UI

# 10 Debug调试

## 10.1 基本调试



## 10.2 条件调试

### 10.2.1 条件断点

循环中经常用到这个技巧，比如：遍历1个大List的过程中，想让断点停在某个特定值。在断点的位置，右击断点旁边的小红点，会出来一个界面，在Condition这里填入断点条件即可，这样调试时，就会自动停在i=10的位置。

### 10.2.2 回到上一步

该技巧最适合特别复杂的方法套方法的场景，好不容易跑起来，一不小心手一抖，断点过去了，想回过头看看刚才的变量值，如果不知道该技巧，只能再跑一遍。method1方法调用method2，当前断点的位置j=100，点击上图红色箭头位置的Drop Frame图标后，时间穿越了。

注：好奇心是人类进步的阶梯，如果想知道为啥这个功能叫Drop Frame，而不是类似Back To Previous 之类的，可以去翻翻JVM的书，JVM内部以栈帧为单位保存线程的运行状态，drop frame即扔掉当前运行的栈帧，这样当前“指针”的位置，就自然到了上一帧的位置。

## 10.3 多线程调试

多线程同时运行时，谁先执行，谁后执行，完全是看CPU心情的，无法控制先后，运行时可能没什么问题，但是调试时就比较麻烦了，最明显的就是断点乱跳，一会儿停这个线程，一会儿停在另一个线程。

## 10.4 多进程调试



## 10.5 远程调试

这也是一个装B的利器，本机不用启动项目，只要有源代码，可以在本机直接远程调试服务器上的代码，打开姿势如下：

项目启动时，先允许远程调试

```
 java -server -Xms512m -Xmx512m -Xdebug -Xnoagent -Djava.compiler=NONE -Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=9081 -Djava.ext.dirs=. ${main_class}
```

起作用的就是

```
-Xdebug -Xnoagent -Djava.compiler=NONE -Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=9081
```

注意：远程调试从技术上讲，就是在本机与远程建立scoket通讯，所以端口不要冲突，而且本机要允许访问远程端口，另外这一段参数，放要在`-jar` 或 `${main_class}`的前面

idea中设置远程调试前提是本机有项目的源代码 ，在需要的地方打个断点，然后访问一个远程的url试试，断点就会停下来。
