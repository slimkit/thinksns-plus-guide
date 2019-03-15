2019年3月15日11:37:46
# 关于聊天消息离线推送

## 使用环信

### 说明
- 环信仅支持 谷歌、华为、小米推送
- [在三方申请应用后记得到环信后台配置证书](http://console.easemob.com/app/app-list)
- [文档地址](http://docs-im.easemob.com/start/200androidcleintintegration/115payloadmsg)

### 推送的配置选项
- 强制推送
```
EMMessage message = EMMessage.createSendMessage(EMMessage.Type.TXT);
EMTextMessageBody txtBody = new EMTextMessageBody("test");
message.setTo("6006");
// 设置自定义扩展字段
message.setAttribute("em_force_notification", true);
// 设置消息回调
message.setMessageStatusCallback(new EMCallBack() {...});
// 发送消息
EMClient.getInstance().chatManager().sendMessage(message);
```
- 自定义推送提示
```
// 这里只是一 TXT 消息为例，IMAGE FILE 等类型的消息设置方法相同
EMMessage message = EMMessage.createSendMessage(EMMessage.Type.TXT);
EMTextMessageBody txtBody = new EMTextMessageBody("消息内容");
message.setTo("6006");
// 设置自定义推送提示
JSONObject extObject = new JSONObject();
try {
    extObject.put("em_push_name", "离线推送标题");
    extObject.put("em_push_content", "离线推送内容部分");
} catch (JSONException e) {
    e.printStackTrace();
}
// 将推送扩展设置到消息中
message.setAttribute("em_apns_ext", extObject);
// 设置消息回调
message.setMessageStatusCallback(new EMCallBack() {...});
// 发送消息
EMClient.getInstance().chatManager().sendMessage(message);
```

ps: 如不使用环信，请参照谷歌、华为、小米或者其他三方聊天SDK提供的文档

- [谷歌](https://firebase.google.com/)
- [华为](https://developer.huawei.com/consumer/cn/console#/serviceCards/AppService)
- [小米](http://admin.xmpush.xiaomi.com/zh_CN/app/nav)

## 使用环信概要说明
### 1 [谷歌](http://docs-im.easemob.com/im/200androidclientintegration/125fcmupgrade)
[文档地址](http://docs-im.easemob.com/im/200androidclientintegration/125fcmupgrade)
国内差不多用不上谷歌推送，如果您要使用谷歌推送，或者在使用谷歌推送的过程中修改了APP包名，请按照以下步骤操作。
1. 到[Firebase](https://console.firebase.google.com/u/0/)中创建项目，如果无需新建项目，则进行第二步。
2. 到您选择的项目中添加应用（一个项目可添加多个应用）,包名修改后请重新添加。
![image](https://user-images.githubusercontent.com/11835710/54403333-98c80080-470a-11e9-9bde-d2f917224474.png)
3. 进入引导页，如下图，点击按钮下载google-services.json文件到本地。替换掉 app module下的google-services.json文件。
![image](https://user-images.githubusercontent.com/11835710/54403386-cb71f900-470a-11e9-91da-d32e697f363b.png)
4. 跳过引导页，点击Cloud Messaging tab页，复制Server Key和Sender ID。
![image](https://user-images.githubusercontent.com/11835710/54403884-a2eafe80-470c-11e9-80a9-ccd1f30f7e82.png)
5. 登录[环信管理后台](http://console.easemob.com/app/app-list)，选择你的应用—新增推送证书，证书的名称要求填上方复制Sender ID，证书秘钥填写上方复制的Server Key。
![image](https://user-images.githubusercontent.com/11835710/54403921-c4e48100-470c-11e9-8959-e244df7df02b.png)
6. 在环信中启用谷歌推送，项目中 com.zhiyicx.thinksnsplus.modules.chat.call.TSEMHyphenate 类的 initOptions 方法中
```java
// 如果暂时不想使用谷歌推，则不配置此两项。
options.setUseFCM(true);
options.setFCMNumber(TSEMConstants.ML_GCM_NUMBER);
```
7. 如果您不使用谷歌推送，并且想清除谷歌推送相关三方库，操作如下:

- 删除项目根目录 build.gradle 中 classpath 'com.google.gms:google-services:4.2.0'
- 删除项目 app module 的 build.gradle 中 implementation dataDependences.playServicesGcm 和 implementation dataDependences.firebase
- 删除项目 baseproject module 的 build.gradle 中 api dataDependences.playServicesBase
- 删除项目 app module 下的 google-services.json文件。


### 2 [华为](http://docs-im.easemob.com/im/200androidclientintegration/115thirdpartypush)
[生成套件](https://developer.huawei.com/consumer/cn/service/hms/catalog/huaweipush_agent.html?page=hmssdk_huaweipush_sdkdownload_agent)

[文档地址](http://docs-im.easemob.com/im/200androidclientintegration/115thirdpartypush)

首先，向您的根级 build.gradle 文件添加规则：

```java
    allprojects {
                repositories {
                    jcenter()
                    maven {url 'http://developer.huawei.com/repo/'}
                }
            }
```
然后，在您的模块 Gradle 文件（通常是 app/build.gradle）中添加依赖
```java
    dependencies {
      // ...
      implementation 'com.huawei.android.hms:push:{version}'
      // 说明：{version} 替换为实际的版本号，如：implementation 'com.huawei.android.hms:push:2.6.3.301'
    }
```
你还需要一个接收服务
```java
   public class MyHuaWeiPushReceiver extends PushReceiver {
       @Override
       public void onToken(Context context, String token, Bundle bundle) {
           super.onToken(context, token, bundle);
           String manufacturer = Build.MANUFACTURER;
           if ((!TextUtils.isEmpty(manufacturer) && "HUAWEI".equals(manufacturer.toUpperCase())) || !TextUtils.isEmpty(DeviceUtils.getEMUI())) {
               EMClient.getInstance().sendHMSPushTokenToServer(ML_HUAWEI_APP_ID, token);
           }
       }

       @Override
       public boolean onPushMsg(Context context, byte[] bytes, Bundle bundle) {
           LogUtils.d("---------------------");
           LogUtils.d(bundle);
           return super.onPushMsg(context, bytes, bundle);
       }
   }
```

### 3 小米

- [配置好Manifest](http://www.easemob.com/question/12897?utm_source=edm&utm_ccampaign=102&email=[RECEIVER_ADDRESS])
- 环信初始化配置
- [也许你还需要最新的小米推送lib](http://admin.xmpush.xiaomi.com/mipush/downpage/)
```java
 options.setMipushConfig(TSEMConstants.ML_MI_APP_ID, TSEMConstants.ML_MI_APP_KEY);
```


