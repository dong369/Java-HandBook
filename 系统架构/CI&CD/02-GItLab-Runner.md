# 1 CI/CD介绍

## 1.1 CI/CD

持续集成、持续交付

GitLab-CI就是一套配合GitLab使用的持续集成系统（当然，还有其它的持续集成系统，同样可以配合GitLab使用，比如Jenkins）。而且GitLab8.0以后的版本是默认集成了GitLab-CI并且默认启用的。

GitLab-CI全称是gitlab continuous integration的意思，也就是持续集成。中心思想是当每一次push到gitlab的时候，都会触发一次脚本执行，然后脚本的内容包括了测试，编译，部署等一系列自定义的内容。

多平台：Unix, Windows,mac0S和任何其他支持Go的平台上执行构建。

多语言：构建脚本是命令行驱动的，并且可以与Java,PHP,Ruby,C和任何其他语言一起使用。

稳定构建：构建在与 Gitlab不同的机器上运行。

并行构建： Gitlab CI/CD在多台机器上拆分构建，以实现快速执行

实时日志记录：合并请求中的链接将您带到动态更新的当前构建日志

灵活的管道：您可以在每个阶段定义多个并行作业，并且可以触发其他构建

版本管道：一个。 gitlab-ci.ym1文件包含您的测试，整个过程的步聚，使每个人都能贡献更改，并确保每个分
支获得所需的管道。

自动缩放：您可以自动缩放构建机器，以确保立即处理您的构建并将成本降至最低。

构建工件：您可以将二进制文件和其他构建工件上載到 Gitlab并浏览和下载它们。

Docker支持：可以使用自定义 Dockerl映像，作为测试的一部分启动服务，构建新的 Dockerl映像，甚至可以在
Kubernetes上运行

容器注册表：内置的容器注册表，用于存储，共享和使用容器映像。

受保护的变量：在部署期间使用受每个环境保护的变量安全地存储和使用机密。

环境：定义多个环境。

## 1.2 GitLab-CI

这个是一套配合GitLab使用的持续集成系统，是GitLab自带的，也就是你装GitLab的那台服务器上就带有的。无需多考虑。.gitlab-ci.yml的脚本解析就由它来负责。

![img](../../插图/clip_image001.jpg)

 

## 1.3 GitLab-Runner

GitLab-Runner是配合GitLab-CI进行使用的。一般地，GitLab里面的每一个工程都会定义一个属于这个工程的软件集成脚本，用来自动化地完成一些软件集成工作。当这个工程的仓库代码发生变动时，比如有人push了代码，GitLab就会将这个变动通知GitLab-CI。这时GitLab-CI会找出与这个工程相关联的Runner，并通知这些Runner把代码更新到本地并执行预定义好的执行脚本。

所以，GitLab-Runner就是一个用来执行软件集成脚本的东西。你可以想象一下：Runner就像一个个的工人，而GitLab-CI就是这些工人的一个管理中心，所有工人都要在GitLab-CI里面登记注册，并且表明自己是为哪个工程服务的。当相应的工程发生变化时，GitLab-CI就会通知相应的工人执行软件集成脚本。

这个是脚本执行的承载者，.gitlab-ci.yml的script部分的运行就是由runner来负责的。GitLab-CI浏览过项目里的.gitlab-ci.yml文件之后，根据里面的规则，分配到各个Runner来运行相应的脚本script。这些脚本有的是测试项目用的，有的是部署用的。

 GitLab-Runner可以分类两种类型：Shared Runner（共享型）和Specific Runner（指定型）。
Shared Runner：这种Runner是所有工程都能够用的。只有系统管理员能够创建Shared Runner。
Specific Runner：这种Runner只能为指定的工程服务。拥有该工程访问权限的人都能够为该工程创建Shared Runner。

 .gitlab-ci.yml 这个是在git项目的根目录下的一个文件，记录了一系列的阶段和执行规则。GitLab-CI在push后会解析它，根据里面的内容调用runner来运行。

## 1.4 Pipeline

.gitlab-ci.yal一次 Pipeline 其实相当于一次构建任务，里面可以包含多个流程，如安装依赖、运行测试、编译、部署测试服务器、部署生产服务器等流程。

+------------------+      +----------------+

|         | trigger |        |

|  Commit / MR  +---------->+  Pipeline  |

|         |      |        |

+------------------+      +----------------+

## 1.5 Stages

Stages 表示**构建阶段**，说白了就是上面提到的流程。我们可以在一次 Pipeline 中定义多个 Stages，这些 Stages 会有以下特点：

所有 Stages 会按照顺序运行，即当一个 Stage 完成后，下一个 Stage 才会开始

只有当所有 Stages 完成后，该构建任务 (Pipeline) 才会成功

如果任何一个 Stage 失败，那么后面的 Stages 不会执行，该构建任务 (Pipeline) 失败

+--------------------------------------------------------+

|                            |

| Pipeline                       |

|                            |

| +-----------+   +------------+   +------------+ |

| | Stage 1 |---->|  Stage 2 |----->|  Stage 3 | |

| +-----------+   +------------+   +------------+ |

|                            |

+--------------------------------------------------------+

## 1.6 Jobs

Jobs 表示**构建作业**，表示某个 Stage 里面执行的工作。我们可以在Stages 里面定义多个 Jobs，这些 Jobs 会有以下特点：

相同 Stage 中的 Jobs 会并行执行

相同 Stage 中的 Jobs 都执行成功时，该 Stage 才会成功

如果任何一个 Job 失败，那么该 Stage 失败，即该构建任务 (Pipeline) 失败

+------------------------------------------+

|                     |

|  Stage 1                 |

|                     |

| +---------+ +---------+ +---------+  |

| | Job 1 | | Job 2 | | Job 3 |  |

| +---------+ +---------+ +---------+  |

|                     |

+------------------------------------------+

## 1.7 对比

Gitlabci & Jenkins
轻量级，不需要复杂的安装手段。
・编译服务和代码仓库分离，耦合度低
配置简单，与 gitlab可直接适配。
插件丰富，支持语言众多
实时构建日志十分清晰，UI交互体验很好
有统一的web管理界面。
使用YAML进行配置，任何人都可以很方便的使用。
插件以及自身安装较为复杂。
没有统一的管理界面，无法统筹管理所有项
体量较大，不是很适合小型团队
・配置依赖于代码仓库，耦合度没有 Jenkins。低
同
Gitlabci有助于 Devops人员，例如敏捷开发中，开发与运维是
个人，最便捷的开发方式
Jenkinsci适合在多角色团队中，职责分明、配置与代码分离、插件丰富

# 2 GitLab

开源：CI/CD是开源 Gitlab社区版和专有 Gitlab企业版的一部分

易于学习：具有详细的入门文档。

无缝集成： Gitlab CI/CD是 Gitlabl的一部分，支持从计划到部署，具有出色的用户体验。

可扩展：测试可以在单独的计算机上分布式运行，可以根据需要添加任意数量的计算机。

更快的结果：每个构建可以拆分为多个作业，这些作业可以在多台计算机上并行运行。

针对交付进行了优化：多个阶段，手动部署，环境和变量。

## 2.1 安装配置

三种安装方式：RPM安装、Docker安装、K8s

1、获取镜像

```ruby
# gitlab-ce为稳定版本，后面不填写版本则默认pull最新latest版本
$ docker pull gitlab/gitlab-ce
```

2、运行镜像

```csharp
docker run -d  -p 443:443 -p 80:80 -p 222:22 --name some-gitlab --restart always -v /home/gitlab/config:/etc/gitlab -v /home/gitlab/logs:/var/log/gitlab -v /home/gitlab/data:/var/opt/gitlab gitlab/gitlab-ce
# -d：后台运行
# -p：将容器内部端口向外映射
# --name：命名容器名称
# -v：将容器内数据文件夹或者日志、配置等文件夹挂载到宿主机指定目录
```

3、配置

按上面的方式，gitlab容器运行没问题，但在gitlab上创建项目的时候，生成项目的URL访问地址是按容器的hostname来生成的，也就是容器的id。作为gitlab服务器，我们需要一个固定的URL访问地址，于是需要配置gitlab.rb（宿主机路径：/home/gitlab/config/gitlab.rb）。

```ruby
# gitlab.rb文件内容默认全是注释
$ vim /home/gitlab/config/gitlab.rb

# 配置http协议所使用的访问地址,不加端口号默认为80
external_url 'http://宿主机IP'

# 配置ssh协议所使用的访问地址和端口
gitlab_rails['gitlab_ssh_host'] = '宿主机IP'
gitlab_rails['gitlab_shell_ssh_port'] = 222 # 此端口是run时22端口映射的222端口
:wq #保存配置文件并退出

# 重启gitlab容器
$ docker restart gitlab
```

4、其他配置

由于GitLab十分消耗资源，所以这里贴出来一些降低资源消耗的配置，仅供参考，因为我的目的仅仅是启动GitLab做实验，所有资源配置都很低。需要修改的是宿主机 /home/gitlab/config/gitlab.rb文件（注：路径需根据自己自定的数据卷位置修改）

```bash
# 配置http协议所使用的访问地址,不加端口号默认为80
external_url 'http://10.106.127.128:8088'

# 配置ssh协议所使用的访问地址和端口
gitlab_rails['gitlab_ssh_host'] = '10.106.127.128:8088'
gitlab_rails['gitlab_shell_ssh_port'] = 222 # 此端口是run时22端口映射的222端口

unicorn['worker_memory_limit_min'] = "100 * 1 << 20"
unicorn['worker_memory_limit_max'] = "150 * 1 << 20" #减小内存

sidekiq['concurrency'] = 5 # 减小sidekiq的并发数
postgresql['shared_buffers'] = "64MB" #数据库缓存
postgresql['max_worker_processes'] = 2 #数据库并发数

unicorn['worker_timout'] = 60

unicorn['worker_processes'] = 2
```

## 2.2 基础使用

# 3 GitLab-Runner

## 3.1 docker-compose

1、原生的docker-compose安装Gitlab-Runner

```yaml
version: '3.1'
services:
 gitlab-runner:
  image: gitlab/gitlab-runner
  restart: always
  container_name: gitlab-runner
  hostname: gitlab-runner
  privileged: true
  volumes:
  	- /opt/soft/gitlab-runner/config:/etc/gitlab-runner
  	- /var/run/docker.sock:/var/run/docker.sock
```

此时遇到一个问题,我们在docker中,需要安装jdk和maven来进行打包,如果没有SCP命令的话,还要安装SCP进行远程发布,此时就需要修改镜像了,有两种方法

a)复制Gitlab-Runner的镜像,去掉注释.修改

b)直接继承Gitlab-Runner 在上面做修改

由于方法二的代码改动量较少,此时列出方法2的Dockerfile和docker-compose.yml(未整理)

## 3.2 docker

1、拉取镜像

```undefined
 docker pull gitlab/gitlab-runner:latest 
```

2、运行镜像

```jsx
docker run -d --name gitlab-runner --restart always \
  -v /docker/gitlab-runner/config:/etc/gitlab-runner \
  -v /var/run/docker.sock:/var/run/docker.sock \
  gitlab/gitlab-runner:latest
```

## 3.3 注册runner

这里使用的是specific Runner方式。在gitlab中 设置 --> CI/CD --> Runner(展开) 找到对应的配置信息。

![image-20201025195245176](../../插图/image-20201025195245176.png)

4、写一个 gitlab-ci.yml

```yaml
stages:
  - 拉取代码
  - MAVEN打包
  - 发布到远程服务器

我在搞代码:
  stage: 拉取代码
  script:
  - 'ls -al'
  - 'pwd'
我在打包:
  stage: MAVEN打包
  script:
  - 'echo $JAVA_HOME'
发布到测试环境:
  stage: 发布到远程服务器
  script:
  - 'echo Hello World'
发布到正式环境环境:
  stage: 发布到远程服务器
  script:
  - 'echo Hello Java'
```

![image-20201025195424055](../../插图/image-20201025195424055.png)