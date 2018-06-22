### config 
> 主要是一些提供常量配置的配置类

- BackgroundTaskRequestMethodConfig
> 是一个枚举类，提供一些操作常量（发布认证申请、评论问题等）

- DefaultUserInfoConfig
> 提供一个获取默认被删除用户的数据实体的方法

- ErrorCodeConfig
> 主要提供一些错误code（token 过期、用户认证失败、上传失败等）

- EventBusTagConfig
> 主要提供一些EventBus事件的Tag（后台刷新用户信息、发送动态到动态列表等）

- GlideConfiguration
> 该类主要是配置Glide，实现了GlideModule接口，对Glide进行配置

- JpushMessageTypeConfig
> 极光推送，推送消息的类型，基本定义

- NotificationConfig
> 通知分类常量

- SharePreferenceTagConfig
> SharePreference操作对象的分类

- TSImageOkHttpStreamFetcher
> 获取网络图片，可以实现获取方式

- TSImageRetryIntercepter
> 加载图片失败后重试的拦截器，可设置重试次数