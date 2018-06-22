# send
> 请求认证时的第二步，上传相关图片的页面

- SendCertificationActivity
> 请求认证时的第二步展示的Activity，继承TSActivity

- SendCertificationComponent
> 请求认证时的第二步页面的注入器，依赖APPComponent获取需要得对象，注入SendCertificationPresenterModule

- SendCertificationContract
> 请求认证时的第二步页面的一个接口类，主要是将presenter和view的抽象方法，放在同一个类方便管理

- SendCertificationFragment
> 请求认证时的第二步页面的Fragment，主要的功能就是在该类中实现的，负责UI方面的功能

- SendCertificationPresenter
> 请求认证时的第二步页面的Presenter，主要是负责事件的处理

- SendCertificationPresenterModule
> 请求认证时的第二步页面的Module，为了能注入Presenter，将需要的Fragment提供出去