---
title: Redis
date: 2023-05-06 11:03:01
toc: true
cover: https://th.bing.com/th/id/OIP.wCDX4W_Glr7-BSlavnG2uAHaFj?pid=ImgDet&rs=1
categories: IT
tags:
  - Redis

---

# 基础

## 基础数据结构

### 5种基础数据结构

#### string（字符串）

Redis的string是动态字符串，可以被修改，内部实现结构类似于Java的ArrayList，采用预分配冗余空间的方式来减少内存的频繁分配，如果字符串长度小于1MB，扩容方式是加倍现有的空间，如果字符串长度超过1MB，扩容方式是只多扩1MB的空间。

Redis所有数据结构都以唯一的key字符串作为名称

一个常见的用途是缓存用户信息，将用户信息序列化成字符串，存入Redis缓存，取出用户信息的时候会经过一次反序列化的过程

mset、mget命令可以实现对多个字符串进行批量读写

ex后缀可以在set的时候指定过期时间

nx后缀可以在set的时候设置条件，如果key不存在才set

#### list（列表）

Redis的列表相当于Java的LinkedList，是双向链表，不是数组。

当 List 中的元素数量较少且元素都比较短时，Redis 通常会采用 ziplist 来存储。ziplist 是一个特殊的双向链表（本质上是一个字节数组），特殊之处在于：没有维护双向指针，prev、next，而是存储了上一个 entry 的长度和当前 entry 的长度，通过长度推算下一个元素。多个ziplist之间使用指针串联起来，这种结构叫做quicklist（快速链表）。

底层使用的是ziplist，即压缩列表

list常用来做异步队列使用，将需要延后处理的任务序列化成字符串放入列表

支持的命令有：rpush、rpop、lpush、lpop、lindex、lrange、ltrim

#### hash（字典）

相当于Java中的HashMap，跟HashMap一样，底层也是数组+链表的实现方式，不同的是Redis的hash的值只能是字符串，且rehash的方式也不一样，rehash 是指在哈希表发生扩容时进行的重新哈希操作。扩容过程包括创建新的哈希表、将旧哈希表的元素 rehash 到新哈希表中，redis采用了渐进式哈希扩容的策略，通过分多次操作逐步完成整个扩容过程，避免服务阻塞的问题。

具体来说，Redis 的哈希表扩容过程如下：

1. 创建新哈希表：系统会根据当前数据库的元素数量和设置的负载因子计算出扩容所需的最小桶数，然后创建一个新的哈希表，将其指针保存在旧哈希表的 rehashidx 属性中。

2. 逐步 rehash 元素：从旧哈希表中取出一个桶（或一个链表），并将其中的元素 rehash 到新哈希表中，如果新哈希表中的相应桶为空，则直接插入元素；如果不为空，则使用链表结构将其作为链表头插入。这个过程需要遍历旧哈希表中所有的非空桶，每次操作都只处理一个桶中的元素，避免一次性处理过多数据。

3. 完成 rehash：当旧哈希表中的所有元素都被 rehash 到新哈希表后，会释放旧哈希表占用的内存。


支持的命令有：hset、hget、hlen、hgetall等

#### set（集合）

Redis的集合set相当于Java的HashSet，是无序的，内部实现相当于一个特殊的字典，字典中所有的value都是NULL。

支持的命令有：sadd、scard、sismember等，scard用于获取计数值。

#### zset（有序集合）

类似于Java中的SortedSet和HashMap的结合体，同样是key-value结构，不同的是zset的value不是一个值，而是member和score两个值（可理解为member-score键值对），member是不重复的，按照score进行排序。底层实现使用的是跳表。

zset可以用来存储粉丝列表，value是粉丝的用户id，score是关注时间，按照关注时间排序，类似的，还可以用来存储学生的成绩。

zset支持的操作：

- zadd：向有序集合添加一个或多个成员，并指定对应的分数。
- zrank：获取成员在有序集合中的排名（从小到大）。
- zrevrank：获取成员在有序集合中的倒序排名（从大到小）。
- zrange：按照排名范围获取有序集合中的成员。
- zrevrange：按照倒序排名范围获取有序集合中的成员。
- zscore：获取成员的分数。
- zincrby：增加成员的分数。
- zrem：从有序集合中移除一个或多个成员。
- zcard：获取有序集合元素的总和

### 容器型数据结构的两条规则

list、set、hash、zset这四种数据结构都是容器型数据结构，容器型数据结构遵从两条规则：

1. create if not exists：如果添加元素时容器不存在，就创建。
2. drop if no elements：如果容器里没有元素，那么立即删除容器释放内存。

### 过期时间

Redis中所有对象都可以设置过期时间。例如，可以对一个hash对象设置过期时间，但是不能对齐某一个key-value设置过期时间。

需要注意的是，如果一个对象已经设置了过期时间，然后调用set修改了这个对象，那么之前设置是过期时间就会失效。

## 其它数据结构

### 位图

对于一些需要存储大量bool型数据的情况（比如一年内的签到数据），如果使用普通的key-value，存储空间消耗极大。为解决这个问题，Redis提供了位图数据结构（不是全新的数据结构，底层其实是string字符串）。位图的最小单位是比特（0或1）。

支持的命令有：getbit、setbit、bitcount、bitpos、bitfield等

其中bitcount和bitpos命令是位图的统计命令，bitcount用来统计指定范围内1的个数、bitops用来查找指定范围内出现的第一个0或1的位置。

bitfield命令可以实现一次性对指定位片段进行多位操作，bitfield有三个子命令，get、set、incrby。如果使用incrby命令时出现了溢出，Redis默认的处理方式是折返（wrap），即不对溢出进行特殊处理，溢出之后是什么值就取什么值。Redisbitfield命令的选择溢出策略的子命令是overflow，用户可以选择溢出行为，包括折返、失败（fail，报错并不予执行）、饱和截断（sat，超过了范围就停留在最大或最小值）。

常用的使用方式有：零存（对位值逐个设置）整取、零存零取、整存（使用字符串一次性填充所有位）零取。

### HyperLogLog

要统计网站上每个网页每天的UV数据总数，数据不需要太精确。由于统计UV需要去重，所以简单的方案是使用set，但是如果页面访问量很大，就存在浪费存储空间的问题。更好的解决方案是使用HyperLogLog，HyperLogLog 使用的内存消耗最多是12 KB，无论估算的基数有多大，它始终只占用 12 KB 的内存空间。

HyperLogLog提供了不精确的去重计数方案，标准误差是0.81%，这样的精确度可以满足UV统计需求。

HyperLogLog之所以内存消耗如此之小，是因为HyperLogLog的存储算法具备这一特点：

1. 当计数比较小时，它的存储空间采用稀疏矩阵存储。
2. 计数值增大到稀疏矩阵占用空间超过阈值后，才会一次性转变为稠密矩阵，占用12KB。

HyperLogLog提供的命令有：pfadd、pfcount、pfmerge

pfadd用来添加数据，pfcount用来获取计数值，pfmerge用来合并HyperLogLog

补充：

UV（Unique Visitor）数据指的是网站或应用程序的独立访问者数量。UV数据用于衡量网站或应用程序的受众规模和用户活跃度。UV数据通常基于用户的唯一标识符（如用户ID、Cookie、设备ID等）进行统计，以便区分不同的访问者。它可以帮助网站或应用程序的管理者了解其用户群体的规模、用户活跃度、用户留存率等重要指标，从而做出相应的优化和决策。

PV（Page View）数据指的是网站或应用程序的页面访问次数。PV数据记录了每个页面被访问的次数，无论是同一个用户多次访问同一个页面，还是不同用户访问同一个页面，每一次访问都计算为一次PV。PV数据可以帮助评估网站或应用程序的流量、页面热度以及用户行为。

### 布隆过滤器

虽然HyperLogLog数据结构能够对数据进行去重计数，但是不能用来判断数据是否已存在。布隆过滤器（Bloom Filter）就是专门用来解决这种问题的，用来判断对象是否存在，相比set能够节省90%的空间，唯一不足是不精确，有一定的误判概率。

布隆过滤器判断结果的真实性的规则是：

1. 如果布隆过滤器输出某个值存在，这个值可能不存在
2. 如果布隆过滤器输出某个值不存在，这个值一定不存在



布隆过滤器提供两条命令：bf.add（一次添加一个元素）、bf.madd（一次添加多个元素）、bf.exists

Redis的布隆过滤器是从Redis4.0开始以插件的形式添加到Redis中的，要使用Redis的布隆过滤器，需要安装对应插件。在使用时，布隆过滤器的initial_size（预计放入的元素数量）参数越大，error_rate（误判率）越小。



**布隆过滤器的原理：**

布隆过滤器底层的数据结构是一个大型的位数组，添加key时，使用几个不同的无偏hash函数，对添加到布隆过滤器的key进行hash，分别算出索引值，然后将索引值位置的值置为1。判断key是否存在时，对添加到布隆过滤器的key进行hash，分别算出索引值，然后看位数组中这几个位的值是否都是1，如果都是1，说明极有可能存在，如果不都是1，说明一定不存在。

所谓无偏就是能够把元素的hash值算得比较均匀，让key被hash映射到位数组中的位值比较随机。

**布隆过滤器的空间占用估计：**

计算布隆过滤器的空间占用估计需要两个参数，预计元素的数量（设为N）和错误率（设为F），可以得到两个输出，位数组的长度（设为L）和hash函数的最佳数量（设为K）：

K = 0.7 * (L / N)

F = 0.6185 ^ (L / N)

K和（L/N）成正比，F和（L/N）成反比。

在线计算布隆过滤器计算器：https://krisives.github.io/bloom-calculator

![image-20230522110209728](./Redis/image-20230522110209728.png)

**当实际元素数量超过设置的预计元素的数量：**

当实际元素数量超过设置的预计元素的数量，错误率会有陡峭的增大，设实际元素数量和设置的预计元素的数量比值为T，使用的hash函数的数量是K，错误率（设为F）的计算公式是：

F = (1 - 0.5^T) ^ K;

**布隆过滤器的其它应用：**

- 在爬虫系统中对URL进行去重。
- 在NoSQL数据库中通过内存中的布隆过滤器过滤掉不存在的row的请求。
- 邮箱系统的垃圾邮件过滤。

### GeoHash

Redis在3.2版本后增加了处理地理位置信息的模块Geo（底层使用的是zset），可以用于实现“附近的单车”、“附近的餐馆”这样的需要对地理位置距离进行排序的功能，在Redis中，是基于GeoHash算法实现的。

GeoHash算法将二维的经纬度数据映射到一维的整数（使用二刀法等刀法实现），要寻找附近的XXX时只需要找一维下的附近的点。计算后的地图元素的坐标都会变为整数，通过这个整数可以还原出元素的坐标，整数越长，还原出来的坐标值的损失程度就越小。GeoHash算法会对这个整数做一次Base32编码。

Base32 是一种数据编码机制，使用 32 个可打印字符（字母 A-Z 和数字 2-7）对任意字节数据进行编码的方案。Base32 将任意字符串按照字节进行切分，并将每个字节对应的二进制值（不足 8 比特高位补 0）串联起来，按照 5 比特一组进行切分，并将每组二进制值转换成十进制来对应 32 个可打印字符中的一个。由于数据的二进制传输是按照 8 比特一组进行（即一个字节），因此 Base32 按 5 比特切分的二进制数据必须是 40 比特的倍数（5 和 8 的最小公倍数）。不足 40 比特的倍数则通过填充符号“=”来补齐。

支持的命令有：geoadd、geodist、geopos、georadiusbymember、georadius

- geodist：获取两个元素之间的距离
- geopos：获取集合中任意元素的经纬度坐标
- georadiusbymember：查询元素附近的其它元素
- georadius：查找经纬度坐标附近的其它元素

### Stream

Redis Stream是Redis 5.0版本中新增的数据结构，是一个支持多播的可持久化的消息队列。

Stream的消息有定长的功能，在 xadd 的指令中提供了一定长长度参数 maxlen，就可以实现清除旧有超长的消息。

#### 消费组

每个Stream都可以挂载多个消费组（Consumer Group），每个消费组会有一个游标（last_delivered_id），用于表示当前消费组以及消费到哪条消息。

消费组之间是独立的

一个消费组可以挂载多个消费者，任意一个消费者读取了消息都会使游标向前移动。

##### 创建消费组

消费组不会自动创建，创建消费组的命令是xgroup create，创建消费组需要提供起始消息 ID 参数用来初始化 last_delivered_id 变量。



##### 独立消费

可以不定义消费组，将 Stream 当成普通的消息队列（list）来使用。

#### 消息

##### 消息ID

消息 ID 的形式是 TimestampInMillis-sequence，例如 1527846880572-5，它表示当前的消息再毫秒时间戳 1527846880572 时产生，并且是该毫秒内产生的第 5 条消息。消息 ID 可以由服务器自动生成，也可以由客户端自己指定，但是形式必须是 “整数-整数”，而且后面加入的消息的 ID 必须要大于前面的消息 ID。

##### 消息内容

消息内容的形式的键值对。

##### 消息操作

1）xadd：向 Stream 追加消息。

2）xdel：向 Stream 中删除消息，这里的删除仅仅是设置标志位，不影响消息总长度。

3）xrange：获取 Stream 中的消息列表，会自动过滤已经删除的消息。

4）xlen：获取 Stream 消息长度。

5）del：删除整个 Stream 消息列表的所有消息。

#### 消费者

##### 消费消息

消费者使用`XREAD`或`XREADGROUP GROUP`命令从Redis Stream中读取消息。

使用 XREAD 以阻塞或非阻塞方式获取消息列表 ，语法格式：

```
XREAD [COUNT count] [BLOCK milliseconds] STREAMS key [key ...] id [id ...]
```

- **count** ：数量
- **milliseconds** ：可选，阻塞毫秒数，没有设置就是非阻塞模式
- **key** ：队列名
- **id** ：消息 ID

示例：

```
# 从 Stream 头部读取两条消息
XREAD COUNT 2 STREAMS mystream writers 0-0 0-0
```

使用 XREADGROUP GROUP 读取消费组中的消息，语法格式：

```
XREADGROUP GROUP group consumer [COUNT count] [BLOCK milliseconds] [NOACK] STREAMS key [key ...] ID [ID ...]
```

- **group** ：消费组名
- **consumer** ：消费者名。
- **count** ： 读取数量。
- **milliseconds** ： 阻塞毫秒数。
- **key** ： 队列名。
- **ID** ： 消息 ID。

示例：

```
XREADGROUP GROUP consumer-group-name consumer-name COUNT 1 STREAMS mystream >
```

消息 ID可以指定读取的起始位置，如`0`表示从最早的消息开始读取，或者使用特殊符号`>`表示从当前最新的消息开始读取。

处理完消息后，消费者需要使用`XACK`命令确认消息的处理完成。

##### pending_ids

每个消费者中维持一个状态变量pending_ids，简称为PEL(Pending Entries List)，记录了当前已经被客户端读取的但尚未被ACK的消息。

## Redis操作命令

### scan

scan是一个Redis命令，用于从海量的key中找出满足特定前缀的key列表。相比Redis之前提供的keys命令（也可以完成这一功能）scan具备以下特点：

- 虽然复杂度也是O(n)，但它是通过游标分步进行的，不会阻塞线程
- 提供了limit参数，可以控制服务器单次遍历的最大条数
- 返回的结果可能有重复（key存储在hash中，hash缩容时会重复遍历正在遍历的槽），需要客户端去重

scan指令返回的游标就是第一维数组的位置索引（槽，slot），limit参数就表示需要遍历的槽位数

scan的遍历顺序是高进位加法，高进位加法从左边加，进位往右边移动。

对于rehash中的字典，scan会同时访问新旧两个数据结构

除了有可以遍历key的scan指令外，还有针对其它容器集合的遍历操作：

- sscan：遍历set集合
- zscan：遍历zset集合
- hscan：遍历hash字典中的元素

scan还可以用来查找大key，方法是对于每个扫描出来的key，使用type指令获得key的类型，然后使用相应数据结构的size或len方法来得到value的大小，对于每一种类型，将大小排名的前若干名作为扫描结果输出。要编写上面过程的脚本比较繁琐，不过Redis官方已经在redis-cli指令中提供了这样的扫描功能。

示例如下：

```shell
redis-cli -h 127.0.0.1 -0 6379 --bigkeys
```

还可以指定睡眠时间：

```shell
redis-cli -h 127.0.0.1 -0 6379 --bigkeys -i 0.1
```

上面这条指令可以实现每个100条scan指令就会休眠0.1s

补充：

大key：指的是key对应的value值大，在实际业务中要尽量避免大key的产生，原因是大key会带来如下坏处：

- 请求阻塞：redis为单线程，读、写或删除大key需要较长的处理时间，会阻塞后续的请求处理。
- 网络阻塞：大key会明显需要更长的传输时间，在整个传输时间内，占用大量的带宽，导致网络阻塞。
- 占用内存：大 key 在 Redis 内部通常会占用较多的内存空间，导致 Redis 的整体内存使用率变高，可能会引起内存溢出等问题。

## Redis的应用

### 分布式锁

Redis可以用作分布式锁。

需要注意的点有：获取锁和设置超时时间需要是组合在一起的原子操作。

可重入性的一个实现方案是使用线程的ThreadLocal变量存储当前持有锁的计数

请求阻塞等待的一个实现方案是使用延时队列

### 延时队列（zset做异步消息队列）

对于消费者数量较少等简单的使用情景，可以使用Redis创建消息队列，简化操作，但是需要注意的是，Redis不是专业的消息中间件，没有非常多的高级特性，没有ack保证，如果对消息的可靠性要求较高，那就不适合使用Redis。

Redis用来做异步消息队列的数据结构通常是list。当队列为空，消费者还是会不断地通过pop操作尝试获取数据，进行空轮询，浪费系统资源，解决方法是，让消费者线程进入sleep状态，一段时间比如1秒后再苏醒继续执行。

但是这会造成系统延迟，更好的解决方案是使用blpop或brpop，前缀b代表的是blocking（阻塞读），阻塞读保证在队列为空的时候，会立即进入休眠状态，一旦数据到来，则立刻醒过来。消息的延迟几乎为0。

当发生锁竞争时，要实现处理请求的阻塞等待，可以使用zset作为队列（Jedis提供的RedisDelayingQueue的底层就是基于Redis的zset数据结构），将冲突的请求放入队列延后处理。

### 简单限流

一个简单的限流策略的例子是，在指定时间内限制某个请求只允许发生N次。使用Redis可以实现这一功能。

实现示例如下：

```java
public class RedisLimiter {
    private final Jedis jedis;

    public RedisLimiter(Jedis jedis) {
        this.jedis = jedis;
    }
    
    public boolean isActionAllowed(String userId, String action, int period, int maxCount) {
        String key = String.format("hist:%s:%s", userId, action);
        long nowTimeMillis = System.currentTimeMillis();
        Pipeline pipeline = jedis.pipelined();
        pipeline.zadd(key, nowTimeMillis, String.valueOf(nowTimeMillis)); // 向key中添加member-score，方法的参数是zadd(String key, double score, String member)
        pipeline.zremrangeByScore(key, 0, nowTimeMillis - period * 1000L); //删除key里面过期的member-score，只保留最近period * 1000L毫秒内的member-score
        Response<Long> count = pipeline.zcard(key);
        pipeline.expire(key, period); //设置过期时间
        pipeline.close();
        pipeline.sync();
        Long value = count.get();
        //System.out.println("value = " + value); //如果没有删除缓存，就可能看到value不是从1开始的
        return count.get() <= maxCount;
    }
    
    public void deleteCache(String userId, String action) {
        String key = String.format("hist:%s:%s", userId, action);
        jedis.del(key);
    }

    public static void main(String[] args) throws InterruptedException {
        Jedis jedis = new Jedis();
        RedisLimiter redisLimiter = new RedisLimiter(jedis);
        final String USER_ID = "user";
        final String ACTION = "getRequest";
        redisLimiter.deleteCache(USER_ID, ACTION); //删除Redis缓存
        for (int i = 0; i < 20; i++) {
            Thread.sleep(1); //sleep一毫秒，确保isActionAllowed函数中的nowTimeMillis值每次都不同
            System.out.println(redisLimiter.isActionAllowed(USER_ID, ACTION, 60, 5));
        }
    }
}
```

输出结果如下，只有当缓存中存储的次数不超过maxCount允许的数量（例子中是5），才可以返回true：

![image-20230522172919614](./Redis/image-20230522172919614.png)

### 漏斗限流（Redis-Cell模块的cl.throttle命令）

漏斗（Funnel）限流是最常用的限流算法之一。漏斗的剩余空间代表着行为当前可以进行的数量，漏斗的流水速率代表系统允许该行为的最大频率。要实现这一算法需要存储几个参数，比如漏斗容量、流水速率、漏斗剩余空间，可以考虑使用Redis的hash数据结构实现，但是存在一个问题，就是无法保证漏斗容量计算时涉及到的取值、运算、写值的三个过程的原子性。

Redis-Cell模块提供了漏斗算法的实现，解决了这一问题，该模块的命令只有一个，cl.throttle，该命令的使用格式如下：

```shell
cl.throttle [key] [capacity] [velocity] [apply 1 operation](optional)
```

1. key：键
2. capacity：漏斗容量
3. velocity：流水速率
4. apply 1 operation：添加一个数，是可选的。

示例：

```shell
cl.throttle 1001 15 30 60 1
```

输出如下：

1)(Integer) 0        # 0表示允许，1表示拒绝

2)(Integer) 15      # 漏斗容量是15

3)(Integer) 14      # 漏斗剩余空间是14

4)(Integer) -1       # 如果被拒绝了，多长时间后重试

5)(Integer) 2         # 多长时间后漏斗完全空出来

# 数据传输与网络

## Redis的特点

- 是单线程程序。除Redis外，Node.js和Nginx也都是单线程。
- 数据存储在内存中
-  I/O多路复用，是Redis能够处理大量客户端连接的原因
- 速度快

Redis 速度快的原因主要包括以下几点：

1. 内存存储：Redis 将数据存放在内存中，而不是硬盘上。因为内存访问速度比硬盘快得多，所以 Redis 能够达到非常快的读写速度，这也是 Redis 被称为高性能数据库的重要原因之一。
2. 单线程模型：Redis 使用单线程模型，即使用一个线程来处理所有的客户端请求，这使得 Redis 可以避免锁竞争、多线程切换等问题，从而提高了效率。
3. 高效的网络 IO 模型：Redis 使用 I/O 多路复用技术来实现高效的网络 IO 模型，这种模型可以同时管理多个客户端连接，并且每个连接都只被唤醒一次，从而减少了系统调用次数，提高了效率。
4. 高效的数据结构：Redis 支持多种数据结构，如字符串、哈希表、列表、集合、有序集合等，这些数据结构经过优化和精简，能够快速地进行插入、删除、查询等操作，从而提高了性能。比如rehash就是优化的一个例子。
5. 高效的持久化和异步处理机制：Redis 提供了 RDB 和 AOF 两种持久化方案，通过定期或追加记录将内存中的数据同步到硬盘。同时 Redis 还具有异步处理机制，可以延迟一部分 I/O 操作，让 CPU 可以在更多的时间内执行其他操作，从而提高了系统的整体效率。

## I/O线程模型

### 同步与异步

同步指的是在发出一个功能调用时，在没有得到结果之前，该调用就不返回。也就是必须一件一件事做，等前一件做完了才能做下一件事。

异步的概念和同步相对，当一个异步过程调用发出后，调用者不需要立刻得到结果。调用被执行完成后，会通知调用者。

### 5种主要的IO模型

1. 阻塞IO模型

   当我们调用套接字的读写放方法，默认是阻塞的，read操作是在没有读取到字节时线程阻塞，write操作是在写缓冲区已满时阻塞。

   典型应用：BIO（Blocking I/O）

2. 非阻塞IO模型

   非阻塞IO在套接字对象上设置了non_blocking，读写方法不会阻塞，会反复地发起读/写请求，对于read操作，当内核准备好数据之后就进行读，对于写操作，当写缓冲区的有空闲空间就进行写。

   典型应用：Java NIO（Non-blocking I/O，New I/O）

3. 多路复用IO模型

   一种简单的多路复用API是select，多个的进程的IO注册到一个复用器（select）上，然后用一个进程调用该select，select会监听所有注册进来的IO。

   如果select没有监听的IO在内核缓冲区都没有可读数据，select调用进程会被阻塞；而当任一IO在内核缓冲区中有可数据时，select调用就会返回；

   典型应用：epoll（linux系统的性能最好的多路复用API）。

4. 异步IO模型

   当进程发起一个IO操作，进程返回（不阻塞），但也不能返回果结；内核把整个IO处理完后，会通知进程结果。如果IO操作成功则进程直接获取到数据。

   典型应用：Java AIO（Asynchronous I/O）

5. 信号驱动IO模型

   当进程发起一个IO操作，会向内核注册一个信号处理函数，然后进程返回不阻塞；当内核数据就绪时会发送一个信号给进程，进程便在信号处理函数中调用IO读取数据。

## Redis序列化协议（RESP）

RESP是Redis序列化协议（Redis Serialization Protocal）的简写，是一种直观的文本协议。

RESP将传输的数据分为5种最小单元类型，单元结束时统一加上回车换行符号\r\n。[规则如下](https://redis.io/docs/reference/protocol-spec/)：

In RESP, the first byte determines the data type:

- For **Simple Strings**, the first byte of the reply is "+"
- For **Errors**, the first byte of the reply is "-"
- For **Integers**, the first byte of the reply is ":"
- For **Bulk Strings**, the first byte of the reply is "$". A "$" byte followed by the number of bytes composing the string (a prefixed length), terminated by CRLF.
- For **Arrays**, the first byte of the reply is "`*`"

NULL用多行字符串表示，不过长度要写成-1：

```
$-1\r\n
```

客户端向服务端发送的指令只有一种格式，多行字符串数组。比如指令`set author codehole`会被序列化为下面的字符串：

```
*3\r\n$3\r\nset\r\n$6\r\nauthor\r\n$8\r\ncodehole\r\n
```

服务端向客户端返回的数据结构有的比较复杂，不过也是以上五种基本类型的组合。例如scan命令的返回给客户端的结果，scan命令返回的是一个嵌套数组，数组的第一个值表示游标的值，如果这个值为零，说明已经遍历完毕。scan返回结果示例：

```
*2\r\n$1\r\n0\r\n*3\r\n$4\r\ninfo\r\n$5\r\nbooks\r\n$6author\r\n
```

里面嵌套了一个数组

```
*3\r\n$4\r\ninfo\r\n$5\r\nbooks\r\n$6author\r\n
```

虽然RESP协议里面有大量冗余的回车换行符，但是依然是非常受欢迎的一个文本协议。

## 管道

Redis的管道（Pipeline）不是由Redis服务器提供的技术，而是由客户端提供的。

Redis管道通过将多个命令打包在一起，然后一次性发送给Redis服务器，在一次通信中获得多个命令的执行结果。这样就可以减少通信次数，提高性能。

使用Redis自带的压力测试工具redis-benchmark,，可以测试出设置不同的单个管道内并行的请求数量所带来的QPS（Queries Per Second，每秒查询率）的改变。

## 事务

Redis事务的操作指令有multi、exec、discard、watch。分别表示事务的开始、提交、丢弃、监视变量。

所有指令在exec之前不会执行，而是缓存在服务器的事务队列中。执行完毕后一次性返回所有指令的运行结果。

Redis的事务不具备原子性，仅仅实现了事务的“串行化”，当前执行的事务不被其它的事务打断。

Redis的事务通常会结合pipeline一起使用，可以将多次IO操作合并为一次。在Python的Redis客户端，Redis执行事务时要强制使用pipeline。

**watch**

在 Redis 中，watch命令用来监视某个键，在服务器收到exec命令将要执行缓存的事务队列时，Redis会检查自变量被watch之后是否被改过。如果该键watch之后和exec之前被修改过，exec就会返回 NULL告诉客户端事务执行失败，这个时候客户端一般会选择重试。

需要注意的是，Redis禁止在multi和exec之间执行watch命令，必须在multi之前watch变量。

示例（Java语言中使用watch命令）：

```java
public class WatchUsage {
    public static String keyFor(String userId) {
        return String.format("account_%s", userId);
    }

    public static int doubleAccount(Jedis jedis, String userId) {
        String key = keyFor(userId);
        while (true) {
            jedis.watch(key);
            int value = Integer.parseInt(jedis.get(key));
            value <<= 1; //乘以2
            Transaction transaction = jedis.multi();
            transaction.set(key, String.valueOf(value));
            List<Object> result = transaction.exec();
            if (result != null) {
                break;
            }
        }
        return Integer.parseInt(jedis.get(key));
    }

    public static void main(String[] args) {
        Jedis jedis = new Jedis();
        String userId = "1001";
        String key = keyFor(userId);
        jedis.setnx(key, String.valueOf(5));
        System.out.println(doubleAccount(jedis, userId));
        jedis.close();
        /*
        输出结果：
        	10
        */
    }
}
```



## 消息多播（PubSub）

消息多播允许生产者只生产一次消息，由中间件负责将消息复制到多个消息队列，每个消费队列由对应的消费组进行消费。这是分布式系统常用的一种解耦方式，用于将多个消费组的逻辑进行拆分。

Redis中支持消息多播的模块是PubSub（PublisherSubscriber，发布者-订阅者模式）

Redis提供的模式订阅命令是`subscribe`，Redis还提供了`psubscribe`命令（pattern subscribe），可以实现通过模式匹配订阅主题。

命令：

1. subscribe：订阅主题
2. psubscribe：通过模式匹配订阅主题
3. unsubscribe：取消订阅主题
4. unpsubscribe：通过模式匹配取消订阅主题

PubSub的缺点：

1. 当生产者发送消息时，如果消费者下线没有收到消息，那么该消息对于该消费者来说就是彻底丢失了

正式因为PubSub有这个缺点，在消息队列的领域几乎找不到合适的应用场景。Redis5.0新增了Stream数据结构，给Redis带来了持久化的消息队列，从此PubSub退出作为消息队列的技术方案选项。

# 持久化

### RDB日志（内存快照）

RDB（Redis DataBase）日志（内存快照）是内存数据的二进制序列化，是全量备份。

内存快照要求Redis必须进行文件IO操作，而Redis是单线程程序，如果一边处理业务请求，一边进行文件IO操作，会降低处理业务请求的性能，还有个问题是，这种操作下，内存数据一边被持久化一边被修改，快照就不是对一个时间点的记录，而是成了多个时间点交错的记录，无法使用。

Redis使用操作系统多进程COW（Copy On Write）机制来实现快照持久化。

Copy-On-Write（COW）是一种操作系统中常用的技术，其基本思想是：当多个进程需要访问同一块内存地址时，操作系统会将这块内存标识为只读，并且在任何进程试图写入该内存前，都会复制一份副本供该进程使用。这样就能够保证每个进程都拥有自己的独立内存空间，而不会互相干扰。

### AOF日志

AOF（Append Only File）日志是内存数据修改的指令记录文本，是增量备份。

Redis收到客户端的修改命令后，进行参数校验、逻辑处理，如果没问题，就将该指令文本存储到AOF日志中，即先执行指令再将存储日志。



AOF日志在长期的运行过程中会变得十分庞大，数据库重启时需要加载AOF日志进行指令重放，这个过程就会很漫长，所以需要定期进行AOF重写，给AOF日志进行瘦身。

Redis提供了bgrewriteaof命令用于对AOF日志进行瘦身，其原理是开辟一个子进程对内存进行遍历，转换成一系列Redis的操作命令，序列化到一个新的AOF日志文件中，序列化完成后再将操作期间发生的增量AOF日志追加到这个新的AOF日志文件中，追加完毕后就可以替代旧的AOF日志文件了。



当程序对AOF日志文件进行写操作时，实际上是将内容写到了以内核为文件描述符分配的一个内存缓存中，然后内核会异步地将脏数据刷回到磁盘。这就意味着，如果突然宕机，AOF日志还没有完全刷新到磁盘中，就会出现日志丢失。Linux的glibc提供的fsync(int fd)函数可以将指定文件的内容强制从内核缓存刷新到磁盘，只要Redis进行实时调用fsync函数就可以保证AOF日志不丢失。但是fsync是一个磁盘IO操作，很慢，如果Redis执行一条指令就要fsync一次，那么会严重降低Redis的性能。所以在生产环境的服务器中，Redis通常是每隔1s左右执行一次fsync操作，这个1s的周期是可以配置的，是在安全性和性能间做的折中。

因为RDB和AOF都会加重系统的负担，所以通常Redis的主节点不会进行持久化操作，持久化操作主要在从节点进行，这是因为从节点没有来自客户端请求的压力，系统资源充足。

### 混合持久化

使用RDB恢复内存状态会丢失备份后修改的数据，而使用AOF日志的全量文件重放又相对RDB慢很多，Redis为解决这个问题，从Redis4.0开始，引入了一个新的持久化选项，混合持久化。

混合持久化的持久化方式是指生成RDB全量日志和该RDB的AOF增量日志，在Redis重启的时候，先加载RDB日志，再加载AOF日志。

# 内存管理

## 内存回收机制

被删除的key分散在很多页面中，这个页面可能还有其它正在使用的key，操作系统是以页为单位进行内存回收的，这个页上只要还有一个key在使用，那这个页就不能回收。

Redis虽然无法保证立即回收已经删除key的内存，但是它会重新使用哪些尚未回收的空闲内存。

## 内存分配机制

Redis在内存分配方面，直接使用了第三方的内存分配库，目前Redis使用jemalloc（facebook）库来管理内存，也可以切换到tcmalloc（google），因为jemalloc比tcmalloc性能稍好，所以Redis默认使用jemalloc。

通过`info memory`可以查看Redis使用的是哪个第三方的内存分配库。

## 数据过期机制

### 定时删除

EXPIRE命令可以为指定的键设置过期时间，时间到达后，这些建会被自动删除。

serverCron函数会定时触expire.c下的activeExpireCycle函数，该函数会清除数据库中的过期数据，该函数可以设置最长执行时间和每次删除操作删除的最大的key数量。以避免删除操作延时过长。

### 惰性删除

惰性删除是当用户查询键时，检测键是否过期，如果键已经过期，则删除该键。该操作由expireIfNeeded函数完成。

## 数据淘汰策略

### Redis支持的数据淘汰策略

[Redis官方给出的数据淘汰策略（Eviction policies）文档](https://redis.io/docs/reference/eviction/)。下面是从官网复制的Redis支持的数据淘汰策略及其解释。

The exact behavior Redis follows when the `maxmemory` limit is reached is configured using the `maxmemory-policy` configuration directive.

The following policies are available:

- **noeviction**: New values aren’t saved when memory limit is reached. When a database uses replication, this applies to the primary database
- **allkeys-lru**: Keeps most recently used keys; removes least recently used (LRU) keys
- **allkeys-lfu**: Keeps frequently used keys; removes least frequently used (LFU) keys
- **volatile-lru**: Removes least recently used keys with the `expire` field set to `true`.
- **volatile-lfu**: Removes least frequently used keys with the `expire` field set to `true`.
- **allkeys-random**: Randomly removes keys to make space for the new data added.
- **volatile-random**: Randomly removes keys with `expire` field set to `true`.
- **volatile-ttl**: Removes keys with `expire` field set to `true` and the shortest remaining time-to-live (TTL) value.

### LRU

LRU（Least Recently Used）：如果一个数据在最近一段时间内没有被访问，那么可以认为它未来被访问的概率很小。当空间满时，最久没有访问的数据会最先被淘汰。

LRU记录的是时间戳。

Redis的LRU算法是一种近似LRU算法，没有维护key的被访问时间顺序，而是采用随机采样出N个（比如5个，可以设置），然后淘汰掉最旧的key，为能够识别出key的访问时间，Redis给每个key增加了一个额外的字段，最后一次被访问的时间戳。

### LFU

LFU（Least Frequently Used）：如果一个数据在最近一段时间内很少被访问，那么认为将来它被访问的可能性很小。当空间满时，最小频率访问的数据最先被淘汰。

LFU记录的是使用次数。

Redis会根据键的空闲事件对LFU计数进行衰减。



# 集群

## 主从同步

Redis的主从数据是异步复制的，所以分布式Redis系统不满足CAP理论中的一致性（C，Consistency）。

即使在主从网络断开的情况下，主从节点依旧可以向外提供服务，所以Redis满足可用性（A，Availability）。

Redis支持主从同步和从从同步（用以减轻主节点同步的负担）

### 快照（RDB）同步

快照同步是在主节点上进行一次bgsave，将当前内存的数据快照存储到磁盘，再将快照文件传输到从节点，从节点接收完毕后，执行全量加载，加载完毕后通知主节点进行增量同步。

进行快照同步时，文件IO操作十分耗时，且会影响fsync的执行，所以从Redis2.8.18开始支持无盘复制，主服务器通过套接字直接将快照内容发送给从节点，从节点将接收到的内容存储到硬盘文件。

### 增量（AOF）同步

Redis增量同步同步的是指令流，主节点会将那些对自己的状态产生修改性影响的指令记录在本地的内存buffer中，然后异步地将buffer中的指令同步到从节点。

Redis的buffer是一个定长的环形数组，如果数组内容满了，就会从头开始覆盖前面的内容，如果因为网络状况不好等原因没有及时的同步，那么没有同步的指令可能会被后续的指令覆盖，此时就需要使用快照同步。如果进行快照同步的过程中，buffer又发生了覆盖，就会引发又一次的快照同步，所以如果buffer大小设置不当，可能引起快照同步的死循环。

### 增加节点

当节点添加到集群，会先进行一次快照同步，完成后再继续进行增量同步。

### 同步复制

Redis3.0之后增加了wait指令，可以将主从复制由异步改为同步，可以设置需要同步的从节点的数量和最长等待时间（-1表示无限等待）。

## Sentinel

Redis提供的Sentinel（哨兵）可以实现集群主节点发生故障后自动进行主从切换。具体作用如下：

- Sentinel能够持续监控主从节点的在线状况。

- 客户端连接集群时，会首先连接Sentinel，通过Sentinel查询主节点的地址，当主节点发生故障时，Sentinel会将最新的主节点地址告诉客户端。
- Sentinel无法保证主从同步因为异步而在主节点下线后产生消息丢失，但是会采取措施限制主从延迟，方式是设置参数`min-slaves-to-write`和`min-slaves-max-tag`，第一个参数表示主节点至少有多少个从节点正在进行正常复制，如果不够，就停止写服务。正常复制是含义是由第二个参数控制的，它的单位是秒，表示如果在多少秒内没有收到从节点的反馈，就意味着从节点的同步不正常。
- 主从切换后，为使客户端“知道”地址变更了，Sentinel会关闭所有的客户端连接，在重连时使客户端使用新的地址。

## Redis集群方案

### Codis

Codis是Redis的集群代理中间件，当客户端向Codis发送指令时，Codis负责将指令转发到后面的Redis实例执行，并将返回结构转回给客户端。Code上挂接的所有Redis实例构成一个Redis集群，当集群空间不足时，可以通过动态增加Redis实例来实现扩容。

因为单个Codis代理能支撑的QPS有限，可以启动多个Codis代理增加QPS，还可以起到容灾的功能。

**Codis的槽位定位算法：**

Codis将key转发到对应Redis实例的定位机制是通过划分槽位实现的。Codis默认将所有的key划分为1024个槽位（slot），对客户端传入的key进行crc32运算计算hash值，然后用这个hash值对1024取余，这个余数就是key所属的槽位。

每个槽位都会映射到多个Redis实例。Codis会维护槽位和Redis实例的对应关系。当使用到多个Codis实例，就需要对不同Codis实例的槽位信息进行同步，需要使用一个分布式配置存储库如zookeeper，Codis会监听到槽位信息的变化并同步槽位信息。

**Codis处理Redis扩容：**

当Redis扩容（增加Redis实例）时，会对槽位关系进行调整，并进行自动均衡。

**使用Codis的缺点：**

由于key分散在不同的Redis实例中，所以不再支持事务。

客户端需要多走一个网络节点（Codis节点）才能到达Redis，性能上比直接访问Redis性能有所下降。

**Codis的优点：**

Codis在设计上比Redis Cluster简单，将分布式配置问题交给了第三方（zookeeper或etcd）负责，省去了编写和维护分布式一致性的工作。而Redis Cluster自己实现了这一点，混合使用了Raft和Gossip协议，有大量需要调优的配置参数，集群出现故障时不容易排查。

### Codis和Redis Cluster的不同

1. Codis的默认槽位数是1024，而Redis Cluster的默认槽位数是16382
2. Codis是中心化的（需要使用如zookeeper维护配置信息），Redis Cluster是去中心化的（通过Raft和Gossip协议自行维护配置信息）
3. 客户端访问Codis维护的Redis集群每次都需要经过Codis节点，而客户端访问Redis Cluster维护的Redis集群可以直接根据获取到的配置信息定位到Redis实例
4. Codis默认使用crc32算法计算key的hash值，Redis Cluster默认使用的是crc16

### Redis Cluster

与Codis不同，Redis Cluster是去中心化的，该集群由三个Redis节点组成，每个节点负责整个集群的一部分数据，它们之间使用一种特殊的二进制协议交互集群信息。

Redis Cluster将所有key划分为16382个槽位，每个节点负责存储其中一部分槽位映射信息。客户端连接集群时会得到一份集群的槽位配置信息，客户端可以直接根据该信息定位到目标节点（Redis实例）。

**Redis Cluster的槽位定位算法：**

Redis Cluster默认对key使用crs16算法进行hash，得到一个整数值，然后对16382取余得到具体的槽位

Redis Cluster还允许用户强制把指定key挂在特定的槽位上，实现方法是在key字符串上添加tag标记。

**Redis Cluster的槽位纠错机制：**

当客户端向一个错误节点发出了指令，该节点会发现指令的key所在的槽位并不归自己管理，这时它会向客户端发送一个特殊的跳跃指令（MOVED指令）携带目标操作的节点地址，告诉客户端去连接这个节点以获取数据。

**Redis Cluster的数据迁移策略：**

Redis Cluster的数据迁移的单位是槽，提供的迁移工具是redis-trib，redis-trib首先会在源节点和目标节点设置好中间过渡状态，然后再一次性获取源节点槽位的所有key列表，再逐个key进行迁移。每个key迁移到过程是以源节点作为目标节点的客户端，源节点对当前key执行dump指令得到序列化内容，然后向目标节点发送restore指令携带序列化的内容作为参数，目标节点再反序列化就可以把内容恢复到目标节点的内存中。然后返回给源节点OK信息，源节点收到后把当前节点的key删除。

当源节点正在进行对key的数据迁移，源节点的主线程就会处于阻塞状态，直到key被成功删除。在迁移过程中如果每个key都很小，migrate迁移指令会执行的很快，而如果key比较大，就会导致阻塞源节点的正常服务。

因为migrate命令是同步阻塞的，因此不会存在一个key正在被迁移又同时被读写的情况，但由于一个slot下可能有部分key被迁移完成，部分key正在等待迁移的情况，因此如果读写的key所属的slot正在被迁移，redis-cluster做如下处理：

1. 客户端根据本地slots缓存发送命令到源节点，如果存在键对象则直接指向并返回结果给客户端。
2. 如果key对象不存在，但key所在的slot属于本节点，则可能存在于目标节点，这时源节点会回复ASK重定向异常`-ASK targetNodeAddr`
3. 客户端从ASK重定向异常提取出目标节点的地址信息（targetNodeAddr），发送asking命令到目标节点。目标节点如果key存在则执行，不存在则返回不存在信息。

**Redis Cluster处理网络抖动：**

网络抖动是突然间部分连接不可访问，然后很快又恢复正常的一种现象。

为解决网络抖动的问题，Redis Cluster提供了配置参数cluster-node-timeout，表示当前某个节点持续timeout的时间失联时，才认定该节点出现故障。如果没有这一配置选项，网络抖动会导致频繁的主从切换。

#### Raft协议

#### Gossip协议

可能下线（PFail）和确定下线（Fail）：因为Redis Cluster是去中心化的，一个节点认为某个节点失联了并不代表所有节点都认为它失联了，所以集群需要进行一次协商，只有当大多数节点都认为某节点失联了，集群才做出节点已经下线的判断。

Redis Cluster采用Gossip协议来广播自己的状态以及改变对整个集群节点的在线状态。

# 拓展

## info指令

- info stats：查看Redis 每秒执行多少次指令。
- info clients：查看Redis 连接了多少客户端。
- rejected_connections：查看因为超出大量连接限制而被拒接的客户端连接次数。如果这个数字很大意味着服务器的最大连接数设置的过低，需要调整 maxclients 参数。其默认值为 1w。
-  info memory：查看Redis 内存占用多大
- info replication：查看复制积压缓冲区大小





# Reference

- 钱文品. Redis深度历险:核心原理与应用实践. 北京: 电子工业出版社, 2019.1.



