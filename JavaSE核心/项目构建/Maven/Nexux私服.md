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