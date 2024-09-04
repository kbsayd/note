## Normal

```plain
TYPE key    获取key的存储类型，可以返回string, list, set, zset 和 hash等不同的类型。
TTL key    获取 key剩余的过期时间，以秒为单位   -1：永远不过期。
DEL key [key …]    删除指定的一批keys，返回值：返回值是被删除的 key 的数量，如果删除中的某些key不存在，则直接忽略。
EXISTS key [key …]    检查给定 key 是否存在。1 如果key存在 0 如果key不存在
expire key seconds  设置过期时间秒
persist key      设置永不过期 
数据操作返回值
 表示运行结果是否成功
 (integer)0 一 false 失败
 (integer)1 - true 成功
 表示运行结果值
 (integer)3一 3
 环价
 (integer)1- 1
 数据未获取到 
 (nil) 等同于null
```
Redis存储键值对，key一定是string，下面讨论value类型
## String

```plain
整数，小数，字符串，批量操作
set key value [EX|PX seconds]  [NX|XX]   
设置指定 key 的值。如果 key 存在时会直接覆盖原来的值
ex seconds：设置键key的过期时间，单位时秒；正常执行返回OK，否则，如果没有设置条件则返回nil。  
nx：只能做添加操作    ex：存在则更新，不存在则不操作
EX seconds：设置失效时长，单位秒
PX milliseconds：设置失效时长，单位毫秒
setnx key value  等价于  SET key value NX（不会覆盖已有的，会添加没有的）
setex key seconds value  key值设置为value ，并key过期时间设为seconds秒钟，如果key存在则覆盖原有值
GET key    获取指定key的值，key不存在，返回特殊值nil。
mset key value [key value …]    批量存放键值
mget key [key …]   批量获取键的值，如果获取的某个不存在则返回（nil），其它正常返回
append key value     追加内容，成功后返回当前key的长度
getset key value   设置更新key值，先原有的值返回出来，并设置新值，如果key不存在时使用getset则返回nil，并设置新值
incr key    key数字值增一，并返回增加后的值
incrby key increment    ey中数字值增加指定步长increment，并返回增加后的值，也可以是负数
decr key    key数字值减一，并返回减后的值。
decrby key decrement  key中储存的数字值减指定步长increment，并返回减后的值 
应用：热点数据加速访问
利用incr作为计数器，统计数数；
分布式锁（ET lock_key unique_value NX PX 10000）；
共享session；
```
## Hash

```plain
hset key field value [field value …]
hget key field
hdel key field [field …]
hexists key field  查看哈希表的指定字段field是否存在，1存在，0不存在
hgetall key   返回存储在key中的哈希表中所有的field和value
hkeys key    返回存储在key中哈希表的所有field
hvals key    返回存储在key中哈希表的所有value
hincrby key field increment  为哈希表key中的field的值加上指定的增量，并返回增量后的值（增量正数累加，增量负数递减）
hlen   查看 key 下存在多少个 field 属性
hmget 和 hmset
hscan key cursor [match pattern] [count count]   用于遍历哈希表中的键值对 
应用：存对象，购物车
```
hash 类型应用场景
 解决方案

 以客户id作为key，每位客户创建一个hash存储结构存储对应的购物车信息将商品编号作为field，购买数量作为value进行存储

 添加商品: 追加全新的field与value

 浏览:遍历hash

 更改数量:自增/自减，设置value值

 删除商品: 删除field

 清空:删除key

## List

```plain
以l开头，表示对列表的左边进行数据操作；以r开头，表示对列表的右边进行数据操作。
增加：左插，右插，指定集合元素前后插；
删除：头删，尾删，删除固定值，保留范围值其他删除；
修改：修改固定下标元素；  
查看：查看范围元素，查看指定下标元素，查看元素下标；
rpush key element [element …]  将一个或多个值插入到集合key的尾部（尾插法），key不存在则创建新的
rpushx key element [element …]   将一个或多个值插入到集合key的尾部（尾插法），key不存在无法插入
lpush key element [element …]  将一个或多个值插入到集合key的头部（头插法），key不存在则创建新的
lpushx key element [element …]  往集合左边插入一个元素；key不存在无法插入
linsert key before|after pivot element  把element元素插入到指定集合key里，但是还要以pivot内部的一个元素为基准，看是插到这个元素的左边还是右边，如果列表中有重复数据，系统会从左边开始寻找，找到的第一个目标数据的位置就停止查找，然后插入。
lpop key [count]   集合左边（头部）弹出（删除）指定的count个元素删除
rpop key [count]   集合右边（尾部部）弹出（删除）指定的count个元素删除
lrange key start stop   查询集合元素，并设置查询区间  start：起始值，设置正数则从左往右，设置负数则从右往左开始 stop：终点值，设置正数则从左往右，设置负数则从右往左开始  注：start（0） stop（-1）代表查询全部，参数代表的是下标
lindex key index   返回集合key里索引index位置存储的元素，0~n从左往右索引、-1~-n从右往左索引
lset key index element  设置集合key中index位置的元素值为新的element，index为正数则从头到位索引，为负数从尾到头索引查询
lrem key count element  从集合key中删除前count个值等于element的元素
   count > 0: 从头到尾删除值为 value 的元素；count < 0: 从尾到头删除值为 value 的元素；count = 0: 移除所有值为 value 的元素；
ltrim key start stop   保留start stop范围内的数据
lpos key element [rank rank] [count num-matches] [maxlen len]  返回集合key中匹配给定element成员的索引
    [rank rank]：选择匹配上的第几个元素，若超出集合指定元素的个数则返回(nil)
    [count num-matches]：返回匹配上元素的索引个数，默认返回1个
    [maxlen len]：告知lpos命令查询集合的前len个元素，限制查询个数
    lpos listString Romanti     -- 查询集合listString里的Romanti出现的索引位置（0开始索引）
    lpos listString Romanti rank 2  -- 查询Romanti元素的第二个索引位置
    lpos listString Romanti rank 1 count 3  -- 查询Romanti索引的三条记录
    lpos listString Romanti rank 1 count 3 maxlen 20    -- 限制查询下标为0~20
llen key 获取列表长度 
双向链表，可以双端进，双端出，可以分页，所有数据的结束操作索引是-1，实现栈模型（获取最新消息）
应用：
微博的关注列表或者文章列表（有顺序的）  lrange key start stop；微信朋友圈点赞，要求按照点赞顺序显示点赞好友信息；消息队列；
```
## Set

```plain
查看全部，是否存在，随机取出元素，交并差，删除指定元素
sadd key member [member …]  将一个或多个元素加入到集合中，添加已存在的集合元素将被忽略（不会添加上），返回添加成功的元素个数
srem key member [member …]  删除指定的元素；如果指定的元素不是集合成员则被忽略，返回被删除元素个数，不含不存在的元素
spop key [count] 从集合key中删除一个或多个随机元素，并返回删除的元素
scard  key 获取集合中元素的总数
srandmember key [count]   随机返回集合key中的一个或多个随机元素，若返回个数的count大于集合总数则返回全部
sinter key [key …]   返回第一个集合与其它集合之间的交集
sinterstore destination key [key …]   类似sinter，将结果保存到destination集合，返回成功添加到新集合上的个数
sunion key [key …]   用于返回所有给定集合的并集
sunionstore destination key [key …]  类似于sunion，返回存储在destination集合。
sdiff key [key …] 第一个集合的某个元素在其它集合都不存在则这个元素会被返回
sdiffstore destination key [key …] 将结果保存到destination集合，并返回成功添加到新集合上的个数。
smembers key   返回存储在key中的集合的所有的成员
sismember key member   判断元素member是否是集合key的成员，是返回1，否则0 


应用：随机推荐类信息检索；共同关注；
```
## Zset

```plain
不重复，相同数据会覆盖score值，通过分数属性实现有序，也兼set功能
查看范围元素，查看指定元素分数，查看元素排名，交并差，删除固定元素，删除范围元素
   指定元素增加/减少分数
zadd key [nx|xx]  [ch] [incr] score member [score member …]   用于将一个或多个member元素及其score值加入到有序集key当中，若添加的member已存在则更新当前分数  [ch]：返回添加和更新成功的个数
   [incr]：累加操作，score代表更新member的步长
zrem key member [member …]  用于从有序集合key中删除指定的多个成员member。如果member不存在则被忽略
zrank key member  返回有序集key中成员member的排名，其中有序集成员按score值从低到高排列  升序
zrevrank key member  返回有序集key中成员member的排名，其中有序集成员按score值从高到低排列  降序  
zscore key member  用于返回有序集和key中成员member的分数（不存在的元素返回nil）
zcard  key  返回元素的总个数
zcount key min max  返回有序集key中，score值在min和max之间(min<=score>=max)的成员的数量
zincrby key increment member     有序集key的成员member的score值加上增量increment
zrange key min max [byscore|bylex] [rev] [limit offset count] [withscores]
   返回有序集合，按照下标，分数，元素来获取指定范围
   注：默认下标查询，所有0 -1代表查询全部
[limit offset count]：筛选后的结果排序  offset：起始位（0下标开始数） count：查询元素个数
   [withscores]：最终查询结果显示分数，但是只适用于byscore查询和默认下标查询
   [rev]：设置倒序排列，注意写最大值和最小值要反过来写      
zrevrange key start stop [withscores]  成员的位置按score值递减(从高到低)来排列获取
   zrangebyscore key min max [withscores] [limit offset count]     
   zrevrangebyscore key max min [withscores] [limit offset count]  返回有序集合key中指定分数区间的成员列表。有序集成员按分数值递增(从小到大)次序排列获取  
zinter numkeys key [key …] [weights weight [weight …]] [aggregate sum|min|max] [withscores]
   计算numkeys个有序集合的交集（就是把几个相同元素的分数进行处理）
    numkeys：计算交集的key个数
    key | [key …]：设置要处理交集的有序集合，按照我们给出的numkeys写指定个数的key
    [weights weight [weight …]]：权重计算（乘法因子）；要设置权重的话，则有几个key就得写几个权重值
   若key1 key2 weights 10 15 说明：第一个key里面的全部分数要乘于10，第二个key的全部分数乘于15
   注：此属性在交集、并集计算中都存在，只要是符合[交集|并集]的才会计算并返回给客户端
   [aggregate sum|min|max]：你可以指定交集、并集的结果集的聚合方式 注：指定sum（默认）则交集的元素的分数结合，若指定max，则会选择最大的作为交集的分数
    [withscores]：显示分数
zunion numkeys key [key …] [weights weight [weight …]] [aggregate sum|min|max] [withscores]
   说明：计算给定的numkeys个有序集合的并集，并且返回结果 
应用：排行榜；延时队列
按条件获取数据
zrangebyscore key min max [WITHSCORES] [LIMIT)zrevzangebyscore key max min [WITHSCORES
条件删除数据
zremrangebyrank key start stop
zremrangebyscore key min max
注意:
min与max用于限定搜索查询的条件
start与stop用于限定查询范国，作用于索引，表示开始和结束索引
offset与count用于限定查询范围，作用于查询结果，表示开始位置和数据总量
获取集合数据总量
zcard key
zcount key min max
集合交、并操作
zinterstore destination numkeys key [key。。。
zunionstore destination numkeys key [key。。。
获取数据对应的索引(排名)
zrank key member
zrevrank key member
score值获取与修改
zscore key member
zinerby key increment member
```
## 应用场景

缓存：降低服务器压力和响应时间

共享session

HTTP协议是无状态的，这种无状态意味着程序需要验证每一次请求，从而辨别客户端的身份。在这之前，程序都是通过在服务端存储的登录信息来辨别请求的。这种方式一般都是通过存储Session来完成或者基于Token的身份验证

Redis解决集群session共享问题（多台服务器即tomcat不共享session，请求切换到其他服务器后导致数据丢失的问题）用户登陆后会在redis生成token为key，用户Dto的json

 字符串为键的有过期时间的键值对，这样就算负载均衡带其他服务器，只要携带token，并查询到该键值对则证明该用户有权限进入系统token登录，一般使用拦截器判断，或者gateway全局过滤器


缓存一致性问题

缓存和数据库双写⼀致性问题：先更新数据库，再删缓存，加超时时间自己删，未删除成功则

进行重试

缓存更新策略：

 读：缓存命中直接返回，未命中读数据库写缓存加超时时间

 写：先更新数据库再删除缓存，缓存加超时时间 保证他俩原子性，删除缓存失败加消息队列重试机制

缓存穿透

客户端不断请求在缓存和数据库都不存在的数据，缓存永远不生效，请求都回到到数据库，导致数据库崩溃

缓存null对象，设置ttl避免内存消耗；

缓存雪崩

同一个时刻出现大规模的缓存同时失效或者服务器宕机，导致数据库压力巨大，

搭建Redis集群，key的ttl添加随机数

缓存击穿

热点的Key，有大并发集中对其进行访问，Key突然失效并且重建缓存复杂，导致数据库压力剧增

互斥锁解决缓存击穿，缓存未命中则先尝试获得锁，获得锁再去数据库查


分布式锁：分布式多节点或集群模式下多进程可见并互斥的锁；

实现：redis的setnx key 实现互斥；zookeeper利用节点唯一性和有序性实现互斥

利用setnx获取锁，并设置过期时间，保存当前线程标识（存放在value），释放锁先获取当前线程标识，判断当前线程是否与获取锁的线程一致，一致则删除，服务宕机或设置超时时间实现自动释放锁，要保证判断动作与释放动作的原子性；

获取锁： SET lock_key unique_value NX EX 10000

key 资源的key键；value 客户端唯一标识；互斥，不存在才添加；设置避免客户端发生异常而无法释放锁。

释放锁

将lock_key键删除   

解锁操作分为两步：先判断锁的unique_value是否相同，是 再删除；保证解锁两个操作的原子性,通过Lua脚本释放锁：

分布式ID：

全局唯一ID，redis自增：时间戳+计数器


消息队列：

使用 list 结构作为队列， rpush 生产消息， lpop 消费消息。当 lpop 没有消息的时候，要适当sleep 一会再重试或者使用 blpop ，没有消息会阻塞住直到消息到来。

生产一次消费多次呢？使用 pub / sub 主题订阅者模式，可以实现 1:N的消息队列。


延迟队列：

指把当前要做的事情，往后推迟一段时间再做。

京东下单，超过一定时间未付款，订单会自动取消；

点外卖商家在10分钟还没接单，就会自动取消订单；


 zadd key score value 命令一直生产消息。score属性，用它来存储当前时间戳

启动一个消费者线程，使用zrangebyscore以0<score<=当前时间戳命令获取定时从zset中获取当前时间之前的所有消息，当有满足的条件的元素,先删除zrem该元素(保证不被其他进程取到)，再进行业务逻辑处理;Redis 的 zrem方法时多线程多进程争抢任务的关键，它的返回值决定了当前实例有没有抢到任务


秒杀：

 超卖问题

 乐观锁：更新数据  修改数据前判断数据是否被改过，通过where版本号字段=修改前的值

 悲观锁：插入数据  一人一单，操作数据前要先获得锁

 


## 线程安全问题

Redis 单线程指的是「接收客户端请求->解析请求 ->进行数据读写等操作->发送数据给客户端」这个过程是由一个线程（主线程）来完成的，但是，**Redis 程序并不是单线程的**，Redis 在启动的时候，是会**启动后台线程**（BIO）的：

Redis使用单线程和原子操作来保证数据的一致性和并发性，多个命令在并发中也是原子性的吗？不一定，用 Lua 脚本实现原子性操作

出现线程安全问题的场景：

并发下的库存操作 计数器操作

解决Redis的线程安全问题：

使用分布式锁来确保只有一个线程可以执行关键代码块

使用事务：通过将多个操作打包成一个事务，可以保证这些操作的原子性。

Redis6.0的多线程部分只是用来处理网络数据的读写和协议解析，执行命令仍然是单线程顺序执行，但是当多个Redis客户端同时执行多个指令的时候，就无法保证原子性。采用队列模式将并发访问变为串行访问 应用程序加锁synchronized或者lock；使用setnx实现锁，若给key 已存在，返回哦啥也不做，GETSET key value功能：将给定 key 的值设为 value ，并返回 key 的旧值(old value)；lua脚本执行

如何解决并发竞争问题

● 场景：

○ 多客户端同时并发写一个 key，可能本来应该先到的数据后到了，导致数据版本错了；或者是多客户端同时获取一个 key，修改值之后再写回去，只要顺序错了，数据就错了。

● 解决方案：

○ 使用分布式锁(基于zookeeper或者redis)   确保只有一个实例操作某个key

○ 同时采用CAS  比较数据的时间戳   若当前这个 value 的时间戳是否比缓存里的 value 的时间戳要更新 则可以写，否则，就不能用旧的数据覆盖新的数据。

## 事务

## Lua脚本

脚本可以编写多条redis命令，确保多命令执行的原子性，支持通过redistemplate调用

## 原理

Redis 基于内存，单线程的非关系型数据库，key-value数据库

为什么快？

Redis 的大部分操作都在内存中完成， 采用单线程模型可以避免了多线程之间的竞争，省去了多线程切换带来的时间和性能上的开销，而且也不会导致死锁问题。采用了 I/O 多路复用机制处理大量的客户端 Socket 请求。


## 数据删除策略   

过期删除策略   删除已过期的 key

 1，定时删除 （数据加过期时间）  2.惰性删除    3.定期删除

Redis 使用的过期删除策略是惰性删除+定期删除

定时删除策略：在设置 key 的过期时间时，同时创建一个定时事件，当时间到达时，由事件处理器自动执行key的删除操作

定期删除策略的做法是，每隔一段时间随机从数据库中取出一定数量的 key 进行检查，并删除其中的过期key

惰性删除策略的做法是，不主动删除过期键，每次从数据库访问 key 时，都检测 key 是否过期，如果过期则删除该 key

## 内存淘汰策略

Redis 运行内存达到了某个阀值，就会触发内存淘汰机制

阀值即设置的最大运行内存，Redis 的配置文件中，配置项为 maxmemory。

6个策略

不进行数据淘汰的策略：不再提供服务，直接返回错误

进行数据淘汰策略：

设置了过期时间的数据中进行淘汰

随机淘汰设置了过期时间的任意键值

* **lru**是按照数据的最近最少访问原则来淘汰数据，可能存在的问题是如果大批量冷数据最近被访问了一次，就会占用大量内存空间，如果缓存满了，部分热数据就会被淘汰掉。
* **lfu**是按照数据的最小访问频率访问次数原则来淘汰数据，如果两个数据的访问次数相同，则把访问时间较早的数据淘汰。
* 优先淘汰更早过期的键值

在所有数据范围内进行淘汰：

随机淘汰任意键值

* **lru**是按照数据的最近最少访问原则来淘汰数据，可能存在的问题是如果大批量冷数据最近被访问了一次，就会占用大量内存空间，如果缓存满了，部分热数据就会被淘汰掉。
* **lfu**是按照数据的最小访问频率访问次数原则来淘汰数据，如果两个数据的访问次数相同，则把访问时间较早的数据淘汰。
## 持久化机制

RDB：快照形式是直接把内存中的数据保存到⼀个 dump ⽂件中，定时保存，保存策略。（会丢数据）

AOF：把所有的对Redis的服务器进⾏修改的命令都存到⼀个⽂件⾥，命令的集合。（影响性能）

RDB持久化⽅式会在⼀个特定的间隔保存那个时间点的⼀个数据快照

Redis 默认会开启 RDB 快照，所以你的 Redis 重启下，之前的缓存数据就会被重新加载了

AOF 日志：注意只会记录写操作命令:Redis 每执行一条写操作命令，把命令写入到一个文件里

 appendonly yes  //表示是否开启AOF持久化(默认 no，关闭)

 appendfilename  “appendonly.aof”// AOF持久化文件的名称

RDB 快照就是记录某一个瞬间的内存数据，记录的是实际数据

Redis 提供了两个命令来生成 RDB 文件，分别是 save 和 bgsave

执行了 save 命令，就会在主线程生成 RDB 文件，由于和执行操作命令在同一个线程，所以如果写入 RDB 文件的时间太长，会阻塞主线程；

 执行了 bgsave 命令，会创建一个子进程来生成 RDB 文件，这样可以避免主线程的阻塞；

Redis 还可以通过配置文件的选项来实现每隔一段时间自动执行一次 bgsave 命令，默认会提供以下配置：

 save 900 1

 save 300 10

 save 60 10000

实际上执行的是 bgsave 命令，也就是会创建子进程来生成 RDB 快照文件。

只要满足上面条件的任意一个，就会执行 bgsave，它们的意思分别是：

900 秒之内，对数据库进行了至少 1 次修改；

 300 秒之内，对数据库进行了至少 10 次修改；

 60 秒之内，对数据库进行了至少 10000 次修改。

 这里提一点，Redis 的快照是全量快照，也就是说每次执行快照，都是把内存中的「所有数据」都记录到磁盘中。




## 主从复制

一主多从，读写分离，主服务器可以进行读写操作，当发生写操作时自动将写操作同步给从服务器，而从服务器一般是只读，并接受主服务器同步过来写操作命令，然后执行这条命令

主从服务器之间的命令复制是异步进行的，而是主服务器自己在本地执行完命令后，就会向客户端返回结果，并不会等从服务器实际执行完命令后再向客户端返回结果，无法保证强一致性

## 哨兵机制

监控通知    故障转移

监控主从服务器运行状态，如果发现主节点挂了，它就会选举一个从节点切换为主节点，并且把新主节点的相关信息通知给从节点和客户端

哨兵会每隔 1 秒给所有主从节点发送 PING 命令，当主从节点收到 PING 命令后，会发送一以判断它们是否在正常运行


## RedisTemplate


RedisTemplate中5种常见的OpsFor分别是:opsForValue、opsForList、opsForHash、opsForSet、OpsForZSet。


redis内部的五种数据类型：字符串、列表、集合、有序集合、散列表，但是键的类型只能为字符串。


