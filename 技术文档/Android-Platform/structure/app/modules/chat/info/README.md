# info
> 该包下是编辑群组群主的页面

- ChatInfoActivity
> 聊天信息页面的Activity，继承自TSActivity（查看聊天信息的界面，就是由哪些人组成）

- ChatInfoComponent
> 聊天信息页面的dagger注入器，依赖AppComponent，由ChatInfoPresenterModule提供view

- ChatInfoContract
> 聊天信息页面的一个接口类，主要是将presenter和view的抽象方法，放在同一个类方便管理

- ChatInfoFragment
> 聊天信息页面的Fragment，主要功能在改类中实现，UI绘制和交互

- ChatInfoPresenter
> 聊天信息页面的Presenter，主要是负责事件的处理

- ChatInfoPresenterModule
> 聊天信息页面的Module，为了能注入Presenter，将需要的Fragment提供出去