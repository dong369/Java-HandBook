# 1  理论知识

## 1.1 概念

Anaconda（[官方网站](https://link.jianshu.com/?t=https%3A%2F%2Fwww.anaconda.com%2Fdownload%2F%23macos)）就是可以便捷获取包且对包能够进行管理，同时对环境可以统一管理的发行版本。Anaconda包含了conda、numpy、scipy、ipython notebook等在内的超过180个科学包及其依赖项。

## 1.1 区别

Anaconda、conda、pip、virtualenv的区别？

Anaconda是一个包含180+的科学包及其依赖项的发行版本。其包含的科学包包括：conda, numpy, scipy, ipython notebook等。conda**结合**了pip和virtualenv的功能。

conda是包及其依赖项和环境的管理工具。快速安装、运行和升级包及其依赖项；在计算机中便捷地创建、保存、加载和切换环境。

pip是用于安装和管理软件包的包管理器。

virtualenv：用于创建一个**独立的**Python环境的工具。

# 2 环境配置

## 2.1 下载安装



## 2.2 环境变量

https://www.jianshu.com/p/62f155eb6ac5

E:\Anaconda（Python需要）
E:\Anaconda\Scripts（conda自带脚本）
E:\Anaconda\Library\mingw-w64\bin（使用C with python的时候）
E:\Anaconda\Library\bin（jupyter notebook动态库）

## 2.3 源配置

conda config --add channels r

修改：C:\Users\guodd\\.condarc

```shell
channels:
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
  - defaults
show_channel_urls: true
ssl_verify: true
```



# 3 Conda

## 3.1 基本命令

```shell
# 版本信息
conda --version
# 当前环境信息
conda info
```

## 3.2 下载依赖

```python
# 1、搜索模块
conda search --full-name <package_full_name>
# 2、安装模块
conda install <package_name>
# 3、指定安装版本
conda install pymysql=8.21
# 4、更新所以依赖
conda update --all
```

## 3.3 多环境

```python
# 1、创建环境
conda create --name <env_name> <package_names>
conda create --name <new_env_name> --clone <copied_env_name>
# 2、查看当前环境列表
conda info --envs & conda info -e & conda env list
# 3、激活指定名称的环境
activate <env_name>
# 4、删除指定名称的环境
conda remove --name <env_name> --all
# 5、退出环境
deactivate
```

