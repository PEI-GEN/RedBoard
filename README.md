# 项目概述

### 介绍
该项目是一个社交平台项目。前端基于nginx，后端基于SpringBoot、Redis、MySQL和Mybatis-Plus。具备可以让学生在该平台上发帖、评论、交流等功能。
### 技术栈
`SpringBoot` `MySQL` `Redis` `Nginx` `Docker`
### 项目实现
#### 短信登录
#### 1.登录验证
#### 2.拦截器
- 使用AOP切面功能来实现
- 使用Spring的拦截器相关接口来自定义拦截器   
  - 实现**WebMvcConfigurer**接口，重写addCorsMappings()方法和addInterceptors()方法【配置拦截器】
  - 实现**HandlerInterceptor**接口或者继承**HandlerInterceptorAdapter**，重写preHandle()方法【自定义拦截器】
### 查询缓存
涉及到企业的缓存使用技巧，缓存雪崩，穿透等问题
#### 1.String缓存实现
#### 2.List缓存实现
#### 3.Zset缓存实现
#### 4.缓存主动更新策略
- 这里需要了解**数据库与缓存一致性**
#### 5.缓存穿透
布隆过滤器
#### 6.缓存雪崩
随机过期时间
#### 7.缓存击穿
互斥锁
### 达人探店
#### 1.基于Set实现点赞列表
使用到redis的set集合，key为blog的id，value为user的id。
每个key代表每条博客，每个key下的value集合代表所有点赞的用户id集合。
用set的ismembet方法判断，当前集合是否有userId，来判读该博客，用户是否已经点赞过了。
#### 2.基于SortedSet实现点赞排行榜
### 好友关注
基于Set集合的关注，取关，共同关注，消息推送等功能
```java
		//求交集
        Set<String> intersect = stringRedisTemplate.opsForSet().intersect(key1, key2);
```
### 优惠卷秒杀

使用分布式锁

#### 乐观锁解决优惠卷超卖问题
#### redisson分布式锁
#### 附近商户
GEO数据结构的用法
#### 用户签到
BitMap数据结构的使用
#### UV统计

### 我的实现
  1. 使用 Redis 实现**分布式 Session**，解决集群间登录态同步问题；使用 Redis 对高频访问店铺进行缓存，降低 DB 压力同时提升 90% 的数据查询性能。
  2. 使用**常量类全局管理** Redis Key 前缀、TTL 等，保证了键空间的业务隔离，减少冲突。
  3. 解决使用Redis作为缓存时遇到**缓存雪崩**、**缓存击穿**、**缓存穿透**问题，并解决缓存与数据库的**双写一致性**问题。
  4. 针对平台可以投放优惠卷的功能，负责实现优惠卷秒杀功能，并且使用**Redisson分布式锁**解决优惠卷超卖问题。
  5. 使用**Geo + Hash**数据结构分类存储附近商户，并使用 Geo Search 命令实现高性能商户查询及按距离排序。
  6. 使用**List**数据结构存储用户点赞信息，并基于 **Zset** 实现 TopN 点赞排行。
  7. 使用**Bitmap**实现用户连续签到统计功能，相对于传统关系库存储，大幅降低了使用内存。
  8. 在系统用户量不大的前提下，基于推模式实现关注 **Feed 流**，保证了新点评消息的及时可达，并减少用户访问的等待时间。
