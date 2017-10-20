2017年5月11日 13:54:57
# IMSDK

`imsdk`是一个基于`websocket`协议封装的及时聊天SDK

## 代码结构图
```java
|--imsdk
   |--builder               // 构造器
   |--core                  // websocket核心
   |--db                    // 本地数据库
   |--entity                // 数据模型
   |--manage
      |--listener               // 回调接口，包括im状态、im消息、超时等
      |--soupport               // 对外暴露的接口
      |--ChatClent.java         // 单聊模块
      |--ChatRoomClient.java    // 聊天室模块
      |--ZBIMClient.java        // 聊天中继器，用于消息是上传与分发
      |--ZBIMSDK.java           // SDK入口
   |--policy                // 策略
      |--timeout                // 超时
   |--receiver              // 广播接收器
   |--service               // 服务
   |--utils                 // 工具包

```

## 技术说明
具体通信协议请查看[文档](http://www.kancloud.cn/xiew/webim/192522),如有权限问题，请联系文档管理员 **孙永鸿**

>- websocket选型
基于对`Android`的友好支持，底层`websocket`通信选用 [autobahn](http://autobahn.ws/android/) 搭建`websocket`连接
- 数据格式支持
a.普通文本（text）
b.二进制（binary）
- 序列化支持
b.`Json`
c.[msgpack](http://msgpack.org/)
- 数据压缩支持
a.`deflate`
b.`zlib`
c.`gzip`
- 数据库支持
结构功能单一，所以选择基于`SQLiteOpenHelper`自行封装

### 心跳机制

- 每隔一段时间给ping一下服务器，保证连接正常
- 每重连一次，重连间隔时间-10s，增加网络环境差的保活率
- 根据软件在前后台调节心跳速率
- 根据当前网络情况调整网络速率

### 重连机制
- ping后或者发送普通消息 HEART_PING_PONG_RATE ms收不到回应则重连
- 网络切换重连
- 服务器意外的失去了先前建立的连接重连
- 手动重连

### 超时处理
通过`TimeOutTaskManager`管理超时任务，`TimeOutTaskPool`处理超时任务，超时任务通过`Message`的`id`作为唯一标识来区别

### 消息逻辑

- `ImService`
用户和`websocket`服务器通信，发送和接收消息
- `SockectService`
1、用于连接`ImService`和`ZBIMClient`进行消息传递
2、处理消息：消息解析、消息去重、消息缓存、消息超时
3、维持心跳
4、处理重连
- `ZBIMClient`
消息中继器，用于上传和分发消息；例如：当消息从`websocket`服务器发送回来，首先是IMService拿到消息，给到SocketSercvice进行消息处理，再通过EventBus传递给ZBIMClient，ZBIMClient根据当前注册了监听的场景进行分发。例如：分发到单聊（ChatClient）、聊天室（ChatroomClient）等，

## 使用说明

使用说明请阅读[MANUAL](MANUAL.md)