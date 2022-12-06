---

title: redis 面试题：redis 为什么执行速度这么快
date: '2022-12-6'
type: book
weight: 701
commentable: true
editable: true
tags:
  - redis
---

## 纯内存操作

1. 避免大量访问数据库，减少直接读取磁盘数据
2. redis 将数据储存在内存里面，读写数据的时候都不会受到硬盘 I/O 速度的限制。

## 单线程操作

1. 避免了不必要的上下文切换和竞争条件
2. 不存在多进程或者多线程导致的切换而消耗CPU
3. 不用去考虑各种锁的问题，不存在加锁释放锁操作，没有因为可能出现死锁而导致的性能消耗

## 采用了非阻塞I/O多路复用机制

采用多路 I/O 复用技术可以让单个线程高效的处理多个连接请求（尽量减少网络 IO 的时间消耗）。

多路I/O复用模型是利用 select、poll、epoll 可以同时监察多个流的 I/O 事件的能力。

在空闲的时候，会把当前线程阻塞掉，当有一个或多个流有 I/O 事件时，就从阻塞态中唤醒，于是程序就会轮询一遍所有的流（epoll 是只轮询那些真正发出了事件的流），并且只依次顺序的处理就绪的流，这种做法就避免了大量的无用操作，从而提高效率。

![redis-面试题-redis为什么这么快-多路复用.jpg](https://cnymw.github.io/GolangStudy/docs/img/redis-面试题-redis为什么这么快-多路复用.jpg)

## 灵活多样的数据结构

1. redis 内部使用一个 redisObject 对象来表示所有的 key 和 value。 
2. redisObject 主要的信息包括数据类型、编码方式、数据指针、虚拟内存等。它包含 String，Hash，List，Set，Sorted Set 五种数据类型，针对不同的场景使用对应的数据类型，减少内存使用的同时，节省网络流量传输。

## 持久化

由于 redis 的数据都存放在内存中，如果没有配置持久化，redis 重启后数据就全丢失了，于是需要开启 redis 的持久化功能，将数据保存到磁盘上，当 redis 重启后，可以从磁盘中恢复数据。

redis 提供两种方式进行持久化，一种是 RDB 持久化，另外一种是AOF（append only file）持久化。持久化似乎和 redis 的速度快并没有直接关系，但是这保证的 redis 数据的安全性和可靠性，也起到数据备份的作用。

## 思维导图

![redis-面试题-redis为什么这么快.png](https://cnymw.github.io/GolangStudy/docs/redis-面试题-redis为什么这么快/redis-面试题-redis为什么这么快.png)
