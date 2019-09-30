# 1 SQL plus

Oracle的客户端。

```sql
sqlplus sys/bjsxt as sysdba;
alter user scott account unlock;
sqlplus /nolog
connect /as sysdba
```



| 实例名(SID ) | orcl         |
| ------------ | ------------ |
| 管理员帐号   | system/admin |
| 字符集       | utf-8        |
| 端口         | 1521         |



# 2 iSQL plus

http://192.168.2.117:5560/isqlplus/



# 3 PL SQL

## 环境配置

2.进入目录instantclient_11_2 打开tnsnames.ora文件，修改数据库连接地址

用记事本等文件打开，修改172.16.6.01为自己需要连接数据库的地址
ORCL =
(DESCRIPTION =
(ADDRESS = (PROTOCOL = TCP)(HOST = 172.16.6.01)(PORT = 1521))
(CONNECT_DATA =
(SERVER = DEDICATED)
(SERVICE_NAME = orcl)
)
)

3.修改环境变量
变量名：ORACLE_HOME 变量值：E:\PLSQL Developer\instantclient_11_2
变量名：TNS_ADMIN 变量值：E:\PLSQL Developer\instantclient_11_2
变量名：NLS_LANG 变量值：SIMPLIFIED CHINESE_CHINA.ZHS16GBK
修改Path变量：在后面添加 E:\PLSQL Developer\instantclient_11_2



# 4 Toad











