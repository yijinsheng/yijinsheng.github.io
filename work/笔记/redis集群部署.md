# 集群部署

1. redis.conf
```
port 6379
bind 0.0.0.0
daemonize no
pidfile /var/run/redis_6379.pid
cluster-enabled yes
cluster-config-file nodes_6379.conf
cluster-node-timeout 15000
appendonly yes
```

2. 启动redis

```
redis-server ./redis.conf
```

3. 设置集群

```
redis-cli --cluster create 172.38.0.11:6379 172.38.0.12:6379 172.38.0.13
:6379 172.38.0.14:6379 172.38.0.15:6379 172.38.0.16:6379 --cluster-replicas 1 
```

4. 设置密码

```
redis-cli -c -h 192.168.43.86 -p 6379
config set masterauth passwd123 
config set requirepass passwd123 
config rewrite 

```

5.Redis集群Hash槽分配异常 CLUSTERDOWN Hash slot not served的解决方式
在以下三台虚拟机机器=搭建Redis集群——

```shell
192.168.200.160

192.168.200.161

192.168.200.162
```
启动三台Redis集群，然后连接其中一台客户端，随便set一个指令，测试集群是否可行，结果报出异常(error) **CLUSTERDOWN Hash slot not served**提示——
```shell
[app@hadoop-nn bin]$ ./redis-cli -c -h 192.168.200.162
192.168.200.162:6379> set zhu "test"
(error) CLUSTERDOWN Hash slot not served
```
首先，先看一下集群各个节点是否能互相发现，执行以下指令查看各个节点连接情况——

```shell
192.168.200.162:6379> cluster nodes
8c5809df064ad7234c6475555411afda026c230f :6379@16379 myself,master - 0 0 0 connected

```
接着再检查一下当前集群状态，发现目前状态为fail，说明集群没有互连成功——

```shell
192.168.200.162:6379> cluster info
cluster_state:fail
cluster_slots_assigned:0
cluster_slots_ok:0
cluster_slots_pfail:0
cluster_slots_fail:0
cluster_known_nodes:1
cluster_size:0
cluster_current_epoch:0
cluster_my_epoch:0
cluster_stats_messages_sent:0
cluster_stats_messages_received:0
```
发现，三台Redis搭建的集群没有互相发现，故而，只需要在其中一台客户端上执行以下指令，手动帮助该节点去发现其他两个节点，因集群是互连的，所以只需要在其中一台上手动发现另外两台即可——

```shell
192.168.200.162:6379> cluster meet 192.168.200.160 6379
OK
192.168.200.162:6379> cluster meet 192.168.200.161 6379
OK
```
完成以上指令，查看各个节点状态，发现当前节点已经能发现其他两台机器节点了——

```shell
192.168.200.162:6379> cluster nodes
a0cf910effc52eda7c5561746c42f8bcd710f735 192.168.200.161:6379@16379 master - 0 1639410795898 0 connected
5e5f08f9ec39910cc250239b4f44e701d4b831f5 192.168.200.160:6379@16379 master - 0 1639410794885 1 connected
8c5809df064ad7234c6475555411afda026c230f 192.168.200.162:6379@16379 myself,master - 0 1639410795000 2 connected
```
再测试集群状态，发现状态依然还是失败，且还报CLUSTERDOWN Hash slot not served异常——

```shell
192.168.200.162:6379> cluster info
cluster_state:fail
cluster_slots_assigned:0
cluster_slots_ok:0
cluster_slots_pfail:0
cluster_slots_fail:0
cluster_known_nodes:3
cluster_size:0
cluster_current_epoch:2
cluster_my_epoch:2
cluster_stats_messages_ping_sent:26
cluster_stats_messages_pong_sent:30
cluster_stats_messages_meet_sent:3
cluster_stats_messages_sent:59
cluster_stats_messages_ping_received:30
cluster_stats_messages_pong_received:29
cluster_stats_messages_received:59
192.168.200.162:6379> set zhu "test"
(error) CLUSTERDOWN Hash slot not served
```
到这一步，说明当前集群存在hash槽异常情况，那么，可以执行以下指令修复下——

```shell
[app@hadoop-nn bin]$ ./redis-cli --cluster fix 192.168.200.162:6379
```
回车执行，顿时就会运行打印很多以下信息，说明正在对16384个hash槽重新分配——

```shell
>>> Covering slot 10620 with 192.168.200.162:6379
>>> Covering slot 3059 with 192.168.200.162:6379
>>> Covering slot 9764 with 192.168.200.162:6379
>>> Covering slot 11335 with 192.168.200.162:6379
>>> Covering slot 6368 with 192.168.200.162:6379
>>> Covering slot 4884 with 192.168.200.162:6379
>>> Covering slot 15271 with 192.168.200.162:6379
>>> Covering slot 5109 with 192.168.200.162:6379
......
```
等运行完成后，我们再检查一下集群状态，发现状态已经由刚刚的fail变出ok了，说明hash槽已经正确分配——

```shell
192.168.200.162:6379> cluster info
cluster_state:ok
cluster_slots_assigned:16384
cluster_slots_ok:16384
cluster_slots_pfail:0
cluster_slots_fail:0
cluster_known_nodes:3
cluster_size:3
cluster_current_epoch:19
cluster_my_epoch:18
cluster_stats_messages_ping_sent:1514
cluster_stats_messages_pong_sent:1486
cluster_stats_messages_meet_sent:3
cluster_stats_messages_sent:3003
cluster_stats_messages_ping_received:1486
cluster_stats_messages_pong_received:1517
cluster_stats_messages_received:3003
```
最后，在其中一台集群上输入以下指令测试下，没有报异常了——

```shell
192.168.200.162:6379> set test zhu
OK
```

另外，在其他两台机器上，输入以下指令，都可以获取到192.168.200.162机器redis输入的测试k-v值了

```shell
192.168.200.160:6379> get test
-> Redirected to slot [6918] located at 192.168.200.162:6379
"zhu"
```

参考:
- [Redis集群Hash槽分配异常 CLUSTERDOWN Hash slot not served的解决方式原](https://cloud.tencent.com/developer/article/1919678)