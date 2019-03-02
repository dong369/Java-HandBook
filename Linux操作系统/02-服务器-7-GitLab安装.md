[TOC]

## 0.前提

安装好docker,并配置好docker镜像源,机器能外网（如果不能联网，可以用RPM安装方式）

代码是重要的资产，最好定期演练备份和恢复


## 1.创建GitLab数据目录

`mkdir -p /opt/soft/gitlab`

## 2.创建docker文件
创建文件
`vi start.sh`
把以下内容粘贴进文件（注意IP换了，SSH端口22也要换被占用了，不换的话可以换docker的映射端口）

```shell
#!/bin/base
docker run \
    --detach \
    --publish 443:443 \
    --publish 80:80 \
    --publish 22:22 \
    --name gitlab \
    --hostname 192.168.100.13 \
    --env GITLAB_OMNIBUS_CONFIG="external_url 'http://192.168.100.13/'; gitlab_rails['lfs_enabled'] = true;" \
    --restart unless-stopped \
    --volume /opt/soft/gitlab/etc:/etc/gitlab \
    --volume /opt/soft/gitlab/log:/var/log/gitlab \
    --volume /opt/soft/gitlab/data:/var/opt/gitlab \
    beginor/gitlab-ce:11.3.0-ce.0
```

## 3.下载并运行

`sh start.sh`

## 4.升级

小版本升级（例如从 8.8.2 升级到 8.8.3）， 参照官方的说明， 将原来的容器停止， 然后删除：
```shell
docker stop gitlab
docker rm gitlab
```

然后重新拉一个新版本的镜像下来，

`docker pull beginor/gitlab-ce:11.3.0-ce.0`

还使用原来的运行命令运行(改一下版本号，为了以防万一，最好备份代码)
```shell
#!/bin/base
docker run \
    --detach \
    --publish 443:443 \
    --publish 80:80 \
    --publish 22:22 \
    --name gitlab \
    --hostname 10.9.254.162 \
    --env GITLAB_OMNIBUS_CONFIG="external_url 'http://10.9.254.162/'; gitlab_rails['lfs_enabled'] = true;" \
    --restart unless-stopped \
    --volume /opt/soft/gitlab/etc:/etc/gitlab \
    --volume /opt/soft/gitlab/log:/var/log/gitlab \
    --volume /opt/soft/gitlab/data:/var/opt/gitlab \
    beginor/gitlab-ce:11.3.0-ce.0
```


GitLab 在初次运行的时候会自动升级， 为了预防万一， 还是建议先备份一下`/opt/soft/gitlab`这个目录。

大版本升级（例如从 8.7.x 升级到 8.8.x）用上面的操作有可能会出现错误， 如果出现错误可以尝试登录到容器内部， 可以用 `docker exec` ， 也可以用 ssh ， 依次执行下面的命令：

```shell
gitlab-ctl reconfigure
gitlab-ctl restart
```