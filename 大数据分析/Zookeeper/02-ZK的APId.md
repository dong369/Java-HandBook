周期性向server发送心跳，server没有收到心跳，将sessionID过期。client无法继续访问，只能重新创建ZK的新对象。

client如果收不到server的响应，server停机，尝试连接到另一个主机。

zk客户端可以同步也可以异步。

同步方法阻塞到server响应为止，请求队列化。携带毁掉对象。请求执行完成后，进行毁掉。