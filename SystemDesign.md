# 系统设计 - labuladong
## Question examples
- Design a system
  - 秒杀系统, 微博系统，抢红包系统，短网址系统
- System functions
  - bilibili点赞功能
- Design framework
  - RPC，message queue, buffer framework, distributed file systems
- Choose technology option
  - Cache using [Redis vs Memcached](https://stackoverflow.com/questions/10558465/memcached-vs-redis)
  - Spring cloud gateway(rate limiting, custom filters) vs Netflix Zuul2（Zuul1 blocking APIs, no longlive conn like websockets)
  
想好再说，communication process not right/wrong, use stable nnot new techs, optimize step by step, tradeoff，simple concise
## Step 1 问清楚具体要求 core Functional vs non-functional requirement(queries/sec)
## Step 2: High level抽象设计
## Step 3: what needs to be improved?
- On 1 server？Need among several servers and do load balancer?
- DB process speed? Indexing? Read/Write separation? Cache?
- Multiple DB tables? Distributed file system? 
- Security concerns? upgrade hardware? async on long jobs? 
- SQL improvements? deadlock? reasonable and imptimized indexes? 当前系统的 SQL 语句是否存在问题？
## Step 4: Improve/Optimize Step 3.

## 知识储备
- 高性能架构设计：熟悉系统常见性能优化手段比如引入 读写分离、缓存、负载均衡、异步 等等。
- 高可用架构设计 ：CAP 理论和 BASE 理论、通过集群来提高系统整体稳定性、超时和重试机制、应对接口级故障：降级、熔断、限流、排队。
- 高扩展架构设计 ：说白了就是懂得如何拆分系统。你按照不同的思路来拆分软件系统，就会得到不同的架构。

## Indexes
- response time; 并发数 how many system can handle at the same time; QPS (query/sec) and TPS (transaction/sec)
- 吞吐量 QPS（TPS） = 并发数/平均响应时间(RT); 并发数 = QPS * 平均响应时间(RT)

## 系统活跃度
- page view, unique visitor, daily active user, monthly active user
```
举例：某网站 DAU 为 1200w， 用户日均使用时长 1 小时，RT 为 0.5s，求并发量和 QPS。
平均并发量 = DAU（1200w）* 日均使用时长（1 小时，3600 秒） /一天的秒数（86400）=1200w/24 = 50w
真实并发量（考虑到某些时间段使用人数比较少） = DAU（1200w）* 日均使用时长（1 小时，3600 秒） /一天的秒数-访问量比较小的时间段假设为 8 小时（57600）=1200w/16 = 75w
峰值并发量 = 平均并发量 * 6 = 300w
QPS = 真实并发量/RT = 75W/0.5=100w/s
```
Frontend tols: fiddler, httpwatch
Backend tools: Jmeter, LoadRunner, Galting, apache Bench
