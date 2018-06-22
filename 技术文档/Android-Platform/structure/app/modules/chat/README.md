# chat
> 该包下主要是和聊天界面相关的类

- [adapter 包](./adapter)

- [call 包](./call)
 
- [edit 包](./edit)

- [info 包](./info)

- [item 包](./item)

- [location 包](./location)

- [member 包](./member)

- [select 包](./select)

- [video 包](./video)

- ChatActivity
> 聊天详情页

- ChatComponent
> 聊天详情页面的注入器，依赖APPComponent获取需要得对象，注入ChatPresenterModule

- ChatContract
> 聊天详情页面的一个接口类，主要是将presenter和view的抽象方法，放在同一个类方便管理

- ChatFragment
> 聊天详情页面的Fragment，主要的功能就是在该类中实现的，负责UI方面的功能

- ChatPresenter
> 聊天详情页面的Presenter，主要是负责事件的处理

- ChatPresenterModule
> 聊天详情页面的Module，为了能注入Presenter，将需要的Fragment提供出去