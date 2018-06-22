# manager
> 该包下是群组管理界面相关的类

- GroupManagerActivity
> 群组管理页面的Activity，继承自TSActivity

- GroupManagerComponent
> 群组管理界面的dagger注入器，依赖AppComponent，由GroupManagerPresenterModule提供view

- GroupManagerContract
> 群组管理页面的一个接口类，主要是将presenter和view的抽象方法，放在同一个类方便管理

- GroupManagerFragment
> 群组管理页面的Fragment，主要功能在改类中实现，UI绘制和交互

- GroupManagerPresenter
> 群组管理页面的Presenter，主要是负责事件的处理

- GroupManagerPresenterModule
> 群组管理页面的Module，为了能注入Presenter，将需要的Fragment提供出去