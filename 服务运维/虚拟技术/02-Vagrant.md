# 常见命令

| 命令               | 解释                  |
| ------------------ | --------------------- |
| vagrant box list   | 查看目前已有的box     |
| vagrant box add    | 新增加一个box         |
| vagrant box remove | 删除指定box           |
| vagrant init       | 初始化配置vagrantfile |
| vagrant up         | 启动虚拟机            |
| vagrant ssh        | ssh登录虚拟机         |
| vagrant suspend    | 挂起虚拟机            |
| vagrant reload     | 重启虚拟机            |
| vagrant halt       | 关闭虚拟机            |
| vagrant status     | 查看虚拟机状态        |
| vagrant destroy    | 删除虚拟机            |



vagrant init puppetlabs/centos-7.2-64-nocm

```properties
# -*- mode: ruby -*-
# vi: set ft=ruby :

servers = {
    :master => '192.168.2.11',
    :node01 => '192.168.2.12',
    :node02 => '192.168.2.13',
    :node03 => '192.168.2.14'
}

Vagrant.configure("2") do |config|
  config.vm.box = "centos/7"
  config.vm.box_check_update = false

  servers.each do |server_name, server_ip|
      config.vm.define server_name do |server_config|
          server_config.vm.hostname = "#{server_name.to_s}"
          server_config.vm.network :private_network, ip: server_ip
          server_config.vm.provider "virtualbox" do |vb|
              vb.name = server_name.to_s
                        vb.memory = "2048"
                        vb.cpus = 1
         end
      end
  end
end
```

