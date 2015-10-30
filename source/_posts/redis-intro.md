title: Redis简介
---
Redis 是开源的高性能 KV 存储引擎，提供 string、hash、list、set、sorted set、bitmap 等多种数据结构，可以用作缓存或存储。但原生 Redis 对集群化的支持比较弱，所以在大规模使用时，会在redis的上层再封装一层proxy来做分布式管理，twitter在2012年开源了内部的[Twemproxy](https://github.com/twitter/twemproxy)

## 快速使用

### 源码编译安装
从官网下载最新的稳定版源码包，解压编译
``` bash
wget http://download.redis.io/releases/redis-3.0.5.tar.gz
tar xzf redis-3.0.5.tar.gz
cd redis-3.0.5
make
```

### 启动

``` bash
[work@mingyi redis-3.0.5]$ ./src/redis-server
```
一般会有个redis.conf的配置文件来做一些具体的配置，包括端口、master/slave、持久化策略等

More info: [Redis configuration](http://redis.io/topics/config)

### 通过自带的cli工具连接

``` bash
[work@mingyi redis]$ ./src/redis-cli
127.0.0.1:6379> set foo hello
OK
127.0.0.1:6379> get foo
"hello"
```

More info: [Generating](http://hexo.io/docs/generating.html)

### Deploy to remote sites

``` bash
$ hexo deploy
```

More info: [Deployment](http://hexo.io/docs/deployment.html)
