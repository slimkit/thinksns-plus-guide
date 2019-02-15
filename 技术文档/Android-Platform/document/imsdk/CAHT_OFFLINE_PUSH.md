2019年2月15日11:11:41
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


首先，向您的根级 build.gradle 文件添加规则，以纳入 google-services 插件和 Google 的 Maven 代码库：

```java
    buildscript {
        // ...
        dependencies {
            // ...
            classpath 'com.google.gms:google-services:4.1.0' // google-services plugin
        }
    }

    allprojects {
        // ...
        repositories {
            // ...
            maven { url 'https://maven.google.com' }
            google() // Google's Maven repository
        }
    }
```
然后，在您的模块 Gradle 文件（通常是 app/build.gradle）中，在文件的底部添加 apply plugin 代码行，以启用 Gradle 插件：
```java
    apply plugin: 'com.android.application'

    android {
      // ...
    }

    dependencies {
      // ...
      implementation 'com.google.firebase:firebase-core:16.0.4'

      // Getting a "Could not find" error? Make sure you have
      // added the Google maven respository to your root build.gradle
    }

    // ADD THIS AT THE BOTTOM
    apply plugin: 'com.google.gms.google-services'
```
最后在环信初始化中配置
```java
    options.setUseFCM(true);
    options.setFCMNumber(TSEMConstants.ML_GCM_NUMBER);
```
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


