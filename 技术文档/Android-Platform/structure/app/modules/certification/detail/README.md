# detail
> 认证详情页面相关的类

- CertificationDetailActivity
> 认证信息详情展示的Activity，继承TSActivity

- CertificationDetailComponent
> 认证详情页面的注入器，依赖APPComponent获取需要得对象，注入CertificationDetailPresenterModule

- CertificationDetailContract
> 认证详情页面的一个接口类，主要是将presenter和view的抽象方法，放在同一个类方便管理

- CertificationDetailFragment
> 认证详情页面的Fragment，主要的功能就是在该类中实现的，负责UI方面的功能

- CertificationDetailPresenter
> 认证详情页面的Presenter，主要是负责事件的处理

- CertificationDetailPresenterModule
> 认证详情页面的Module，为了能注入Presenter，将需要的Fragment提供出去