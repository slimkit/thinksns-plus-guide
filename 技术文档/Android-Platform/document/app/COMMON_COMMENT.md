# 评论公用模块实现
#### 一. 概述：
评论模块实现了常见评论的通用操作，例如发送、删除评论接口操作，以及与之相应的数据库操作。代码整体遵循依赖倒置以及里氏替换等等设计原则，扩展性挺高的QAQ。
#### 二. 定义：
标签: 评论
#### 三.使用 ：
#####1.构建：
* UML 图：

![此处输入图片的描述][1]

* builder构造：
```java
private CommentCore_ CommentCore_() {
    return new CommentCore_.Builder()
            .buildCommentEvent(CommentCore_.CommentState.SEND)
            .buildCommonMetadataAndProvider(mCommonMetadataProvider,createComment)
            .build()
}

// 具体可配置项如下：
// 配置具体事件(事件默认两个，可自由扩展)
public Builder buildCommentEvent(@NotNull ICommentState commentState);

// 配置内容提供者以及需要操作的评论项
public <C> Builder buildCommonMetadataAndProvider(@NotNull CommonMetadataProvider commonMetadataProvider, C comment);

// 配置回调
public Builder buildCallBack(BackgroundTaskHandler.OnNetResponseCallBack callBack) ;

```
#####2.使用：
```java
private CommentCore_ comment;
CommentCore_.Builder builder = new CommentCore_.Builder();
comment = builder.buildCommentEvent(CommentCore_.CommentState.SEND)
               .buildCommonMetadataAndProvider(mCommonMetadataProvider,createComment)
               .build()
               
comment.handleCommentInBackGroud();
// 或者
comment.handleComment();
```
#### 四.实现&&扩展 ：
* 实现：
内容提供者由使用的人继承CommonMetadataProvider实现，将用户的评论类型转换为公用模型,内部默认实现了数据库操作
示例：
见 `com.zhiyicx.thinksnsplus.comment.TCommonMetadataProvider`
* 扩展
1.事件扩展
实现 `ICommentEvent` 接口即可添加自定义事件
实现 `ICommentState` 接口提供自定义的事件
在 `buildCommentEvent(@NotNull ICommentState commentState)` 传入自定义事件即可

2017-05-11 10:39:25

  [1]: https://thumbnail0.baidupcs.com/thumbnail/8bed1a941e307dd327eb251375a6c6e0?fid=3510106323-250528-312874332999233&time=1494464400&rt=sh&sign=FDTAER-DCb740ccc5511e5e8fedcff06b081203-EsDSliZNgw5a3V0sZW3yvx5gHgk=&expires=8h&chkv=0&chkbd=0&chkpc=&dp-logid=3024668916108267044&dp-callid=0&size=c710_u400&quality=100