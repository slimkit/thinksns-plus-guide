# 概述

配置、集成环信SDK以及使用环信EaseUI.

## 配置和集成环信SDK

如果不使用环信的UI框架，可以直接使用gradle导入SDK

#### 1、首先在你的项目根目录build.gradle文件的allprojects→repositories属性下加入远程库地址
```
maven { url "https://raw.githubusercontent.com/HyphenateInc/Hyphenate-SDK-Android/master/repository" }
```
#### 2、然后在你的module的build.gradle里加入以下代码
```
android {
    //use legacy for android 6.0
    useLibrary 'org.apache.http.legacy'
}
dependencies {
    compile 'com.android.support:appcompat-v7:23.4.0'
    //Optional compile for GCM (Google Cloud Messaging).
    compile 'com.google.android.gms:play-services-gcm:9.4.0'
    compile 'com.hyphenate:hyphenate-sdk:3.3.0'
}
```
#### 3、配置环信key
build.gradle中
```
manifestPlaceholders = [
                ......
                EASEMOB_APPKEY: "your key", // 环信三方配置
        ]
##
```
#### 4、配置so文件
如果你的项目中有其他三方库，请配合其他三方调整
```
ndk {
            //选择要添加的对应cpu类型的.so库。
            abiFilters 'armeabi', 'x86', 'armeabi-v7a', 'armeabi-v8a', 'x86_64', 'mips', 'mips64'
        }
```

## 集成环信UI框架-EaseUI
#### 1、导入EaseUI框架
以module的方式导入：File→New→Import Module→选择或输入 EaseUI 库路径→Next→Next→Finish

**备注:**[EaseUI Github 仓库](https://github.com/easemob/easeui)

**备注:**[EaseUI官方使用文档](http://docs.easemob.com/im/200androidclientintegration/135easeuiuseguide)

#### 2、修改环信的UI
核心类：EaseChatRow、EaseChatRowPresenter

修改方法：继承复写相关方法，如修改文字消息:
```
public class ChatRowText extends ChatBaseRow {

    @Override
    protected void onInflateView() {
        // 使用自己的布局文件
        inflater.inflate(message.direct() == EMMessage.Direct.SEND ?
                R.layout.item_chat_list_send_text : R.layout.item_chat_list_receive_text, this);
    }

    @Override
    protected void onSetUpView() {
        // 此方法用于展示内容等等
        super.onSetUpView();
        EMTextMessageBody txtBody = (EMTextMessageBody) message.getBody();
        // 正文
        Spannable span = EaseSmileUtils.getSmiledText(context, txtBody.getMessage());
        mTvChatContent.setText(span, TextView.BufferType.SPANNABLE);
    }

    @Override
    protected void onViewUpdate(EMMessage msg) {
        // 此方法用于更新布局，如发送失败重新发送后等等
    }
}

// easeui 本身提供了文字图片语音定位等等的presenter，继承的时候可以直接对应继承
// 其余点击item等方法可以参考EaseChatRowPresenter
public class TSChatTextPresenter extends EaseChatTextPresenter {

    @Override
    protected EaseChatRow onCreateChatRow(Context cxt, EMMessage message, int position, BaseAdapter adapter, ChatUserInfoBean userInfoBean) {
        return new ChatRowText(cxt, message, position, adapter, userInfoBean);
    }
}
```

**备注:** ChatBaseRow继承自EaseChatRow，用于处理公用的用户时间等信息

## 常见问题和注意事项（建议）

1、TS+采用服务器注册环信用户，因此在登陆后获取环信用户信息的接口有可能失败，需要在后台获取，成功后登陆环信。

2、环信的ui框架并没有适配7.0，需要在自己的项目中根据实际情况处理

3、注册环信建议使用id，用户信息环信本身并没有提供，如果需要使用可以修改EaseUI，加入对应的用户bean再从外部传入。
