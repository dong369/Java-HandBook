# 1 文件操作
## 1.1 文件

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
touch  filename                              // 创建文件
echo content  >  filename                    // 给文件添加内容[覆盖添加内容，原内容被删除]
>  filename                                  // 置空文件
echo content  >> filename                    // 给文件追加内容
echo content  >/>>  newfilename              // 会创建一个新的文件，并且有添加内容[重定向方式]
wc  文件               // 计算文件行数
cat/more/less         // 输出文件内容
head -n  文件          // 查看文件前n行内容
tail -n  文件          // 查看文件末尾n行内容
less -N  文件          // 带行号查看
```

## 1.2 文件夹

```properties
mkdir  hehe/xixi
mkdir -p first/second/third      // 递归创建3个目录 加-p选项
mv book.txt  shu.txt             // 改名字，地址只要不存在就是改名字
mv  ten/bread.ods  firstt        // 移动，只要地址是存在的目录就是移动
cp -r /a /b                      // 复制"目录"需要添加参数-R
rm -rf /a                        // 可以删除一切普通的目录或文件 递归
mkdir -p server1/{data,log} server2/{data,log} server3/{data,log}   // 创建多个文件夹
mkdir -pm 777 usedir // 指定文件夹的权限 
```

# 2 Vim编辑

## 2.1 安装

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

## 2.2 Vi编辑

### 2.2.1 模式切换

> ##### 命令模式、插入模式、末行模式，[中文维基教科书](https://zh.wikibooks.org/zh-sg/Vim/%E4%B8%89%E7%A7%8D%E6%A8%A1%E5%BC%8F)。

### 2.2.2 命令模式

| 命令                         | 作用                    |
| ---------------------------- | ----------------------- |
| [N]yy、y[N]motion            | 复制一行或者N行         |
| [N]dd、d[N]motion            | 删除\剪切一行或者N行    |
| x                            | 删除当前字符            |
| b、w                         |                         |
| cw、ciw（ci"）;yi"；di"；dfa |                         |
| p                            | 粘贴                    |
| u                            | 撤回上一步              |
| `Ctrl+u`                     | 撤回全部                |
| `ctrl+r`                     | 恢复上一步操作          |
| `[N]G`                       | 文档的第N行或者最后一行 |
| `gg`                         | 文档的第一行            |
| `Ctrl+f`                     | 上一页                  |
| `Ctrl+b`                     | 下一页                  |
| shift+a/g                    | 行尾/行首插入           |
| shift+o                      | 上一行插入              |
| hjkl                         | 左下上右                |

### 2.2.3 插入模式

| 命令   | 作用               |
| ------ | ------------------ |
| `i`    | 插入到光标前面     |
| `I`    | 插入到行的开始位置 |
| `a`    | 插入到光标的后面   |
| `A`    | 插入到行的最后位置 |
| `o, O` | 新开一行           |
| `Esc`  | 关闭插入模式       |

### 2.2.4 末行模式

| 命令                      | 作用                                |
| ------------------------- | ----------------------------------- |
| `:w`                      | 保存                                |
| `:wq` , `:x`              | 保存并关闭                          |
| `:q`                      | 关闭（已保存）                      |
| `:q!`                     | 强制关闭                            |
| `/`                       | 从前往后找（正向搜索），使用n下一个 |
| `?`                       | 从后往前走(反向搜索)                |
| `:set ff`、`:set ff=unix` | 查看文件格式（fileformat=unix）     |
| `:set nu`                 | 显示文件行号（numeric）             |
| `:set nonu`               | 去除文件行号                        |

# 3 查找替换

## 3.1 文件查找

```shell
find / -name file
```

## 3.2 内容查找

```shell
grep -r "JAVA_HOME" /etc/

# 查看文件
grep -Ev "^$|#" file
grep -Ev "^#|^$" /etc/kibana/kibana.yml

egrep -v "^$|#" 文件名
```

## 3.3 快速注释

### 3.3.1 多行注释

1、进入命令行模式，按ctrl + v进入 visual block模式，然后按j, 或者k选中多行，把需要注释的行标记起来

2、按大写字母I，再插入注释符，例如//

3、按esc键就会全部注释了

### 3.3.2 取消多行注释

1、进入命令行模式，按ctrl + v进入 visual block模式，按字母l横向选中列的个数，例如 // 需要选中2列

2、按字母j，或者k选中注释符号

3、按d键就可全部取消注释

## 3.4 过滤

```properties
ls | grep vim
```

## 3.5 替换

```properties
将dev替换为prod，在末行模式下执行
:%s/dev/prod/g 
```

# 4 Vim配置

## 4.1 自定义

cd：进入home路径

创建文件夹：mkdir .vim

进入.vim，创建vimrc文件

编辑配置vimrc文件

```shell
let mapleader=" "
syntax on

set number
set norelativenumber
set relativenumber
set cursorline   // 显示线
set wrap // 
set showcmd
set wildmenu
set scrolloff=5

set hlsearch
exec "nohlsearch"
set incsearch
set ignorecase
set smartcase

noremap <LEADER><CR> :nohlsearch<CR>
noremap n h

map s <nop>
map S :w<CR>
map Q :q<CR>
map R :source $MYVIMRC<CR>
```

## 4.2 录制宏

1、把光标定位在第一行；

2、在normal模式下输入qa(当然也可以输入qb, qc, etc，这里的a, b, c是指寄存器名称，vim会把录制好的宏放在这个寄存器中)(PS：如果不知道什么是vim的寄存器，请自行放狗搜之)；

3、正常情况下，vim的命令行会显示“开始录制”的字样，这时候，把光标定位到第一个字符（按0或者|），再按x删除，按j跳到下一行；

4、normal模式下输入q，结束宏录制。

5、在normal模式下输入@a，以播放我们刚录制好的存在寄存器a中的宏。

## 4.3 快捷键

shift+a、shift+i、o、shift+o