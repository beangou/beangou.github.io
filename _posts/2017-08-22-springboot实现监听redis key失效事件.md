---
layout: default
title: springboot实现监听redis key失效事件
---

### 一、需求说明：
大家在使用redis的时候，可能需要在redis key失效的时候，做一些事情，比如统计方面（我的上一篇文章就用到：[统计当前在线用户数、平均访问时长、在线用户最高数等实现](https://my.oschina.net/beanGo/blog/1507424)），因为查找资料都讲的不是很全，或者有问题，所以，在此做一个详细点的说明。

### 二、详解：
   * 首先，修改redis相关事件配置。找到redis配置文件redis.conf，查看“notify-keyspace-events”的配置项，如果没有，添加“notify-keyspace-events Ex”，如果有值，添加Ex，相关参数说明如下：![输入图片说明](https://static.oschina.net/uploads/img/201708/21002711_Ox7N.png "在这里输入图片标题")

   * 修改后，重启redis，看是否凑效。使用redis-cli连接redis，输入“psubscribe __keyevent@0__:expired”，该命令用于监听key失效的事件；然后另外打开一个窗        口，使用redis-cli连接redis，使用setex设置一个key，以及失效时间，看第一个窗口是否能监听到这个key的失效事件，如果能监听到，可以看到这样的日志打印出来。如果出现，则说明，redis配置成功，截图如下：
![输入图片说明](https://static.oschina.net/uploads/img/201708/21003243_JFMX.png "在这里输入图片标题")
![输入图片说明](https://static.oschina.net/uploads/img/201708/21003305_jqqG.png "在这里输入图片标题")

* 使用java代码监听key的失效事件。我使用的是springboot框架，只需要引入如下依赖即可：
  ```
  <dependency>
     <groupId>org.springframework.boot</groupId>
     <artifactId>spring-boot-starter-data-redis</artifactId>
     <version>1.4.0.RELEASE</version>
  </dependency>
  ```
* 直接贴上代码，主要是覆盖类KeyExpirationEventMessageListener的onMessage方法即可，在onMessage方法，大家可以做自己的业务处理。
```
	public class RedisKeyExpirationListener extends KeyExpirationEventMessageListener {
	public RedisKeyExpirationListener(RedisMessageListenerContainer listenerContainer) {
        super(listenerContainer);
    }

    /**
     * 针对redis数据失效事件，进行数据处理
     * @param message
     * @param pattern
     */
    @Override
    public void onMessage(Message message, byte[] pattern) {
        // 用户做自己的业务处理即可,注意message.toString()可以获取失效的key
        String expiredKey = message.toString();
    }
}
	```
