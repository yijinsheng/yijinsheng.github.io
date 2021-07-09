## 集群部署

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