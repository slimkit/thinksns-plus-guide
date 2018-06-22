### base
> 该包下是一些基础类

- AppApplication
> 继承自TSApplication，主要做一些应用初始化相关的操作（极光推送，环信，后台任务，通讯录等），提供一些和登录用户有关的方法

- AppBasePresenter
> 继承自BasePresenter，提供一些方法，是否登录，是否是游客，积分的检查等方法

- AppComponent
> 一个注入器，向AppApplication注入相关的对象，提供几个类的注入方法，向外部提供几个主要的对象

- BaseSubscribe
> RxJava的订阅器，主要处理服务器返回的数据，进行一些状态的判断，提供成功，失败，异常等回调接口

- BaseSubscribeForV2
> 和BaseSubscribe类似

- BaseWebLoad
> Web加载的基础类，向外提供加载完成回调接口

- CacheModule 
> 提供Cache相关的相关的配置

- EmptySubscribe
> 一个空的订阅器，空实现onError，onNext，使用户自关心完成事件

- InjectComponent
> 一个接口主要提供一个注入方法

- ServiceModule
> 提供网络操作的Client对象（网络接口Api）

- TSEaseBaseFragment
> 环信fragment base基类，继承 ts+ 基类，提供隐藏输入法的方法，和子类需要实现setUpView方法

- TSEaseChatFragment
> 环信聊天界面fragment 基类
