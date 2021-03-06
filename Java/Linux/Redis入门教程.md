# Redis入门教程

## 1.NoSQL简介

* key-value:Redis
* 列存储： Hbase
* 文档; MongoDB
* 图形：Neo4J

## 2.Redis简介

REmote DIctionary Server(Redis) 是一个由Salvatore Sanfilippo写的key-value存储系统。

Redis是一个开源的使用ANSI C语言编写、遵守BSD协议、支持网络、可基于内存亦可持久化的日志型、Key-Value数据库，并提供多种语言的API。

它通常被称为数据结构服务器，因为值（value）可以是 字符串(String), 哈希(Hash), 列表(list), 集合(sets) 和 有序集合(sorted sets)等类型。

## 3. Redis安装

### 1. 下载

官网`https://redis.io/download` 下载文件，这里下的是`redis-5.0.3.tar.gz`

然后上传到服务器。这里是放在了`/usr/software`目录下



### 2. 环境准备

**安装编译源码所需要的工具和库**:

```linux
# yum install gcc gcc-c++ ncurses-devel perl 
```

### 3. 解压安装

#### 1. 解压

```linux
[root@localhost software]# tar -zxvf redis-5.0.3.tar.gz -C /usr/local
//解压到/usr/local目录下
```

#### 2.编译

进入刚才解压的后的文件夹`redis-5.0.3`进行编译

```linux
[root@localhost redis-5.0.3]# make
```

如果提示`Hint: It's a good idea to run 'make test' ;)`就说明编译ok了，接下来进行安装。

#### 3. 安装

进入`src`目录下

```linux
[root@localhost redis-5.0.3]# cd src/
[root@localhost src]# make install
```

出现下面的提示代表安装ok

```shell
    CC Makefile.dep

Hint: It's a good idea to run 'make test' ;)

    INSTALL install
    INSTALL install
    INSTALL install
    INSTALL install
    INSTALL install

```

#### 4. 文件复制

创建两个文件夹来存放Redis命令和配置文件。

```linux
[root@localhost local]# mkdir -p /usr/local/redis/etc
[root@localhost local]# mkdir -p /usr/local/redis/bin
```

把``redis-5.0.3`下的`redis.conf`复制到`/usr/local/redis/etc`目录下

```linux
[root@localhost redis-5.0.3]# cp redis.conf /usr/local/redis/etc/
```

把`redis-5.0.3/src`里的`mkreleasehdr.sh`、`redis-benchmark`、`redis-check-aof`、`redis-check-rdb`、`redis-cli`、`redis-server` 文件移动到`/usr/local/redis/bin`下

```linux
[root@localhost src]# cp mkreleasehdr.sh redis-benchmark redis-check-aof redis-check-rdb redis-cli redis-server /usr/local/redis/bin
```

### 4. 启动

#### 1. 前台启动

启动时并指定配置文件：.

```linux
[root@localhost etc]# /usr/local/redis/bin/redis-server /usr/local/redis/etc/redis.conf
```

出现如下提示代表启动成功

```shell
[root@localhost etc]# /usr/local/redis/bin/redis-server /usr/local/redis/etc/redis.conf
10869:C 05 Mar 2019 13:33:39.041 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
10869:C 05 Mar 2019 13:33:39.042 # Redis version=5.0.3, bits=64, commit=00000000, modified=0, pid=10869, just started
10869:C 05 Mar 2019 13:33:39.042 # Configuration loaded
10869:M 05 Mar 2019 13:33:39.044 * Increased maximum number of open files to 10032 (it was originally set to 1024).
                _._                                                  
           _.-``__ ''-._                                             
      _.-``    `.  `_.  ''-._           Redis 5.0.3 (00000000/0) 64 bit
  .-`` .-```.  ```\/    _.,_ ''-._                                   
 (    '      ,       .-`  | `,    )     Running in standalone mode
 |`-._`-...-` __...-.``-._|'` _.-'|     Port: 6379
 |    `-._   `._    /     _.-'    |     PID: 10869
  `-._    `-._  `-./  _.-'    _.-'                                   
 |`-._`-._    `-.__.-'    _.-'_.-'|                                  
 |    `-._`-._        _.-'_.-'    |           http://redis.io        
  `-._    `-._`-.__.-'_.-'    _.-'                                   
 |`-._`-._    `-.__.-'    _.-'_.-'|                                  
 |    `-._`-._        _.-'_.-'    |                                  
  `-._    `-._`-.__.-'_.-'    _.-'                                   
      `-._    `-.__.-'    _.-'                                       
          `-._        _.-'                                           
              `-.__.-'                                               

10869:M 05 Mar 2019 13:33:39.046 # WARNING: The TCP backlog setting of 511 cannot be enforced because /proc/sys/net/core/somaxconn is set to the lower value of 128.
10869:M 05 Mar 2019 13:33:39.046 # Server initialized
10869:M 05 Mar 2019 13:33:39.047 # WARNING overcommit_memory is set to 0! Background save may fail under low memory condition. To fix this issue add 'vm.overcommit_memory = 1' to /etc/sysctl.conf and then reboot or run the command 'sysctl vm.overcommit_memory=1' for this to take effect.
10869:M 05 Mar 2019 13:33:39.047 # WARNING you have Transparent Huge Pages (THP) support enabled in your kernel. This will create latency and memory usage issues with Redis. To fix this issue run the command 'echo never > /sys/kernel/mm/transparent_hugepage/enabled' as root, and add it to your /etc/rc.local in order to retain the setting after a reboot. Redis must be restarted after THP is disabled.
10869:M 05 Mar 2019 13:33:39.047 * DB loaded from disk: 0.000 seconds
10869:M 05 Mar 2019 13:33:39.047 * Ready to accept connections

```

**退出**：`CTRL+C`

#### 2. 后台启动

(**注意要使用后台启动需要修改`redis.conf`里的`daemonize`改为`yes`**)

```shell
[root@localhost etc]# vim redis.conf 
#主要修改下面这个daemonize
# By default Redis does not run as a daemon. Use 'yes' if you need it.
# Note that Redis will write a pid file in /var/run/redis.pid when daemonized.
# daemonize no 把这个改为yes no代表前台启动 yes代表后台启动
daemonize yes

# The working directory.
#
# The DB will be written inside this directory, with the filename specified
# above using the 'dbfilename' configuration directive.
#
# The Append Only File will also be created inside this directory.
#
# Note that you must specify a directory here, not a file name.
# dir ./  这个是工作区 默认为./ 即上级目录 这里也改一下
dir /usr/local/redis/etc
```

再次启动

```linux
[root@localhost etc]# /usr/local/redis/bin/redis-server /usr/local/redis/etc/redis.conf
```

**验证启动是否成功**：

```linux
[root@localhost etc]#/ps aux|grep redis

root      11012  0.2  0.2 153880  2344 ?        Ssl  13:36   0:00 /usr/local/redis/bin/redis-server 127.0.0.1:6379
root      11126  0.0  0.0 112708   976 pts/2    R+   13:39   0:00 grep --color=auto redis
```

redis启动成功端口号也是默认的6379。

### 5. 使用

#### 1.进入客户端

进入redis客户端

```linux
[root@localhost etc]# /usr/local/redis/bin/redis-cli 
127.0.0.1:6379> 
```

成功进入Redis客户端

随意操作一下：

```shell
127.0.0.1:6379> keys *
(empty list or set)
127.0.0.1:6379> set name illusory
OK
127.0.0.1:6379> keys *
1) "name"
127.0.0.1:6379> get name
"illusory"
127.0.0.1:6379> set age 22
OK
127.0.0.1:6379> get age
"22"
127.0.0.1:6379> quit #退出命令
[root@localhost etc]# 
```

**退出客户端**：`quit`

#### 2.关闭Redis

退出redis服务的三种方法：

*  1.`pkill redis-server`、
* 2.`kill 进程号`、
* 3.`/usr/local/redis/bhi/redis-cli shutdown`

#### 4. dump.rdb文件

由于前面配置文件中配置的是`dir /usr/local/redis/etc`,所以Redis的所有数据都会存储在这个目录

```linux
[root@localhost etc]# ll
total 68
-rw-r--r--. 1 root root    92 Mar  5 13:36 dump.rdb
-rw-r--r--. 1 root root 62174 Mar  5 13:36 redis.conf
```

确实有这个文件。**这个文件删除后Redis中的数据就真的没了**。



## 4. Redis基础数据类型详解

Redis一共分5种数据类型：``String`、``Hash`、``List`、``Set`、``ZSet`

### 1. String

```java
设置值：set key value-->set myname illusory   //同一个key多次set会覆盖
获取值：get key  ------>get myname
删除值：del key-------->del myname
```

其他set方法：

`setnx(not exist)`: 如果key不存在就设置值，返回1;存在就不设置，返回0；

```shell
#格式：setnx key value
127.0.0.1:6379> set myname illusory
OK
127.0.0.1:6379> setnx myname cloud   #myname 已经存在了 返回0
(integer) 0
127.0.0.1:6379> get myname  # 值也没有发生变化
"illusory"
```

`setex(expired)`: 设置数据过期时间，数据只存在一段时间

```shell
#格式：setnx key seconds value;
setnx vercode 60 123456； 
#设置key-->vercode有效时间60s，60s内获取值为123456,60s后返回nil（Redis中nil表示空）
```

 ```shell
127.0.0.1:6379> setex vercode 5 123456
OK
127.0.0.1:6379> get vercode #时间没到 还能查询到
"123456"
127.0.0.1:6379> get vercode  #5s到了 数据过期 查询返回nil
(nil)
 ```



`setrange`：替换字符串

```shell
#格式：setrange key offset value
set email 123456789@gmail.com
setrange email 10 qqqqq # 从第10位开始替换(不包括第10位) 后面跟上用来替换的字符串

```

```shell
127.0.0.1:6379> set email 123456789@gmail.com
OK
127.0.0.1:6379> get email
"123456789@qqail.com"
127.0.0.1:6379> setrange email 10 qqqqq
(integer) 19
127.0.0.1:6379> get email
"123456789@qqqqq.com"
```

mset：一次设置多个值

`mset key1 value1 key2 value2 ...keyn valuen`

mget：一次获取多个值

`mget key1 key2 key3...keyn`

```shell
127.0.0.1:6379> mset k1 111 k2 222 k3 333 
OK
127.0.0.1:6379> mget k1 k2 k3
1) "111"
2) "222"
3) "333"

```

`getset`: 返回旧值并设置新值

```shell
#格式 getset key value
getset name cloud #将name设置为cloud并放回name的旧值
```

```shell
127.0.0.1:6379> set name illusory
OK
127.0.0.1:6379> get name
"illusory"
127.0.0.1:6379> getset name cloud
"illusory"
127.0.0.1:6379> get name
"cloud"
```

`incr/decr`:对一个值进行递增或者递减操作。

```shell
# 格式 incr key/decr key
incr age #age递增1
decr age #age递减1
```

```shell
127.0.0.1:6379> get age
"22"
127.0.0.1:6379> incr age #递增
(integer) 23
127.0.0.1:6379> get age
"23"
127.0.0.1:6379> decr age #递减
(integer) 22
127.0.0.1:6379> get age
"22"
```

`incrby/decrby`:对一个值按照一定`步长`进行递增或者递减操作。

```shell
# 格式 incrby key increment/decrby key increment
incrby age 3 #age递增3
decrby age 3 #age递减3
```

```shell
127.0.0.1:6379> get age
"22"
127.0.0.1:6379> incrby age 3
(integer) 25
127.0.0.1:6379> get age
"25"
127.0.0.1:6379> decrby age 3
(integer) 22
127.0.0.1:6379> get age
"22"

```

`append`:字符串追加

```shell
#格式 append key value
append name cloud #在name后追加cloud
```

```shell
127.0.0.1:6379> get name
"illusory"
127.0.0.1:6379> append name cloud
(integer) 13
127.0.0.1:6379> get name
"illusorycloud"
```

`strlen`：获取字符串长度

```shell
#格式 strlen key 
strlen name #获取name对应的value的长度
```

```shell
127.0.0.1:6379> get name
"illusorycloud"
127.0.0.1:6379> strlen name
(integer) 13
```

### 2.Hash

工作中使用最多的就是Hash类型

将一个对象存储在Hash类型里要比String类型里占用的空间少一些，并方便存取整个对象。

`hset`:类似于set，数据都存为Hash类型，类似于存在map中

```shell
# 格式 hset key filed value
hset me name illusory #me是hash名 name是hash中的key illusory为hash中的value 

#类似于Java中的Map
        Map<Object,Object> me = new HashMap<>();
        me.put("name", "illusory");
```

`hget`:类似于get

```shell
# 格式 hget hash filed
hget me name #获取hash名为me的hash中的name对应的value
```

```shell
127.0.0.1:6379> hset me name illusory
(integer) 1
127.0.0.1:6379> hset me age 22
(integer) 1
127.0.0.1:6379> hget me name
"illusory"
127.0.0.1:6379> hget me age
"22"

```

同样也有批量操作的`hmset`、`hmget`

```shell
#格式 hmset key filed1 value1 filde2 value2 ....filedn valuen
#格式 hmget key filed1  filde2....filedn 
```

```shell
127.0.0.1:6379> hmset me name illusory age 22
OK
127.0.0.1:6379> hmget me name age
1) "illusory"
2) "22"

```

`hsetnx(not exist)`: 如果key不存在就设置值，返回1;存在就不设置，返回0；

```shell
#格式 hsetnx value filed value
```

`hincrby/hdecrby`:对一个值按照一定`步长`进行递增或者递减操作。

```shell
# 格式 hincrby key filed increment/hdecrby key filed increment
incrby me age 3 #age递增3
decrby me age 3 #age递减3
```

`hstrlen key filed`:回哈希表 `key` 中， 与给定域 `field` 相关联的值的字符串长度（string length）。

如果给定的键或者域不存在， 那么命令返回 `0` 。



`hexists`:判断是否存在

```shell
#格式 hexists value filed
```

`hlen`:查看hash的filed数

```shell
#格式 hlen key
```

`hdel`:删除指定hash中的filed

```shell
#格式 hdel key filed
```

`hkeys`:返回指定hash中所有的filed

```shell
#格式 hkeys key 
```

`hvals`:返回指定hash中所有的value

```shell
#格式 hvals key 
```

`hgetall`:返回指定hash中所有的filed和value

```shell
#格式 hgetall key
```

```shell
127.0.0.1:6379> hgetall me
1) "name"
2) "illusory"
3) "age"
4) "23"

```

### 3.List

可以看做Java中的List，不过更像Queue。

下表列出了列表相关的基本命令：

| 序号 | 命令及描述                                                   |
| ---- | ------------------------------------------------------------ |
| 1    | [BLPOP key1 [key2 \] timeout](http://www.runoob.com/redis/lists-blpop.html)  移出并获取列表的第一个元素， 如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止。 |
| 2    | [BRPOP key1 [key2 \] timeout](http://www.runoob.com/redis/lists-brpop.html)  移出并获取列表的最后一个元素， 如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止。 |
| 3    | [BRPOPLPUSH source destination timeout](http://www.runoob.com/redis/lists-brpoplpush.html)  从列表中弹出一个值，将弹出的元素插入到另外一个列表中并返回它； 如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止。 |
| 4    | [LINDEX key index](http://www.runoob.com/redis/lists-lindex.html)  通过索引获取列表中的元素 |
| 5    | [LINSERT key BEFORE\|AFTER pivot value](http://www.runoob.com/redis/lists-linsert.html)  在列表的元素前或者后插入元素 |
| 6    | [LLEN key](http://www.runoob.com/redis/lists-llen.html)  获取列表长度 |
| 7    | [LPOP key](http://www.runoob.com/redis/lists-lpop.html)  移出并获取列表的第一个元素 |
| 8    | [LPUSH key value1 [value2\]](http://www.runoob.com/redis/lists-lpush.html)  将一个或多个值插入到列表头部 |
| 9    | [LPUSHX key value](http://www.runoob.com/redis/lists-lpushx.html)  将一个值插入到已存在的列表头部 |
| 10   | [LRANGE key start stop](http://www.runoob.com/redis/lists-lrange.html)  获取列表指定范围内的元素 |
| 11   | [LREM key count value](http://www.runoob.com/redis/lists-lrem.html)  移除列表元素 |
| 12   | [LSET key index value](http://www.runoob.com/redis/lists-lset.html)  通过索引设置列表元素的值 |
| 13   | [LTRIM key start stop](http://www.runoob.com/redis/lists-ltrim.html)  对一个列表进行修剪(trim)，就是说，让列表只保留指定区间内的元素，不在指定区间之内的元素都将被删除。 |
| 14   | [RPOP key](http://www.runoob.com/redis/lists-rpop.html)  移除列表的最后一个元素，返回值为移除的元素。 |
| 15   | [RPOPLPUSH source destination](http://www.runoob.com/redis/lists-rpoplpush.html)  移除列表的最后一个元素，并将该元素添加到另一个列表并返回 |
| 16   | [RPUSH key value1 [value2\]](http://www.runoob.com/redis/lists-rpush.html)  在列表中添加一个或多个值 |
| 17   | [RPUSHX key value](http://www.runoob.com/redis/lists-rpushx.html)  为已存在的列表添加值 |

### 4.Set

`Set`集合是String类型的`无序`集合，通过hashtable实现的，对集合我们可以取交集，并集，差集。

Java中List的升级版。

下表列出了 Redis 集合基本命令：

| 序号 | 命令及描述                                                   |
| ---- | ------------------------------------------------------------ |
| 1    | [SADD key member1 [member2\]](http://www.runoob.com/redis/sets-sadd.html)  向集合添加一个或多个成员 |
| 2    | [SCARD key](http://www.runoob.com/redis/sets-scard.html)  获取集合的成员数 |
| 3    | [SDIFF key1 [key2\]](http://www.runoob.com/redis/sets-sdiff.html)  返回给定所有集合的差集 |
| 4    | [SDIFFSTORE destination key1 [key2\]](http://www.runoob.com/redis/sets-sdiffstore.html)  返回给定所有集合的差集并存储在 destination 中 |
| 5    | [SINTER key1 [key2\]](http://www.runoob.com/redis/sets-sinter.html)  返回给定所有集合的交集 |
| 6    | [SINTERSTORE destination key1 [key2\]](http://www.runoob.com/redis/sets-sinterstore.html)  返回给定所有集合的交集并存储在 destination 中 |
| 7    | [SISMEMBER key member](http://www.runoob.com/redis/sets-sismember.html)  判断 member 元素是否是集合 key 的成员 |
| 8    | [SMEMBERS key](http://www.runoob.com/redis/sets-smembers.html)  返回集合中的所有成员 |
| 9    | [SMOVE source destination member](http://www.runoob.com/redis/sets-smove.html)  将 member 元素从 source 集合移动到 destination 集合 |
| 10   | [SPOP key](http://www.runoob.com/redis/sets-spop.html)  移除并返回集合中的一个随机元素 |
| 11   | [SRANDMEMBER key [count\]](http://www.runoob.com/redis/sets-srandmember.html)  返回集合中一个或多个随机数 |
| 12   | [SREM key member1 [member2\]](http://www.runoob.com/redis/sets-srem.html)  移除集合中一个或多个成员 |
| 13   | [SUNION key1 [key2\]](http://www.runoob.com/redis/sets-sunion.html)  返回所有给定集合的并集 |
| 14   | [SUNIONSTORE destination key1 [key2\]](http://www.runoob.com/redis/sets-sunionstore.html)  所有给定集合的并集存储在 destination 集合中 |
| 15   | [SSCAN key cursor [MATCH pattern\] [COUNT count]](http://www.runoob.com/redis/sets-sscan.html)  迭代集合中的元素 |

### 5. ZSet

`ZSet`则是`有序`的。

下表列出了 redis 有序集合的基本命令:

| 序号 | 命令及描述                                                   |
| ---- | ------------------------------------------------------------ |
| 1    | [ZADD key score1 member1 [score2 member2\]](http://www.runoob.com/redis/sorted-sets-zadd.html)  向有序集合添加一个或多个成员，或者更新已存在成员的分数 |
| 2    | [ZCARD key](http://www.runoob.com/redis/sorted-sets-zcard.html)  获取有序集合的成员数 |
| 3    | [ZCOUNT key min max](http://www.runoob.com/redis/sorted-sets-zcount.html)  计算在有序集合中指定区间分数的成员数 |
| 4    | [ZINCRBY key increment member](http://www.runoob.com/redis/sorted-sets-zincrby.html)  有序集合中对指定成员的分数加上增量 increment |
| 5    | [ZINTERSTORE destination numkeys key [key ...\]](http://www.runoob.com/redis/sorted-sets-zinterstore.html)  计算给定的一个或多个有序集的交集并将结果集存储在新的有序集合 key 中 |
| 6    | [ZLEXCOUNT key min max](http://www.runoob.com/redis/sorted-sets-zlexcount.html)  在有序集合中计算指定字典区间内成员数量 |
| 7    | [ZRANGE key start stop [WITHSCORES\]](http://www.runoob.com/redis/sorted-sets-zrange.html)  通过索引区间返回有序集合成指定区间内的成员 |
| 8    | [ZRANGEBYLEX key min max [LIMIT offset count\]](http://www.runoob.com/redis/sorted-sets-zrangebylex.html)  通过字典区间返回有序集合的成员 |
| 9    | [ZRANGEBYSCORE key min max [WITHSCORES\] [LIMIT]](http://www.runoob.com/redis/sorted-sets-zrangebyscore.html)  通过分数返回有序集合指定区间内的成员 |
| 10   | [ZRANK key member](http://www.runoob.com/redis/sorted-sets-zrank.html)  返回有序集合中指定成员的索引 |
| 11   | [ZREM key member [member ...\]](http://www.runoob.com/redis/sorted-sets-zrem.html)  移除有序集合中的一个或多个成员 |
| 12   | [ZREMRANGEBYLEX key min max](http://www.runoob.com/redis/sorted-sets-zremrangebylex.html)  移除有序集合中给定的字典区间的所有成员 |
| 13   | [ZREMRANGEBYRANK key start stop](http://www.runoob.com/redis/sorted-sets-zremrangebyrank.html)  移除有序集合中给定的排名区间的所有成员 |
| 14   | [ZREMRANGEBYSCORE key min max](http://www.runoob.com/redis/sorted-sets-zremrangebyscore.html)  移除有序集合中给定的分数区间的所有成员 |
| 15   | [ZREVRANGE key start stop [WITHSCORES\]](http://www.runoob.com/redis/sorted-sets-zrevrange.html)  返回有序集中指定区间内的成员，通过索引，分数从高到底 |
| 16   | [ZREVRANGEBYSCORE key max min [WITHSCORES\]](http://www.runoob.com/redis/sorted-sets-zrevrangebyscore.html)  返回有序集中指定分数区间内的成员，分数从高到低排序 |
| 17   | [ZREVRANK key member](http://www.runoob.com/redis/sorted-sets-zrevrank.html)  返回有序集合中指定成员的排名，有序集成员按分数值递减(从大到小)排序 |
| 18   | [ZSCORE key member](http://www.runoob.com/redis/sorted-sets-zscore.html)  返回有序集中，成员的分数值 |
| 19   | [ZUNIONSTORE destination numkeys key [key ...\]](http://www.runoob.com/redis/sorted-sets-zunionstore.html)  计算给定的一个或多个有序集的并集，并存储在新的 key 中 |
| 20   | [ZSCAN key cursor [MATCH pattern\] [COUNT count]](http://www.runoob.com/redis/sorted-sets-zscan.html)  迭代有序集合中的元素（包括元素成员和元素分值） |

## 5.Redis高级命令

* 返回满足的所有键keys* (可以模糊匹配） 
* exists是否存在指定的key
* expire设置某个key的过期时间，使用ttl查看剰余时间 
* persist取消过期时间
* select选择数据库数据库为0到15 (—共16个数据库）默认进入的是0数据库 
* move [key][数据库下标]将当前数据中的key转移到其他数据库中 
* randomkey随机返回数据库里的一个key 
* rename重命名key
* echo打印命令 
* dbsize查看数据库的key数量 
* info 获取数据库信息 
* conflg get 实时传储收到的请求(返回相关的配置信息} 
* config get *返回所有配置 
* flushdb 清空当前数据库，
* flushall 清空所有数据库 

Redis分片原因：

* 最初是为了数据安全，分片用于做备份，现在Redis有集群了，这个基本不用了。
* 快速查询，把不同数据存在不同的片中，查询时快速定位
* 只是逻辑划分，不是物理划分。所有分片共用一个空间。

## 6. 安全性

可以为Redis设置密码。
修改配置文件

```shell
[root@localhost etc]# vim redis.conf 

# Warning: since Redis is pretty fast an outside user can try up to
# 150k passwords per second against a good box. This means that you should
# use a very strong password otherwise it will be very easy to break.
#
# requirepass foobared
#设置密码 这里的redis就是密码
requirepass redis

```

重启Redis，并进入客户端，执行查询keys * 提示无权限

```shell
[root@localhost etc]# /usr/local/redis/bin/redis-server /usr/local/redis/etc/redis.conf
[root@localhost etc]# /usr/local/redis/bin/redis-cli 
127.0.0.1:6379> keys *
(error) NOAUTH Authentication required.

```

输入密码后即可正常使用。

`auth password`

```shell
127.0.0.1:6379> auth redis
OK
127.0.0.1:6379> keys *
1) "zset1"
2) "set1"
3) "list1"

```

也可以在进入客户端时输入密码

```shell
/usr/local/redis/bin/redis-cli -a password
```

不过一般都不设置密码，工作中都是只能内网访问。

## 7.Redis主从复制

### 7.1 主从复制简介

* 1.Master可以拥有多个slave

* 2.多个Slavic可以连接同一个master外，还可以连接到其他的slave 
* 3.从复制不会阻塞master在同步数据时master可以继续处理client请求 
* 4.提供系统的伸缩性 

### 7.2 主从复制过程

* 1.slave与master建立连接，发送sync同步命令
* 2.master开启一个后台进程，将数据库快照保存到文件中，同时master主进程会开始收集新的写命令并缓存
* 3.后台完成保存后，就将文件发送给slave
* 4.slave将此文件保存到硬盘上 

### 7.3 主从复制配置

**主服务器不需要任何调整，只需要对从服务器进行配置**。



修改从服务器Redis配置文件：`/usr/local/redis/etc/redis.conf `

```shell
#添加如下配置
#slaveof master服务器IP master服务器端口号
#slaveof <masterip> <masterport>
slaveof 192.168.1.111 6379
```

如果master服务器上的Redis配置了密码，那么还需要配置以下密码

```shell
#masterauth <master-password>
masterauth redis
```

最后使用info查看role角色即可知道是主服务或从服务。

主服务器可读可写，从服务器只能读不能写。

### 7.4 问题

如果从服务器中查看info出现

```shell
master_link_status:down
```

即主从连接失败，这时在Redis.conf配置文件中做如下修改：

```shell
#bindip 表示具有访问权限
#bind 127.0.0.1 即localhost才能访问 修改为0.0.0.0 即都可以访问
bind 0.0.0.0 
```

修改后从重启一下，应该就oK了。

当然可能还要防火墙的问题，需要关掉防火墙

```shell
systemctl stop firewalld # 临时关闭防火墙
systemctl disable firewalld # 禁止开机启动
```

或者主从服务器根本ping不通，这....

## 8. Redis事务

Redis事务非常简单，使用方法如下：

首先使用`multi`打开事务，然后就写数据了，现在设置的数据都会放在队列里进行保存，最后使用`exec`执行，把数据依次存储到Redis中，`discard`取消事务。

`watch` : 监视一个或多个key，如果事务执行前key被修改了，那么事务将被打断。

```shell
127.0.0.1:6379> multi
OK
127.0.0.1:6379> set a 1
QUEUED
127.0.0.1:6379> set b 2
QUEUED
127.0.0.1:6379> set c 3
QUEUED
127.0.0.1:6379> incr a
QUEUED
127.0.0.1:6379> set d a
QUEUED
127.0.0.1:6379> incr d
QUEUED
127.0.0.1:6379> exec
1) OK
2) OK
3) OK
4) (integer) 2
5) OK
6) (error) ERR value is not an integer or out of range

```

不过事务中出错后并不会回滚，前面的还是已经执行了...

## 9. Redis持久化

### 9.1 简介

Redis是一个支持持久化的内存数据库，可以将内存中的数据写入到磁盘里。

Redis有两种持久化机制：

* **RDB**持久化（原理是将Reids在内存中的数据以快照的方式写入到二进制文件dump.rdb中,所以叫RDB），默认开启RDB机制。

* **AOF(append-only file)**机制对每条写入命令作为日志，以append-only的模式写入一个AOF日志文件中，在redis重启的时候，可以通过回放AOF日志中的写入指令来重新构建整个数据集。 



### 9.2 对比

RDB方式是定时保存一次，若突然掉电，很可能会丢失数据。

AOF方式可以配置每次操作都写入日志文件中，



RDB持久化是指在指定的时间间隔内将内存中的数据集快照写入磁盘，实际操作过程是fork一个子进程，先将数据集写入临时文件，写入成功后，再替换之前的文件，用二进制压缩存储。 

AOF持久化以日志的形式记录服务器所处理的每一个写、删除操作，查询操作不会记录，以文本的方式记录，可以打开文件看到详细的操作记录。 

RDB存在哪些优势呢？

1). 一旦采用该方式，那么你的整个Redis数据库将只包含一个文件，这对于文件备份而言是非常完美的。

2). 对于灾难恢复而言，RDB是非常不错的选择。因为我们可以非常轻松的将一个单独的文件压缩后再转移到其它存储介质上。

3). **性能最大化**。对于Redis的服务进程而言，在开始持久化时，它唯一需要做的只是fork出子进程，之后再由子进程完成这些持久化的工作，这样就可以极大的避免服务进程执行IO操作了。

4). 相比于AOF机制，如果数据集很大，RDB的**启动效率会更高**。

RDB又存在哪些劣势呢？

1). 如果你想保证数据的高可用性，即最大限度的避免数据丢失，那么RDB将不是一个很好的选择。因为系统一旦在定时持久化之前出现宕机现象，此前没有来得及写入磁盘的数据都将丢失。

2). 由于RDB是通过fork子进程来协助完成数据持久化工作的，因此，如果当数据集较大时，可能会导致整个服务器停止服务几百毫秒，甚至是1秒钟。

AOF的优势有哪些呢？

1). 该机制可以带来**更高的数据安全性**，即数据持久性。Redis中提供了3中同步策略，即每秒同步、每修改同步和不同步。事实上，每秒同步也是异步完成的，其 效率也是非常高的，所差的是一旦系统出现宕机现象，那么这一秒钟之内修改的数据将会丢失。而每修改同步，我们可以将其视为同步持久化，即每次发生的数据变 化都会被立即记录到磁盘中。可以预见，这种方式在效率上是最低的。至于无同步，无需多言，我想大家都能正确的理解它。

2). 由于该机制对日志文件的写入操作采用的是append模式，因此在写入过程中即使出现宕机现象，也不会破坏日志文件中已经存在的内容。然而如果我们本次操 作只是写入了一半数据就出现了系统崩溃问题，不用担心，在Redis下一次启动之前，我们可以通过redis-check-aof工具来帮助我们解决数据 一致性的问题。

3). 如果日志过大，Redis可以自动启用rewrite机制。即Redis以append模式不断的将修改数据写入到老的磁盘文件中，同时Redis还会创 建一个新的文件用于记录此期间有哪些修改命令被执行。因此在进行rewrite切换时可以更好的保证数据安全性。

4). AOF包含一个格式清晰、易于理解的日志文件用于记录所有的修改操作。事实上，我们也可以通过该文件完成数据的重建。

AOF的劣势有哪些呢？

1). **大数据时恢复速度慢**，对于相同数量的数据集而言，AOF文件通常要大于RDB文件。

2). 根据同步策略的不同，**AOF在运行效率上往往会慢于RDB**。总之，每秒同步策略的效率是比较高的，同步禁用策略的效率和RDB一样高效。

二者选择的标准，就是看系统是愿意**牺牲一些性能，换取更高的缓存一致性（aof）**，还是愿意写操作频繁的时候，不启用备份来换取更高的性能，待手动运行save的时候，再做备份（rdb）。rdb这个就更有些 eventually consistent的意思了。

### 9.3 配置

RDB方式是默认开启的不用配置，如果配置开启了AOF那么RDB会关闭。

如何开启AOF？

修改Redis.conf配置文件

```shell
#appendonly no 是否开启aof 改成yes即可
appendonly yes
# The name of the append only file (default: "appendonly.aof")
#设置保存的日志文件名 一般用默认的就好了
appendfilename "appendonly.aof"
```

## 10. 发布与订阅

Redis提供了简单的发布订阅功能，具体如下：

```shell
# 订阅监听
subscribe [频道]   
# 进行发布消息广播
publish [频道][发布内容]
```

```shell
# 订阅频道cctv
127.0.0.1:6379> subscribe cctv
Reading messages... (press Ctrl-C to quit)
1) "subscribe"
2) "cctv"
3) (integer) 1
# 发布广播
127.0.0.1:6379> publish cctv hello
(integer) 0

# 订阅窗口就可以接受到消息了
1) "message"
2) "cctv"
3) "hello"
```

## 11.javaapi操作

需要引入Jedis包，其实和直接在shell中操作是一样的。

### 11.1 基本使用

```java
/**
 * @author illusoryCloud
 */
public class JedisTest {
    @Test
    public void jedisTestOne() {
        Jedis jedis = new Jedis("192.168.1.111", 6379);

        //-------------string----------------
        String set = jedis.set("name", "illusorycloud");
        //ok
        System.out.println(set);
        String name = jedis.get("name");
        //illusorycloud
        System.out.println(name);
        //-------------hash----------------
        Map<String, String> map = new HashMap<>();
        map.put("name", "illusory");
        map.put("age", "30");
        map.put("address", "cq");
        jedis.hmset("user", map);

        JedisPool pool = jedisPoolTest();
        //从连接池中获取一个Jedis实例
        Jedis j = pool.getResource();
        j.set("test", "111");
    }

    /**
     * 方法描述 构建redis连接池
     * 池子中存放着多个jedis实例
     */
    @Test
    public JedisPool jedisPoolTest() {

        JedisPool pool = null;
        if (pool == null) {
            JedisPoolConfig config = new JedisPoolConfig();
            //控制一个pool可分配多少个jedis实例，通过pool.getResource()来获取；
            //如果赋值为-1，则表示不限制；如果pool已经分配了maxActive个jedis实例，则此时pool的状态为exhausted(耗尽)。
            config.setMaxTotal(50);
            //控制一个pool最多有多少个状态为idle(空闲的)的jedis实例。
            config.setMaxIdle(5);
            //表示当borrow(引入)一个jedis实例时，最大的等待时间，如果超过等待时间，则直接抛出JedisConnectionException；单位毫秒
            //小于零:阻塞不确定的时间,  默认-1
            config.setMaxWaitMillis(1000 * 100);
            //在borrow(引入)一个jedis实例时，是否提前进行validate操作；如果为true，则得到的jedis实例均是可用的；
            config.setTestOnBorrow(true);
            //return 一个jedis实例给pool时，是否检查连接可用性（ping()）
            config.setTestOnReturn(true);
            //connectionTimeout 连接超时（默认2000ms）
            //soTimeout 响应超时（默认2000ms）
            pool = new JedisPool(config, "127.0.0.1", 6379, 2000, "619868");
        }
        return pool;
    }
}

```

### 11.2 组合使用

```java

//指定业务 查询业务 SYS_USER_SEL_AGE_25
    //指定业务 查询业务 SYS_USER_SEL_SEX_MAN
    //指定业务 查询业务 SYS_USER_SEL_SEX_WOMEN
    private final String SYS_USER_TABLE = "SYS_USER_TABLE";
    private final String SYS_USER_SEL_AGE_25 = "SYS_USER_SEL_AGE_25";
    private final String SYS_USER_SEL_SEX_MAN = "SYS_USER_SEL_SEX_MAN";
    private final String SYS_USER_SEL_SEX_WOMEN = "SYS_USER_SEL_SEX_WOMEN";
   /**
     * User对象 数据量很大且查询频繁 需要把user表中的数据都放入缓存
     */
    @Test
    public void jedisTestTwo() {
        Jedis jedis = new Jedis("192.168.5.154", 6379);

        //1.做放入操作
        Map<String, String> map = new HashMap<>();
        String uid1 = "illusory" + UUID.randomUUID().toString();
        User u1 = new User(uid1, "illusoryCloud", 22, "man");
        map.put(uid1, JSON.toJSONString(u1));

        String uid2 = "illusory" + UUID.randomUUID().toString();
        User u2 = new User(uid2, "Java", 23, "women");
        map.put(uid2, JSON.toJSONString(u2));
        String uid3 = "illusory" + UUID.randomUUID().toString();
        User u3 = new User(uid3, "Android", 24, "man");
        map.put(uid3, JSON.toJSONString(u3));

        String uid4 = "illusory" + UUID.randomUUID().toString();
        User u4 = new User(uid4, "iOS", 25, "women");
        map.put(uid4, JSON.toJSONString(u4));

        String uid5 = "illusory" + UUID.randomUUID().toString();
        User u5 = new User(uid5, "Python", 26, "man");
        map.put(uid5, JSON.toJSONString(u5));
        jedis.hmset("SYS_USER_TABLE", map);
        //假如这里放入了1000W条数据
        //如何按条件查询
        //select * from user where set='women'
        //select * from user where set='women' and age=25
        //很明显是做不到的
        //一般持久化时都是多种数据类型配合使用 hash+set
        //详情见jedsiTestThree();

    }
```
直接存进入后无法按条件查询，所以存时需要好好考虑。

```java
   @Test
    public void jedsiTestThree() {
        Jedis jedis = new Jedis("192.168.5.154", 6379);
        //写入数据时往多个集合中写
        //1.做放入操作
        Map<String, String> map = new HashMap<>();
        String uid1 = "illusory" + UUID.randomUUID().toString();
        User u1 = new User(uid1, "illusoryCloud", 22, "man");
        //这里满足多种条件时 每个集合都存一次
        map.put(uid1, JSON.toJSONString(u1));
        jedis.sadd(SYS_USER_SEL_SEX_MAN, uid1);

        String uid2 = "illusory" + UUID.randomUUID().toString();
        User u2 = new User(uid2, "Java", 23, "women");
        map.put(uid2, JSON.toJSONString(u2));
        jedis.sadd(SYS_USER_SEL_SEX_WOMEN, uid2);

        String uid3 = "illusory" + UUID.randomUUID().toString();
        User u3 = new User(uid3, "Android", 24, "man");
        map.put(uid3, JSON.toJSONString(u3));
        jedis.sadd(SYS_USER_SEL_SEX_MAN, uid3);

        String uid4 = "illusory" + UUID.randomUUID().toString();
        User u4 = new User(uid4, "iOS", 25, "women");
        map.put(uid4, JSON.toJSONString(u4));
        jedis.sadd(SYS_USER_SEL_SEX_WOMEN, uid4);
        jedis.sadd(SYS_USER_SEL_AGE_25, uid4);

        String uid5 = "illusory" + UUID.randomUUID().toString();
        User u5 = new User(uid5, "Python", 26, "man");
        map.put(uid5, JSON.toJSONString(u5));
        jedis.hmset(SYS_USER_TABLE, map);
        jedis.sadd(SYS_USER_SEL_SEX_MAN, uid5);
        //select * from user where set='women'
        //select * from user where set='women' and age=25
    }
```
查询
```java
 @Test
    public void select() {
        Jedis jedis = new Jedis("192.168.5.154", 6379);
        //select * from user where set='women'
        System.out.println("------------select * from user where set='women'--------");

        //查出所有women的id
        Set<String> userWomenId = jedis.smembers(SYS_USER_SEL_SEX_WOMEN);
        //再通过id查询user
        for (Iterator iterator = userWomenId.iterator(); iterator.hasNext(); ) {
            String next = (String) iterator.next();
            String userString = jedis.hget(SYS_USER_TABLE, next);
            //string-->json-->user
            JSON userJson = (JSON) JSONObject.parse(userString);
            User user = JSON.toJavaObject(userJson, User.class);
            System.out.println("userName: "+user.getName());
            System.out.println(userJson);
        }
        System.out.println("------------select * from user where set='women' and age=25--------");
        //select * from user where set='women' and age=25
        Set<String> sinter = jedis.sinter(SYS_USER_SEL_AGE_25, SYS_USER_SEL_SEX_WOMEN);
        for (Iterator iterator = sinter.iterator(); iterator.hasNext(); ) {
            String next = (String) iterator.next();
            String userJson = jedis.hget(SYS_USER_TABLE, next);
            System.out.println(userJson);
        }
    }
```
结果
```java
------------select * from user where set='women'--------
userName: iOS
{"sex":"women","name":"iOS","id":"illusory8bf000a3-3ed9-4f80-8d25-95729d3b8447","age":25}
userName: Java
{"sex":"women","name":"Java","id":"illusoryc42cbc91-9ed2-4468-b1a3-8ce05ff86766","age":23}
------------select * from user where set='women' and age=25--------
{"age":25,"id":"illusory8bf000a3-3ed9-4f80-8d25-95729d3b8447","name":"iOS","sex":"women"}
```

## 12. Redis集群
> 最少要3台机器才能形成集群，同时每个主需要配一个从，即最好6台机器。不过这里也没这么多机器
> 小霸王电脑也开不了6个虚拟机 所以就在一台机器上开6个Redis做个伪集群，真实情况下也差不多是这样配置的
### 12.1 创建文件夹
先创建一个文件夹`redis-cluster`，然后在这个文件夹里分别创建6个文件夹当成6台机器，`redis7001`、`redis7002`、
`redis7003`、`redis7004`、`redis7005`、`redis7006`、

```shelll
[root@localhost etc]# mkdir -p /usr/local/redis-cluster
[root@localhost etc]# cd /usr/local/redis-cluster/
[root@localhost redis-cluster]# mkdir 7001 && mkdir 7002 && mkdir 7003 && mkdir 7004 && mkdir 7005 && mkdir 7006 

```

### 12.2 修改配置文件
然后把前面的`redis.conf`配置文件分别复制到这6个文件夹
```shell
# 先复制一份 等修改好后再复制到其他地方
[root@localhost redis-5.0.3]# cp redis.conf /usr/local/redis-cluster/7001

```
然后修改配置文件Redis.conf
```vim
1.daemonize yes 后台启动 改为 
2.port 7001 改为对应的700*
3.bind 192.168.5.191 改为当前机器的IP
4.dir /usr/local/redis-cluster/7001 数据存储目录 每台机器必须指向不同的位置对应的700*
5.cluster-enabled yes 开启集群模式
6.cluster-config-file nodes-7001.conf 这里最好和port对应700*
7.cluster-node-timeout 5000 超时时间可以自己调整
8.appendonly yes 开启AOF
```
修改完成后再分别负责到另外5台机器。**复制后记得把需要修改的地方修改一下，port,dir,cluster-config-file**

### 12.3 环境准备
到这里6个文件夹的Redis.conf配置文件都修改好了，
由于Redis集群需要Ruby命令，所以需要安装Ruby。
```shell
[root@localhost 7006]# yum install ruby
[root@localhost 7006]# yum install rubygems
#redis和Ruby的接口
[root@localhost 7006]# gem install redis    

```
#### 问题
可能最后一步出现问题
```java
redis requires Ruby version >= 2.2.2
```
CentOS 默认支持ruby是2.0版本导致的
#### 解决

通过rvm安装最新的ruby即可，具体如下;
```shell
# 安装curl curl是Linux下的文件传输工具
[root@localhost 7006]# yum install curl
# 安装rvm RVM是一个命令行工具，可以提供一个便捷的多版本Ruby环境的管理和切换。
[root@localhost 7006]# gpg2 --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
[root@localhost 7006]# \curl -sSL https://get.rvm.io | bash -s stable
# 查看一下rvm安装上没有
[root@localhost 7006]# find / -name rvm -print
#如果出现下面这样的就说明安装上了
　　　　 /usr/local/rvm
　　 　　/usr/local/rvm/src/rvm
　　 　　/usr/local/rvm/src/rvm/bin/rvm
　　 　　/usr/local/rvm/src/rvm/lib/rvm
　　 　　/usr/local/rvm/src/rvm/scripts/rvm
　　 　　/usr/local/rvm/bin/rvm
　　 　　/usr/local/rvm/lib/rvm
　　 　　/usr/local/rvm/scripts/rvm
# 使rvm配置文件生效　 　　
[root@localhost 7006]# source /usr/local/rvm/scripts/rvm
# 查看rvm库中已知的ruby版本
[root@localhost 7006]# rvm list known
# 大概会向这样 当前最新的时2.6
# MRI Rubies
[ruby-]1.8.6[-p420]
[ruby-]1.8.7[-head] # security released on head
[ruby-]1.9.1[-p431]
[ruby-]1.9.2[-p330]
[ruby-]1.9.3[-p551]
[ruby-]2.0.0[-p648]
[ruby-]2.1[.10]
[ruby-]2.2[.10]
[ruby-]2.3[.8]
[ruby-]2.4[.5]
[ruby-]2.5[.3]
[ruby-]2.6[.0]
ruby-head

#安装ruby2.6.0  这里需要等一会 下载之后还要等编译完...
[root@localhost 7006]# rvm install 2.6.0
#使用ruby2.6.0
[root@localhost 7006]# rvm use 2.6.0
#设置ruby默认版本
[root@localhost 7006]# rvm use 2.6.0 --default
#查看ruby版本 
[root@localhost 7006]# ruby --version
# 继续安装Redis
[root@localhost 7006]# gem install redis   

```
### 12.4 启动

分别启动6个Redis


```shell
# 分别启动6个Redis
[root@localhost 7006]# /usr/local/redis/bin/redis-server /usr/local/redis-cluster/7001/redis.conf
[root@localhost 7006]# /usr/local/redis/bin/redis-server /usr/local/redis-cluster/7002/redis.conf
[root@localhost 7006]# /usr/local/redis/bin/redis-server /usr/local/redis-cluster/7003/redis.conf
[root@localhost 7006]# /usr/local/redis/bin/redis-server /usr/local/redis-cluster/7004/redis.conf
[root@localhost 7006]# /usr/local/redis/bin/redis-server /usr/local/redis-cluster/7005/redis.conf
[root@localhost 7006]# /usr/local/redis/bin/redis-server /usr/local/redis-cluster/7006/redis.conf
#查看一下启动成功没
[root@localhost redis-cluster]# ps aux|grep redis
root      16205  0.1  0.2 159512  2604 ?        Ssl  14:08   0:16 /usr/local/redis/bin/redis-server 0.0.0.0:6379
root      35225  0.2  0.2 153880  2836 ?        Ssl  16:22   0:00 /usr/local/redis/bin/redis-server 192.168.5.154:7001 [cluster]
root      35388  0.2  0.2 153880  2836 ?        Ssl  16:24   0:00 /usr/local/redis/bin/redis-server 192.168.5.154:7002 [cluster]
root      35398  0.1  0.2 153880  2832 ?        Ssl  16:24   0:00 /usr/local/redis/bin/redis-server 192.168.5.154:7003 [cluster]
root      35407  0.1  0.2 153880  2836 ?        Ssl  16:25   0:00 /usr/local/redis/bin/redis-server 192.168.5.154:7004 [cluster]
root      35415  0.1  0.2 153880  2836 ?        Ssl  16:25   0:00 /usr/local/redis/bin/redis-server 192.168.5.154:7005 [cluster]
root      35424  0.1  0.2 153880  2832 ?        Ssl  16:25   0:00 /usr/local/redis/bin/redis-server 192.168.5.154:7006 [cluster]
root      35468  0.0  0.0 112708   976 pts/1    R+   16:25   0:00 grep --color=auto redis
# ok的
```
### 12.5 创建集群
#### 5.0之前的方式
进入redis安装目录`/usr/local/redis-5.0.3/src`找到`redis-trib.rb`脚本,这就是redis集群相关操作的脚本，是ruby写的.
执行命令创建集群
```shell
# 解释： 
# ./redis-trib.rb 即redis集群操作脚本 
# create 即创建集群
# --replicas  即配置 后面一长串都是集群配置
# 1  这个1是redis主从比例 主机三台 从机三台 主/--> 3/3 即 1
# 后面的6台机器中 会根据配置的比例把前面的几台(这里是3台)做为主，后面的做为从
# 然后主节点中的第一个对应的一定是从节点中的第一个 依次排下去这里就是7001 对应7004
[root@localhost src]# ./redis-trib.rb create --replicas 1 192.168.5.154:7001 192.168.5.154:7002 
 192.168.5.154:7003 192.168.5.154:7004 192.168.5.154:7005 192.168.5.154:7006
 
```

#### 5.0之后的方式
Redis 5.0 版本，放弃了Ruby的集群方式，改为使用C语言编写的 redis-cli的方式，使集群的构建方式复杂度大大降低。
命令如下：
```shell
# 参数的含义和上面也是一样的
#create 即创建集群
# --replicas  即配置 后面一长串都是集群配置
# 1  这个1是redis主从比例 主机三台 从机三台 主/--> 3/3 即 1
# 后面的6台机器中 会根据配置的比例把前面的几台(这里是3台)做为主，后面的做为从
# 然后主节点中的第一个对应的一定是从节点中的第一个 依次排下去这里就是7001 对应7004

[root@localhost src]# redis-cli --cluster create 192.168.5.154:7001 192.168.5.154:7002 
192.168.5.154:7003 192.168.5.154:7004 192.168.5.154:7005 192.168.5.154:7006 --cluster-replicas 1
# 接下来会询问是否设置 输入yes即可
Can I set the above configuration? (type 'yes' to accept): yes
>>> Performing Cluster Check (using node 192.168.5.154:7001)
M: 092694ff8d9a2fef0df632af1650653b6756efd5 192.168.5.154:7001
   slots:[0-5460] (5461 slots) master
   1 additional replica(s)
S: 1c465b386313dbe47fbdea93775a9ef249b8e5ae 192.168.5.154:7004
   slots: (0 slots) slave
   replicates 092694ff8d9a2fef0df632af1650653b6756efd5
M: 1c3f76d0f78d378f6413a66b7df0092b3f5be7ea 192.168.5.154:7002
   slots:[5461-10922] (5462 slots) master
   1 additional replica(s)
S: 80694351fa024c34316ffc443a3e25a5e99132cf 192.168.5.154:7005
   slots: (0 slots) slave
   replicates 1c3f76d0f78d378f6413a66b7df0092b3f5be7ea
S: 35eeed488e23f852520df69cdac3dfcb0001c7cf 192.168.5.154:7006
   slots: (0 slots) slave
   replicates 74fde96cb2cd794f0c15d26366641ce9afc34946
M: 74fde96cb2cd794f0c15d26366641ce9afc34946 192.168.5.154:7003
   slots:[10923-16383] (5461 slots) master

# 到这里集群就搭建好了 其中 slots:[0-5460] (5461 slots) 表示槽 
# 可以发现只有master有 slave没有 因为slave是不支持写的 只能读
```
### 12.6 测试
到此为止，集群已经搭建成功，进行验证。
连接随便一个客户端
```shell
# -c 表示集群模式 -h 主机名 -p 端口号
[root@localhost bin]# /usr/local/redis/bin/reids-cli -c -h 192.168.5.154 -p 7001
```
检测
`cluster nodes`查看集群信息
```shell
[root@localhost bin]# /usr/local/redis/bin/redis-cli -c -h 192.168.5.154 -p 7001
192.168.5.154:7001> cluster nodes
1c465b386313dbe47fbdea93775a9ef249b8e5ae 192.168.5.154:7004@17004 slave 092694ff8d9a2fef0df632af1650653b6756efd5 0 1551948999244 4 connected
092694ff8d9a2fef0df632af1650653b6756efd5 192.168.5.154:7001@17001 myself,master - 0 1551948997000 1 connected 0-5460
1c3f76d0f78d378f6413a66b7df0092b3f5be7ea 192.168.5.154:7002@17002 master - 0 1551948997000 2 connected 5461-10922
80694351fa024c34316ffc443a3e25a5e99132cf 192.168.5.154:7005@17005 slave 1c3f76d0f78d378f6413a66b7df0092b3f5be7ea 0 1551948998236 5 connected
35eeed488e23f852520df69cdac3dfcb0001c7cf 192.168.5.154:7006@17006 slave 74fde96cb2cd794f0c15d26366641ce9afc34946 0 1551948997226 6 connected
74fde96cb2cd794f0c15d26366641ce9afc34946 192.168.5.154:7003@17003 master - 0 1551948999000 3 connected 10923-16383

```

添加数据测试一下
```shell
# 集群模式下 数据会随机或者平均分到几个主机中 这里在7001存的数据进入了第5798个槽中 即7002主机
192.168.5.154:7001> set name illusory
-> Redirected to slot [5798] located at 192.168.5.154:7002
OK
```
然后7002查询，在添加前查询时没有数据的 等7001存数据后 再次查询就能查到了
```shell
[root@localhost ~]# /usr/local/redis/bin/redis-cli -c -h 192.168.5.154 -p 7002
192.168.5.154:7002> keys *
(empty list or set)
192.168.5.154:7002> keys *
1) "name"
192.168.5.154:7002> 
```
注：集群模式只需要配置一次，以后启动后就自动为集群模式了。

###12.7 集群常用命令
redis5提供了一些操作集群的工具，在redis安装目录下
`/usr/local/redis-5.0.3/utils/create-cluster/create-cluster` 是个shell脚本文件。
使用前需要修改脚本中的一些信息
```shell
[root@localhost create-cluster]# vim create-cluster
#!/bin/bash

# Settings
#这个端口改为比自己的第一个节点小1
# 会默认自增1形成7001~7006 六个节点
PORT=7000
# 超时时间
TIMEOUT=2000
# 节点数 也需要改为相对应的
NODES=6
# 主从比例也是
REPLICAS=1


```

* 启动集群：` /root/local/redis-5.0.3/utils/create-cluster/create-cluster start`

* 关闭集群：` /root/local/redis-5.0.3/utils/create-cluster/create-cluster stop`

附上一个启动集群的脚本
```shell
#!/bin/sh
/usr/local/redis/bin/redis-server  /usr/local/redis-cluster/7001/redis.conf
/usr/local/redis/bin/redis-server  /usr/local/redis-cluster/7002/redis.conf
/usr/local/redis/bin/redis-server  /usr/local/redis-cluster/7003/redis.conf
/usr/local/redis/bin/redis-server  /usr/local/redis-cluster/7004/redis.conf
/usr/local/redis/bin/redis-server  /usr/local/redis-cluster/7005/redis.conf
/usr/local/redis/bin/redis-server  /usr/local/redis-cluster/7006/redis.conf

/usr/local/redis/bin/redis-cli --cluster create 192.168.5.154:7001 192.168.5.154:7002 
192.168.5.154:7003 192.168.5.154:7004 192.168.5.154:7005 192.168.5.154:7006 --cluster-replicas 1
```
更多集群相关命令
`/usr/local/redis/bin/redis-cli --cluster help`
```xml
Cluster Manager Commands:
  create         host1:port1 ... hostN:portN
                 --cluster-replicas <arg>
  check          host:port
                 --cluster-search-multiple-owners
  info           host:port
  fix            host:port
                 --cluster-search-multiple-owners
  reshard        host:port
                 --cluster-from <arg>
                 --cluster-to <arg>
                 --cluster-slots <arg>
                 --cluster-yes
                 --cluster-timeout <arg>
                 --cluster-pipeline <arg>
                 --cluster-replace
  rebalance      host:port
                 --cluster-weight <node1=w1...nodeN=wN>
                 --cluster-use-empty-masters
                 --cluster-timeout <arg>
                 --cluster-simulate
                 --cluster-pipeline <arg>
                 --cluster-threshold <arg>
                 --cluster-replace
  add-node       new_host:new_port existing_host:existing_port
                 --cluster-slave
                 --cluster-master-id <arg>
  del-node       host:port node_id
  call           host:port command arg arg .. arg
  set-timeout    host:port milliseconds
  import         host:port
                 --cluster-from <arg>
                 --cluster-copy
                 --cluster-replace
  help           

For check, fix, reshard, del-node, set-timeout you can specify the host and port of any working node in the cluster.

```

## 13 JavaApi操作集群模式

Redis集群操作主要使用`JedisCluster`这个类。

```java

    @Test
    public void jedisClusterTest() {
        String host = "192.168.5.154";
        Set<HostAndPort> jedisClusterNode = new HashSet<>();
        jedisClusterNode.add(new HostAndPort(host, 7001));
        jedisClusterNode.add(new HostAndPort(host, 7002));
        jedisClusterNode.add(new HostAndPort(host, 7003));
        jedisClusterNode.add(new HostAndPort(host, 7004));
        jedisClusterNode.add(new HostAndPort(host, 7005));
        jedisClusterNode.add(new HostAndPort(host, 7006));
        //jedsi连接池配置
        JedisPoolConfig cfg = new JedisPoolConfig();
        //最大实例数
        cfg.setMaxTotal(100);
        //最大空闲数
        cfg.setMaxIdle(20);
        //最大等待时间 -1 无限
        cfg.setMaxWaitMillis(-1);
        cfg.setTestOnBorrow(true);
        JedisCluster jc = new JedisCluster(jedisClusterNode, 6000, 100, cfg);
        //向单机操作一样 会自动从连接池中拿出一个实例来操作。
        jc.set("name", "illusory");
        jc.set("age", "22");
        jc.set("sex", "man");
        jc.set("addr", "cq");

        System.out.println(jc.set("name", "illusory"));
        System.out.println(jc.set("age", "22"));
        System.out.println(jc.set("sex", "man"));
        System.out.println(jc.set("addr", "cq"));

        System.out.println(jc.get("name"));
    }
```




## 参考

`http://www.runoob.com/redis/redis-hashes.html`
`https://www.jianshu.com/p/72443fef9554`