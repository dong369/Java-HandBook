# 1 基础配置

nexus服务配置，在搭建号nexus服务中，有两个类型的库可以推送，分别是releases和snapshots，在本地搭建的nexus服务中，找到这两个库对应的URL

 http://localhost:8082/repository/maven-releases/

 http://localhost:8082/repository/maven-snapshots/

在安装好的maven服务中，找到settingxml 配置文件，作如下配置：

```xml
<servers>
      <server>
            <id>maven-releases</id>
            <username>admin</username>
            <password>admin123</password>
      </server>
      <server>
            <id>maven-snapshots</id>
            <username>admin</username>
            <password>admin123</password>
      </server>
</servers>
```

Maven推送，在项目pom.xml文件中，做如下配置：

```xml
<distributionManagement>
      <repository>
        <id>maven-releases</id>
        <url>http://localhost:8082/repository/maven-releases/</url>
      </repository>
      <snapshotRepository>
        <id>maven-snapshots</id>
        <url>http://localhost:8082/repository/maven-snapshots/</url>
      </snapshotRepository>
    </distributionManagement>
```

配置完成后，回到该项目文件的根目录先后执行 maven clean install / maven clean deploy

注意：id和name和代理仓库的Name一致

```xml
<repositories>
	<repository>
		<id>maven-public</id>
		<name>maven-public</name>
		<url>http://192.168.2.109:8888/repository/maven-public/</url>
		<snapshots>
			<enabled>true</enabled>
		</snapshots>
		<releases>
			<enabled>true</enabled>
		</releases>
	</repository>
</repositories>
```



```properties
mvn deploy:deploy-file 
  -DgroupId=com.example
  -DartifactId=test
  -Dversion=0.0.1
  -Dpackaging=jar
  -Dfile=E:\workspace\test\WebRoot\WEB-INF\lib\test-0.0.1.jar
  -Durl=http://nexus.example.com:8081/repository/3rd-repo/
  -DrepositoryId=Nexus
```





https://www.cnblogs.com/chywx/p/11227151.html



1、先将本地maven/repository仓库打一个完整的zip压缩包
2、上传到linux目录，如：/opt
3、解压repository.zip
4、进入repository目录
5、创建touch mavenimport.sh脚本，写入以下内容；、

```shell
#!/bin/bash
# copy and run this script to the root of the repository directory containing files
# this script attempts to exclude uploading itself explicitly so the script name is important
# Get command line params

while getopts ":r:u:p:" opt; do
	case $opt in
		r) REPO_URL="$OPTARG"
		;;
		u) USERNAME="$OPTARG"
		;;
		p) PASSWORD="$OPTARG"
		;;
	esac
done

find . -type f -not -path './mavenimport\.sh*' -not -path '*/\.*' -not -path '*/\^archetype\-catalog\.xml*' -not -path '*/\^maven\-metadata\-local*\.xml' -not -path '*/\^maven\-metadata\-deployment*\.xml' | sed "s|^\./||" | xargs -I '{}' curl -u "$USERNAME:$PASSWORD" -X PUT -v -T {} ${REPO_URL}/{} ;
```

6、输入chmod a+x mavenimport.sh进行可执行授权
7、执行导入命令

```shell
./mavenimport.sh -u admin -p admin123 -r http://ip:8081/repository/maven-releases/
```

8、等待全部导入完毕后，在Nexus上刷新即可看到已导入的jar

Nexus3界面上传jar包