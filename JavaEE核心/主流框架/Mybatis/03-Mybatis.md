# 1. 环境搭建

## 1.1 Mybatis

```properties

```



```yaml
mybatis:
  config-location: classpath:mybatis-config.xml
  mapper-locations: classpath:mapper/*.xml
  type-aliases-package: com.tianjian.*.entity
```

## 1.2 数据源

> Spring boot 2.0 以后默认的数据源HikariCP！

```properties
<dependency>
	<groupId>com.zaxxer</groupId>
	<artifactId>HikariCP</artifactId>
	<version>3.3.1</version>
</dependency>
```

## 1.3 分页插件

```properties
<!-- 分页插件 -->
<dependency>
	<groupId>com.github.pagehelper</groupId>
	<artifactId>pagehelper-spring-boot-starter</artifactId>
	<version>1.2.5</version>
</dependency>
```



```yaml
pagehelper:
  helperDialect: mysql
  reasonable: true
  supportMethodsArguments: true
  params: count=countSql
```

# 2. 数据参数

1. #{}是**预编译处理**，${}是字符串替换。

2. Mybatis在处理#{}时，会将sql中的#{}替换为?号，调用PreparedStatement的set方法来赋值；

3. Mybatis在处理${}时，就是把${}替换成变量的值；

4. 使用#{}可以有效的防止SQL注入，提高系统安全性。

## 2.1 顺序传递参数

```properties
public User selectUser(String name, int deptId);

<select id="selectUser" resultType="com.wyj.entity.po.User">
	select * from user where userName = #{0} and deptId = #{1}
</select>
```

## 2.2 注解@Param传递参数

```properties
public User selectUser(@Param("userName") String name, int @Param("deptId") id);

<select id="selectUser" resultType="com.wyj.entity.po.User">
	select * from user where userName = #{userName} and deptId = #{deptId}
</select>
```

## 2.3 使用Map集合传递参数

```properties
public User selectUser(Map<String, Object> params);

<select id="selectUser" parameterType="java.util.Map" resultType="com.wyj.po.User">
	select * from user where userName = #{userName} and deptId = #{deptId}
</select>
```

## 2.4 使用JavaBean实体类传递参数

```properties
public User selectUser(User user);

<select id="selectUser" parameterType="com.wyj.entity.po.User" resultType="com.wyj.entity.po.User">
	select * from user where userName = #{userName} and deptId = #{deptId}
</select>
```

# 3. 查询模型

## 3.2 条件查询



## 3.1 模糊查询

1. #{}可以有效避免SQL注入，也是推荐的写法

```properties
'%'||#{param}#'%'
"%"#{param}"%"
concat(concat('%',#{keyword},'%'))
```

2. ${}

```properties
'%${param}%'
```





# 3. 数据分页





