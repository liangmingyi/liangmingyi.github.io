title: Redis简介
date: 2015-10-30 23:00:02
tags: redis
---
Redis 是开源的高性能 KV 存储引擎，提供 string、hash、list、set、sorted set、bitmap 等多种数据结构，可以用作缓存或存储。但原生 Redis 对集群化的支持比较弱，所以在大规模使用时，会在redis的上层再封装一层proxy来做分布式管理，twitter在2012年开源了内部的[Twemproxy](https://github.com/twitter/twemproxy)。

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

### 各种操作API

翻翻官方的文档好了，各种数据类型以及各种操作的方法，[传送门](http://redis.io/commands)

## 性能

一般来讲，吞吐上很难触及到 Redis 的瓶颈，主要需要关注的是耗时，性能测试和使用场景有非常大的关联。

## 应用场景

### 访问计数

可利用 Redis 的原子计数, INCR, INCRBY 等命令实现最典型的 Redis 应用。如文章的浏览计数，最典型的浏览转提交的场景，每次浏览都意味着一次提交操作。

* key 一般是 id（可能是字符串或数字），value 是数字
* 操作类型有递增和（INCRBY）获取（GET），也可以支持数据过期
* 通过pipeline可以支持批量递增和批量获取
* 高性能的支持（单redis进程7w+ qps，还可以再分布式）

一般使用 Redis 的 string 数据类型。但内存消耗比较大，每对 key-string 平均消耗70-80字节。 内存消耗和 key 的长度也有关系，因此保守可以按每对 key-string 消耗 100 字节内存来估算。

如果 key 的数目非常大，推荐使用 Redis 的 Hash 数据类型，利用多级 key 的思路。 具体方式是：将原来的 key 拆分为两部分，如 12345678 里的高位 12345 作为 hash 的 key，低位 678 作为 hash 结构内部的 key。 这样内存消耗可以降低到原来的 1/3 左右。原理是 hash 内部会对数据做压缩（hash-zipmap）。

### Profile

可利用 Hash，List 等数据结构实现适用范围非常广的场景

* 浏览量大、或更新量大、或二者皆有
* 数据比较短，往往是一组标记、或者一组 id
* 可以局部修改，整体获取，简称零存整取

如是简单的标记位，可以把 Redis 的 string 看做一个标记位数组，支持按 bit 获取和设置。如将用户 id 做 key，第 2 个 bit 存储用户是否会员，第 3 个 bit 标记用户的性别。

如是三级结构，key->field->value，需要使用 Redis 的 hash。如，将用户 id 作为 key，配置项作为 field，配置数据作为 value。

如没有排序需求，可以使用 Redis 的 List。如，将用户 id 作为 key，数据项作为 value。

### 排行榜

可利用 Sorted Set 数据结构实现。

如贴吧里吧的用户积分排名、文库里文档的下载排行，等等。拉链类型的应用。榜单长度可长可短，榜单个数可大可小。

* 榜单里添加、删除 item
* 榜单里修改 item、或递增 item 的值。比如增加用户的积分
* 获取某个 item 的名次。如用户在榜单里的排名
* 分页获取榜单。如查询第 10 页榜单，或者最后一页榜单
* 榜单一般是按积分倒排，也可以正排

这种场景需要使用 Redis 的 Sorted Set 数据类型。以贴吧的用户积分排名为例，将吧 id 作为 key，用户 id 作为 Sorted Set 的 member，积分作为 sorted set 的 score。 按贴吧的实际需求，如果积分相同，再按另外一个字段 X 排序，这样就需要将积分 + 字段 X 共同拼为 score。

支持超长拉链和高性能的操作。单redis进程在5000w长的拉链下，保守估计仍可以保持3.6w+的qps（极限性能4.8w时cpu 92-97%）。无论添加还是拉取操作，因为这些操作的时间复杂度都是O(logn)。

* 添加、修改item：ZADD，时间复杂度O(logn)
* 删除item：ZREM，时间复杂度O(logn)
* 获取item的名次：ZRANK、ZREVRANK，时间复杂度O(logn)
* 分页获取拉链：ZRANGE、ZREVRANGE，时间复杂度O(logn)

### Cache

从功能上，可以把 Redis 当做 Cache 来用。

Redis 内部支持多种淘汰方式：如

* 在有过期时间的数据集合里 LRU 淘汰
* 所有 key 按 LRU 淘汰
* 在有过期时间的数据集合里 random 淘汰
* 所有 key 里 random 淘汰
* 在有过期时间的数据集合里按 TTL 淘汰最近一个将要过期的数据
* 不淘汰，当内存满时拒绝新增数据

Redis 作为 Cache 有两点不足：

+ 为了节省内存，Redis 的 LRU 和 TTL 的算法并不精确。以 LRU 为例，它没有记录完整的 LRU 链表，而是采用抽样的办法，先随即选中 N 个候选，再从中选择最近未访问的 key，再淘汰。
+ 由于使用的内存分配器和 Memcache 的 Slab 机制特点的不同，Redis 在大 Value 上的内存使用率不高。

因此，如果是 Cache 类是需求，使用 Memcache 服务更合适。

### 缓冲区队列

可利用 List，Pub/Sub 实现简单的队列，或发布/订阅结构，但这种解决方案并不可靠。
