## 1. 安装配置



#### 6.4 虚拟机克隆

> 虚拟机-管理-克隆-创建完整克隆

1. 网络设置

```properties
查看命令：cat /etc/udev/rules.d/70-persistent-ipoib.rules（ifconfig -a）
记录下eth1网卡的mac地址00:0c:29:50:bd:17
接下来，打开/etc/sysconfig/network-scripts/ifcfg-eth0
# vi /etc/sysconfig/network-scripts/ifcfg-eth0
将DEVICE="eth0"改成DEVICE="eth1"
将HWADDR="00:0c:29:8f:89:97"改成上面的eth1的mac地址HWADDR="00:0c:29:50:bd:17"
最后，重启网络命令：service network restart
```

#### 6.5 域名解析DNS