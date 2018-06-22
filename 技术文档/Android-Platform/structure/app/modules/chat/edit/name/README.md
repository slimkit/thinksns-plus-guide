# name
> 该包下是群组名字编辑页面

- EditGroupNameActivity
> 群组名字编辑页面的Activity，继承自TSActivity

- EditGroupNameComponent
> 群组名字编辑页面的dagger注入器，依赖AppComponent，由EditGroupNamePresenterModule提供view

- EditGroupNameContract
> 群组名字编辑页面的一个接口类，主要是将presenter和view的抽象方法，放在同一个类方便管理

- EditGroupNameFragment
> 群组名字编辑页面的Fragment，主要功能在改类中实现，UI绘制和交互

- EditGroupNamePresenter
> 群组名字编辑页面的Presenter，主要是负责事件的处理

- EditGroupNamePresenterModule
> 群组名字编辑页面的Module，为了能注入Presenter，将需要的Fragment提供出去