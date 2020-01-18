# 1. 系统安装

![](D:\dev\2019dev\code\idea-workspace\Java-HandBook\插图\Windows操作系统\安装windows系统流程.png)

## 1.1 系统格式

- .GHO 点评：傻大粗 安装简单 体积庞大
- .ISO 点评：官方常见格式 方便刻录 点击安装
- .WIM 点评：官方常用格式 最合理官方的安装方式
- .ESD 点评：官方常用格式 比WIM更小巧

## 1.2 引导模式

Delete Esc F2 F8 F9 F10 F11 F12

> 主板为了兼容MBR分区表，一般会提供Legacy BIOS和UEFI BIOS启动模式选项，而且默认情况下主板是优先开启Legacy BIOS，所以如果要使用UEFI模式安装Windows，就必须手动去调整开启UEFI引导模式。

第一是：更改BIOS默认系统
自带出厂时机器默认的是WIN8 for 64Bit。我们要将其改成“Other OS”。这样的话BIOS设置就是完成了。这样就能加载“U盘”、“光驱”启动了。这样就解决了客户反映的在选择菜单启动时候反复停留在启动菜单选项中。选择什么都是返回这个界面。

第二是：Boot项目设置
BootMode系统默认设置是UEFI要将其改为Legacy Support。
Boot Priorlty也要更改为 Legacy First这样更改就是跟之前那些没有带系统出厂的一样了。可以看到按“F2”进入BIOS。设置，按“F12”进入菜单选择启动菜单选项的提示。

第三是：Boot Security

## 1.3 硬盘分区格式

>MBR和GPT

1、主板BIOS开启UEFI，硬盘就是GPT，主板BIOS是传统Legacy，硬盘就是MBR，而且大多数情况下装系统必须是UEFI+GPT或Legacy+MBR，不能有其他的组合
2、win7默认是Legacy+MBR，win7 64位支持UEFI+GPT下安装，win10默认是UEFI+GPT，win10还支持Legacy+MBR

## 1.4 进入PE系统

>

## 1.5 win10转win7

>修改启动方式：Legacy BIOS + MBR组合方式。

## 1.6 win7转win10

>修改启动方式：UEFI BIOS + GPT组合方式。



HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion

# 2. 软件安装

## 2.1 系统配置

### 2.1.1 Quicker



### 2.1.2 办公娱乐



### 2.1.3 开发工具



## 2.2 下载软件

### 2.2.1 新云下载



### 2.2.2 ZD423



## 2.3 系统备份还原

工具选择：



# 3. 快捷键熟练使用
## 3.1 Windows快捷键

```properties
Ctrl+A：全选
Ctrl+C：粘贴
Ctrl+V：粘贴
Ctrl+X：剪切
Ctrl+F：搜索
Ctrl+W：关闭

Shift+右键：快速进入Dos窗口
Shift+end（shift+home）快速选择从当前行到结尾行（开始行）

Win+q：搜索
Win+w：便签\草图
Win+e：快速进入资源管理
Win+r：CMD快速启动
Win+t：窗口切换
Win+u：window设置
Win+i：window设置
Win+d：显示桌面
Win+x：功能选择
Win+g：音频\性能设置
Win+Tab：切换窗口
Win+数字[1，2，3，4......]windows的下窗口程序选择
win+上下左右箭头：进行系统的分屏操作

Alt+Tab：切换窗口
Alt+F4：关闭应用窗口

Mstsc：打开电脑的远程桌面控制。
Calc：计算器
Notepad：记事本
Mspaint：画图
control：进入控制面板
services.msc：开启服务。
Regedit：打开系统注册表。
sysdm.cpl：进入环境变量编辑
Msconfig：关闭启动项
Regedit：资源管理器

Ipconfig：查看IP
Netstat –ans：查看占用的端口
Winver：电脑的版本
Taskmgr：任务管理器
Syskey：电脑安全命令
Dxdiag：诊断工具
Psr.exe：步骤记录
Sysprep：系统恢复初始化

Cd：进入特定的目录。
Dir：当前目录下的文件。
Cls：清屏

Ctrl+Shift+T：恢复关闭
Ctrl+数字[1，2，3，4......]浏览器或clover工具窗口切换
```

## 3.2 Google快捷键
```properties
F6：快速定位导航栏
ctrl+t：
ctrl+shift+t：
ctrl+tab：向后切换网页
ctrl+shift+tab：向前切换网页
alt+左右箭头：进行网页的回退
```

# 4. 安装操作

## 4.1 联想电脑



## 4.2 华硕电脑