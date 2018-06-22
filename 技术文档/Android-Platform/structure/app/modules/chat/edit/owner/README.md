# owner
> 该包下是编辑群组群主的页面

- EditGroupOwnerActivity
> 编辑群组群主的页面的Activity，继承自TSActivity

- EditGroupOwnerComponent
> 编辑群组群主的页面的dagger注入器，依赖AppComponent，由EditGroupOwnerPresenterModule提供view

- EditGroupOwnerContract
> 编辑群组群主的页面的一个接口类，主要是将presenter和view的抽象方法，放在同一个类方便管理

- EditGroupOwnerFragment
> 编辑群组群主的页面的Fragment，主要功能在改类中实现，UI绘制和交互

- EditGroupOwnerPresenter
> 编辑群组群主的页面的Presenter，主要是负责事件的处理

- EditGroupOwnerPresenterModule
> 编辑群组群主的页面的Module，为了能注入Presenter，将需要的Fragment提供出去