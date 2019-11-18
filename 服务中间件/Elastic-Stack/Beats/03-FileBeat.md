# 1 Filebeat简介

读取日志文件，但不做数据的解析处理
保证数据" At Least once"至少被读取一次，即数据不会丢
其他能力
处理多行数据
解析JSON格式数据
简单的过滤功能



# 2 Filebeat使用

安装-开箱即用
配置- filebeat yml
配置模板- index template
配置 Kibana Dashboards
运行

## 2.1 配置文件

filebeat.yml

```yaml
filebeat.prospectors:
  type: log
  paths: 
    
```

