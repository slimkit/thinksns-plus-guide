# input
> 请求认证时的第一步，输入一些基本信息的页面相关的类

- CertificationInputActivity
> 请求认证时的第一步展示的Activity，继承TSActivity

- CertificationInputComponent
> 请求认证时的第一步页面的注入器，依赖APPComponent获取需要得对象，注入CertificationInputPresenterModule

- CertificationInputContract
> 请求认证时的第一步页面的一个接口类，主要是将presenter和view的抽象方法，放在同一个类方便管理

-  
> 请求认证时的第一步页面的Fragment，主要的功能就是在该类中实现的，负责UI方面的功能

- CertificationInputPresenter
> 请求认证时的第一步页面的Presenter，主要是负责事件的处理

- CertificationInputPresenterModule
> 请求认证时的第一步页面的Module，为了能注入Presenter，将需要的Fragment提供出去