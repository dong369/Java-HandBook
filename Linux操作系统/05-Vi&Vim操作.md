[TOC]

## 1. 技术目标

> * 目标01：Vim 模式切换
> * 目标02：光标移动
> * 目标03：编辑(Editing)
> * 目标04：插入模式(Insert Mode)
> * 目标05：退出(Exiting)
> * 目标06：剪切\复制\删除

## 2. 文件（夹）操作
### 2.1 文件

白色：表示普通文件
蓝色：表示目录
绿色：表示可执行文件
红色：表示压缩文件
浅蓝色：链接文件
红色闪烁：表示链接的文件有问题
黄色：表示设备文件
灰色：表示其它文件

> 本篇文章引导你通过熟练使用Linux

```properties
touch  filename                              //创建文件
echo content  >  filename                    //给文件添加内容[覆盖添加内容，原内容被删除]
echo 内容  >> filename                        //给文件追加内容
echo 内容  >/>>  newfilename                  //会创建一个新的文件，并且有添加内容[重定向方式]
wc  文件               //计算文件行数
cat/more/less         //输出文件内容
head -n  文件          //查看文件前n行内容
tail -n  文件          //查看文件末尾n行内容
```

### 2.2 文件夹

```properties
mkdir  hehe/xixi
mkdir -p first/second/third      //递归创建3个目录 加-p选项
mv book.txt  shu.txt             // 改名字，地址只要不存在就是改名字
mv  ten/bread.ods  firstt        //移动，只要地址是存在的目录就是移动
cp -R /a   /b                    //复制"目录"需要添加参数-R
rm -rf /a                        //可以删除一切普通的目录或文件 递归
mkdir -p server1/{data,log} server2/{data,log} server3/{data,log}
```

### 3. Vi编辑
### 3.1 Vim 模式切换

> ##### 命令模式、插入模式、末行模式，[中文维基教科书](https://zh.wikibooks.org/zh-sg/Vim/%E4%B8%89%E7%A7%8D%E6%A8%A1%E5%BC%8F)。

#### 3.1.1 命令模式
| 命令     | 作用                    |
| -------- | ----------------------- |
| `[N]yy`  | 复制一行或者N行         |
| `[N]dd`  | 删除\剪切一行或者N行    |
| `p`      | 粘贴                    |
| `u`      | 撤回上一步              |
| `Ctrl+u` | 撤回全部                |
| `ctrl+r` | 恢复上一步操作          |
| `[N]G`   | 文档的第N行或者最后一行 |
| `gg`     | 文档的第一行            |
| `Ctrl+f` | 上一页                  |
| `Ctrl+b` | 下一页                  |

#### 3.1.2 插入模式

| 命令   | 作用               |
| ------ | ------------------ |
| `i`    | 插入到光标前面     |
| `I`    | 插入到行的开始位置 |
| `a`    | 插入到光标的后面   |
| `A`    | 插入到行的最后位置 |
| `o, O` | 新开一行           |
| `Esc`  | 关闭插入模式       |

#### 3.1.3 末行模式

| 命令                      | 作用                                |
| ------------------------- | ----------------------------------- |
| `:w`                      | 保存                                |
| `:wq` , `:x`              | 保存并关闭                          |
| `:q`                      | 关闭（已保存）                      |
| `:q!`                     | 强制关闭                            |
| `/`                       | 从前往后找（正向搜索），使用n下一个 |
| `?`                       | 从后往前走(反向搜索)                |
| `:set ff`、`:set ff=unix` | 查看文件格式（fileformat=unix）     |
| `:set nu`                 | 显示文件行号                        |
| `:set nonu`               | 去除文件行号                        |

## 4. Vim编辑

### 4.1 安装

1. 没有安装前

```properties
[root@localhost ~]# rpm -qa|grep vim
vim-minimal-7.4.160-5.el7.x86_64
[root@localhost ~]# 
```

2. 安装后

```properties
[root@localhost ~]# rpm -qa|grep vim
vim-filesystem-7.4.160-5.el7.x86_64
vim-common-7.4.160-5.el7.x86_64
vim-enhanced-7.4.160-5.el7.x86_64
vim-minimal-7.4.160-5.el7.x86_64
[root@localhost ~]# 
```

3. 安装操作（根据安装前返回列表和安装后的对比，缺少哪个安装哪个）

```properties
yum -y install vim-enhanced
yum -y install vim*
```



