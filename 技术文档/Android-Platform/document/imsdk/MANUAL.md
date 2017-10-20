2017年5月11日 13:55:01
# IMSDK文档

## 大纲
>- [IMClient介绍](#IMClient介绍)
>    1. [IM登录](#IM登录)
>    2. [IM登出](#IM登出)
>    3. [设置IM监听器](#设置IM监听器)
>        3.1. [消息监听器](#消息监听器)
>        3.2. [IM连接状态监听器](#-IM连接状态监听器)
>        3.3. [IM连接状态监听器](#-IM连接状态监听器)
>- [MessageBuilder介绍](#MessageBuilder介绍)
>- [ChatRoomClient介绍](#ChatRoomClient介绍)
>- [Message、IMConfig等数据介绍](#Message、IMConfig等数据介绍)
>- [错误码](#错误码)

---

## IMClient介绍

### 1 IM登录
`ZBIMClient.getInstance().login(IMConfig);`[IMConfig详情查看](#Message、IMConfig等数据介绍)
```java
    /**
     * 登录Im服务
     */
    private void ImLogin(int im_uid,String token) {
        IMConfig imConfig=new IMConfig();
        imConfig.setImUid(im_uid);
        imConfig.setToken(im_psw);
        //imConfig.serial(ImService.BIN_MSGPACK);设置服务器返回的数据类型 当前提供msgpack以及Json
        //imConfig.comprs(2);//:0不支持,1:deflate,2:zlib,3:gzip
        //imConfig.setWeb_socket_authority("ws://host:port")
        ZBIMClient.getInstance().login(imConfig);
    }
```
### 2 IM登出
账号切换时需要先登出后再是用新的im_uid和im_psw进行登录
```java
   ZBIMClient.getInstance().loginOut();
```

### 3 设置IM监听器

#### 3.1 消息监听器
     3.1.1 消息内容
     3.1.2 消息应答
     3.1.3 进入对话应答
     3.1.4 离开对话应答
     3.1.5 查看对话信息消息应答

设置监听器并在你不需要的时候解除监听

```java
//消息接收器
ZBIMClient.getInstance().addImMsgReceveListener(imMsgRecevelistener);

ImMsgReceveListener imMsgRecevelistener= new ImMsgReceveListener() {
            /**
            *收到消息内容
            */
            @Override
            public void onMessageReceived(Message message) {

            }
            /**
            *消息应答
            */
            @Override
            public void onMessageACKReceived(Message message) {

            }
            /**
            *进入对话应答
            */
            @Override
            public void onConversationJoinACKReceived(ChatRoomContainer chatRoomContainer) {

            }
            /**
            *离开对话应答
            */
            @Override
            public void onConversationLeaveACKReceived(ChatRoomContainer chatRoomContainer) {

            }
            /**
            *查看对话信息消息应答
            */
            @Override
            public void onConversationMCACKReceived(List<Conversation> conversations) {

            }
        });
//解除监听
ZBIMClient.getInstance().removeImMsgReceveListener(imMsgRecevelistener);
```
#### 3.2 IM连接状态监听器

     3.2.1 IM连接成功
     3.2.2 IM断开连接
     3.2.3 IM连接错误

设置监听器并在你不需要的时候解除监听

```java
//IM状态监听
ZBIMClient.getInstance().setImStatusListener(imStatusListener);
ImStatusListener imStatusListener=new ImStatusListener() {
            /**
            *IM连接成功通知
            */
            @Override
            public void onConnected() {

            }
            /**
            *IM断开连接通知
            */
            @Override
            public void onDisconnect(int code, String reason) {

            }
            /**
            *IM连接错误通知
            */
            @Override
            public void onError(Exception error) {

            }
        });
//解除监听
ZBIMClient.getInstance().removeImMsgReceveListener(imMsgRecevelistener);
```
#### 3.3 TimeOut消息监听

     3.3.1 发送消息超时，针对非实时消息（rt=false）
     3.3.2 加入对话超时
     3.3.3 离开对话超时
     3.3.4 查询对话信息超时

设置监听器并在你不需要的时候解除监听

```java
//超时监听
ZBIMClient.getInstance().setImTimeoutListener(imTimeoutListener);
ImTimeoutListener imTimeoutListener=new ImTimeoutListener() {
            /**
            *发送消息超时，针对非实时消息（rt=false）
            */
            @Override
            public void onMessageTimeout(Message message) {

            }
            /**
            *加入对话超时
            */
            @Override
            public void onConversationJoinTimeout(int roomId) {

            }
            /**
            *离开对话超时
            */
            @Override
            public void onConversationLeaveTimeout(int roomId) {

            }
            /**
            *查询对话信息超时
            */
              @Override
            public void  onConversationMcTimeout(List<Integer> roomIds) {

            }

        });
//解除监听
ZBIMClient.getInstance().removeImMsgReceveListener(imMsgRecevelistener);
```

#### 3.4 消息事件
##### 3.4.1 加入对话
roomId对话为id,pwd为对话密钥，用于超时的唯一标识
```java
   ZBIMClient.getInstance().joinConversation(int roomId, String pwd,int msgid);
```
##### 3.4.2 离开对话
```java
   ZBIMClient.getInstance().leaveConversation(int roomId, String pwd,int msgid);
```
##### 3.4.3 查看对话信息
roomIds为对话id列表，field查询对话字段，默认为全部
```java
   ZBIMClient.getInstance().mc(List<Integer> roomIds, String field,int msgid);
```
##### 3.4.4 发送文本消息
text文本内容，cid对话id
```java
   ZBIMClient.getInstance().sendTextMsg(String text, int cid,int msgid);
```
##### 3.4.4 发送自定义消息
message类型为[Message](#Message介绍)
```java
   ZBIMClient.getInstance().sendMessage(message);
```
---

## MessageBuilder介绍
简单使用方法，更多查看源码
```
   /**
     * 创建文本消息
     * @param msgid     消息id，自定义，用于消息超时标识
     * @param cid       对话id
     * @param ZBUSID    发送者的usid
      * @param uids     发送到对话中指定的uid，仅rt为true时有效，默认全部用户
     * @param text      内容
     * @param rt        是否是实时消息
     * @return  Message
     */
   MessageBuilder.createTextMessage(int msgid,int cid, String ZBUSID, List<Integer> uids, String text, boolean rt) ;

     /**
     * 完全自定义消息
     * @param msgid     消息id，自定义，用于消息超时标识
     * @param cid       对话id
     * @param uids      发送到对话中指定的uid，仅rt为true时有效，默认全部用户
     * @param rt        是否是实时消息
     * @param text      内容
     * @param ZBUSID    发送者的usid
     * @param customID  自定义消息的type类型，标识是什么自定义消息
     * @param ext       自定义消息的内容
     * @param isEnable  在被禁言后，这条消息是否可以发送
     * @return       Message

     */
   MessageBuilder.createMessage(int msgid,int cid, List<Integer> uids,boolean rt, String text, String ZBUSID, int customID, Map ext,boolean isEnable)

```

---


## ChatRoomClient介绍
##### 1. 通过聊天室id，使用者的usid，初始化ChatRoomClient
```java
ChatRoomClient client=new ChatRoomClient(int roomId, String ZBUSID, Context mContext) ;
```
##### 2. 进入聊天室
```java
client.joinRoom();
```
##### 3. 离开聊天室
```java
client.leaveRoom();
```
##### 4. 查看房间信息
```java
client.mc();
```
##### 5. 发送文本消息
```java
client.sendTextMsg(text);
```
##### 6. 发送礼物消息
```java
client.sendGiftMessage(Map jsonstr);
```
##### 7. 点赞，value查看[value规定说明](#value规定说明)
```java
client.sendZan(int value);;
```
##### 8. 发送关注主播
```java
client.sendAttention();
```
##### 9. 发送自定义消息
```java
  /**
     * 发送自定义消息
     * @param isEnable  被禁言时，会不会屏蔽，true为可用，不屏蔽
     * @param jsonstr   可自定义JavaBean，自己对应解析，返回的Message中的Map为键值对存在
     * @param customId  自定义消息类型
     */
client.sendMessage(boolean isEnable, Map jsonstr, int customId);
```
##### 10. 自己加入聊天室通知其他人
```java
client.sendJoinRoomMessage();
```
##### 11. 机器人加入聊天室通知其他人（机器人为模拟真实用户数据）
```java
client.sendRobotJoinRoomMessage(String robotUsid);
```
##### 12. 自己离开聊天室通知其他人
```java
client.sendLeaveRoomMessage();
```
##### 13. 聊天室人数 浏览量 统计（主播统一统计）
```java
client.sendDataCountMessage(ChatRoomDataCount custom);
```
<font color="red">注意：</font>在不需要ChatRoomClient时，调用onDestroy()
```java
client.onDestroy();
```
##### 14. 监听器

```java

client.setImStatusListener(imStatusListener);
client.setImMsgReceveListener(imMsgReceveListener);
/**
*IM状态监听
*/
public interface ImStatusListener {
    void onConnected();
    void onDisconnect(int code, String reason);
    void onError(Exception error);

}
/**
*消息监听
*/
public interface ImMsgReceveListener {
    /**
     * 收到消息
     * @param message
     */
    void onMessageReceived(Message message);
    /**
     * 发送消息回调
     * @param message
     */
    void onMessageACKReceived(Message message);
    /**
     * 加入聊天室回调
     * @param chatRoomContainer
     */
    void onConversationJoinACKReceived(ChatRoomContainer chatRoomContainer);
    /**
     * 查询离开聊天室回调
     * @param chatRoomContainer
     */
    void onConversationLeaveACKReceived(ChatRoomContainer chatRoomContainer);
    /**
     * 查询聊天室人数回调
     * @param conversations
     */
    void onConversationMCACKReceived(List<Conversation> conversations);
}
/**
*超时监听
*/
public interface ImTimeoutListener {
    /**
     * 发送消息超时
     * @param message
     */
    void onMessageTimeout(Message message);
    /**
     * 加入对话超时
     * @param roomId
     */
    void onConversationJoinTimeout(int roomId);
    /**
     * 离开对话超时
     * @param roomId
     */
    void onConversationLeaveTimeout(int roomId);
    /**
     * 查询对话人数超时
     * @param roomIds
     */
    void onConversationMcTimeout(List<Integer> roomIds);
}
```

---
## Message、IMConfig等数据介绍

### 1. Message介绍
```java
public class Message implements Serializable {
    public int uid;
    public int cid;
    public List<Integer> to;
    public int id;
    public int type;
    public String txt;
    public MessageExt ext;
    public boolean rt;
    public int err;
    public long expire = -1;
    public long mid;
    public long create_time;
    public boolean is_del;
    public boolean is_read;
    public int seq;
}
```
Message属性说明

| 名字 | 类型 | 必须 | 说明 |
| :----: | :----: | :----: |:----: |
| uid  | int | 否 | 消息发送者的im_uid;发送者不需要填写 |
| cid  | int | 是 | 消息要发送到的会话ID  |
| to  | List<Integer>| 否 |  指定消息的具体接收者uid(int类型)；如果没有此项，消息将发送给会话中所有成员，如果是系统会话消息并且没有指定uid列表，将发往全部已注册用户。**注意：**因私有会话成员只有两个（自己和对方），所以不支持此参数  |
|id  | int | 否 |  每一条消息对应的单独的id  |
|type  | int| 否|消息类型,详情查看[type规定说明](#type规定说明)|
|text  | String| 否|消息的文本内容|
|ext  | MessageExt| 否|自定义消息|
|rt  | boolean| 否|是否是实时消息，如果有此项且值为true时为实时消息，否则为普通消息|
|err  | int| 否|错误码|
|expire  | long| 否|0为永久禁言，大于0则为解禁时间戳|
|mid  | long| 否|mid 服务端消息ID。mid >> 23 + 1451577600000 = 消息的毫秒时间戳|
|create_time  | long| 否|消息才创建时间|
|is_del  | boolean| 否|消息是否删除，本地删除标识（假删除）|
|is_read  | boolean| 否|消息是否被查看了|
|seq  | int| 否|消息序号，实时消息没有，用户消息必答|





MessageExt属性说明
```java
public class MessageExt implements Serializable {
    public String ZBUSID;
    public int customID;
    public Map custom;
    }
```

|  值  |  类型  | 是否必须| 中文描述 |
| :----: | :----: | :----: | :----: |
| ZBUSID  | String| 否 | 消息发送者的usid |
| customID  | int| 是 | 自定义消息type,用户区分自定消息类型 |
| custom  | Map| 否| 自定义消息内容,以键值对的形式存在|





type规定说明：

|  值  |  类型  |中文描述|
| :----: | :----: | :----: |
| 0  | int  | 文本消息|
| 1  | int  | 图片类型消息|
| 2  | int  | 声音类型消息|
| 3  | int  | 视频类型消息|
| 4  | int  | 位置类型消息|
| 5  | int  | 通知类型消息|
| 6  | int  | 文件类型消息|
| 100  | int  | 用户自定义消息，被禁言时会被屏蔽|
| 210  | int  | 用户自定义消息，被禁言时不会被屏蔽|




<font color="red">注意：</font>当用户被主播禁言后，0-127的消息是会被屏蔽掉


  customId说明：

|  值  |  类型  | 中文描述 |
| :----: | :----: | :----: |
| 50000 - 59999 | int | 聊天室相关的 |
| 50100 | int | 点赞的消息 |
| 50200 | int | IM礼物消息 |
| 50300 | int | 有人进入聊天室消息 |
| 50400 | int | 有人离开聊天室消息 |
| 50500 | int | IM关注消息 |
| 50600 | int | 聊天室人数 浏览量 统计消息 |
| 50700 | int | 主播结束消息 |
| 60000 - 69999 | int | 私聊相关的 |
| 70000 - 79999 | int | 群聊相关的 |
| 0000 - 9999 | int | 给SDK的用户自定义消息时,必须设置 CUSTOM_ID = 0000 - 9999 |




点赞value规定说明：

|  值  |  类型  | 中文描述 |
| :----: | :----: | :----: |
|200|int|0xfee617色值的桃心|
|201|int|0x54b98e色值的桃心|
|202|int|0x47b5da色值的桃心|
|203|int|0xfb667d色值的桃心|



### 2. IMConfig介绍
```java
public class IMConfig implements Serializable {
    private int imUid;
    private String token;
    private int serial = ImService.BIN_MSGPACK;
    private int comprs = ImService.COMPRS_ZLIB;//
    private String ;
}
```
IMConfig属性说明

|  名字  | 类型  |	说明|
| :----: | :----: | :----: |
| imUid | int  | IM通信的id |
| token | String| IM通信的密码 |
| serial  | int  | 希望服务器返回的数据类型是json，还是msgpck,默认为msgpack  |
| comprs | int| 客户端支持的压缩方式:不支持,deflate,zlib,gzip，默认zlib压缩 |
| web_socket_authority | String|im服务器的地址("ws://host:prot")|





---

## 错误码
|  错误代码  | 错误代码类别  |	详细描述|
| :----: | :----: |:----:|
| 1000  | server err | 服务器发生了未知的异常，导致处理中断|
| 1010  | application err | 无效的数据包|
| 1011  | application err | 无效的数据包类型|
| 1012  | application err | 无效的消息主体|
| 1013  | application err | 	无效的序列化类型|
| 1020  | normal err | 未提供认证需要的uid和pwd|
| 1021  | normal err | 认证失败，可能原因是uid不存在或密码错误，或账号已被禁用|
| 2003  | normal err | 聊天室成员查询失败|
| 2001  | normal err | 聊天室加入失败|
| 2002  | normal err | 聊天室离开失败|


