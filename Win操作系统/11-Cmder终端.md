## 1. why cmder

当然这篇文章的受众应当是Windows用户， 因为Mac以及Linux下的终端已经足够了，而cmd命令行却有许多问题存在，所以才会出现这样一个能够替代原生工具的软件。

cmder不是一个独立的工具，应该说是一系列工具包的集合，包括Conemu、clink、git for windows等，足够简单好用，且支持多栏显示，多个tab运行，功能十分强大。

> 全安装版 cmder 自带了 msysgit, 除了 git 本身这个命令之外, 里面可以使用大量的 linux 命令：比如 grep, curl(没有 wget)； 像vim, grep, tar, unzip, ssh, ls, bash, perl 对于爱折腾的Coder更是痛点需求。

ps: 默认使用的是Monokai主题，看起来非常舒适。

## 2. Download

[官网下载地址](https://link.jianshu.com?t=http://cmder.net/)

- mini版： 功能简单，很小巧，只有4M多，主要是cmd和powershell
- full版： 功能强大，包含了git、powershell、bash、chocolatey、Cygwin、SDK等功能

> 可以交叉使用 [cygwin](https://link.jianshu.com?t=http://www.cygwin.com/) 的部分增强命令

## 3. 安装完毕后的准备

### 3.1 添加右键菜单

> 添加到环境变量后，管理员身份运行cmd， 并输入该命令： `Cmder.exe /REGISTER ALL`

### 3.2 设置中文编码

chcp 65001  就是换成UTF-8代码页

chcp 936 可以换回默认的GBK

chcp 437 是美国英语 

1. 查看编码命令：chcp（936代表简体中文；65001代表UTF-8）

2. 设置中文编码：右击cmd窗口，点击setting，在Start-up下的environment中加入：`set LANG=zh_CN.UTF8`或`chcp 65001`
3. 查看校验。

### 3.3 快捷键

| tab操作         | 快捷键                |
| --------------- | --------------------- |
| 新建tab         | Ctrl + t              |
| 关闭tab         | Ctrl + w              |
| 切换Tab         | Ctrl+Tab或Ctrl+1,2... |
| 新建CMD         | Shift + Alt + 1       |
| 新建 PowerShell | Shift + Alt + 2       |
| 全屏操作        | Alt + Enter           |