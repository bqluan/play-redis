### 00 - Configuration: quorum = 2 ###
```
       +----+
       | M1 |
       | S1 |
       +----+
          |
+----+    |    +----+
| R2 |----+----| R3 |
| S2 |         | S3 |
+----+         +----+
```


### 01 - Install Redis ###
分别在三个节点安装redis
```
yum install gcc
cd /usr/local
curl -O http://download.redis.io/releases/redis-4.0.1.tar.gz
tar xzf redis-4.0.1.tar.gz
cd redis-4.0.1
make
```

### 02 - M1 ###
```
cd /usr/local/redis-4.0.1

vi redis.conf
bind 172.16.18.1
protected-mode no
port 6379
daemonize yes
pidfile /var/run/redis_6379.pid
syslog-enabled yes
min-slaves-to-write 1
min-slaves-max-lag 10

./src/redis-server /usr/local/redis-4.0.1/redis.conf
```

### 03 - R2 ###
```
cd /usr/local/redis-4.0.1

vi redis.conf
bind 172.16.18.2
protected-mode no
port 6379
daemonize yes
pidfile /var/run/redis_6379.pid
syslog-enabled yes
min-slaves-to-write 1
min-slaves-max-lag 10
slaveof 172.16.18.1 6379

./src/redis-server /usr/local/redis-4.0.1/redis.conf
```

### 04 - R3 ###
```
cd /usr/local/redis-4.0.1

vi redis.conf
bind 172.16.18.3
protected-mode no
port 6379
daemonize yes
pidfile /var/run/redis_6379.pid
syslog-enabled yes
min-slaves-to-write 1
min-slaves-max-lag 10
slaveof 172.16.18.1 6379

./src/redis-server /usr/local/redis-4.0.1/redis.conf
```

### 05 - S1 ###
```
cd /usr/local/redis-4.0.1

vi sentinel.conf
bind 172.16.18.1
protected-mode no
daemonize yes
syslog-enabled yes
syslog-ident redis-sentinel
syslog-facility local1
dir /usr/local/redis-4.0.1
sentinel monitor mymaster 172.16.18.1 6379 2

./src/redis-sentinel /usr/local/redis-4.0.1/sentinel.conf
```

### 05 - S2 ###
```
cd /usr/local/redis-4.0.1

vi sentinel.conf
bind 172.16.18.2
protected-mode no
daemonize yes
syslog-enabled yes
syslog-ident redis-sentinel
syslog-facility local1
dir /usr/local/redis-4.0.1
sentinel monitor mymaster 172.16.18.1 6379 2

./src/redis-sentinel /usr/local/redis-4.0.1/sentinel.conf
```

### 05 - S3 ###
```
cd /usr/local/redis-4.0.1

vi sentinel.conf
bind 172.16.18.3
protected-mode no
daemonize yes
syslog-enabled yes
syslog-ident redis-sentinel
syslog-facility local1
dir /usr/local/redis-4.0.1
sentinel monitor mymaster 172.16.18.1 6379 2

./src/redis-sentinel /usr/local/redis-4.0.1/sentinel.conf
```
