# container
> 该包下是所有圈子的ViewPager页面

- AllCircleContainerActivity
> 所有圈子页面的Activity

- AllCircleContainerComponent
> 所有圈子页面的注入器

- AllCircleContainerContract
> 所有圈子页面的接口类，便于管理presenter，view，repository中的方法

- AllCircleContainerFragment
> 所有圈子页面的Fragment，主要是使用presenter获取圈子的分类，然后根据获取到的分类，向ViewPager中添加对应的圈子列表的Fragment

- AllCircleContainerPresenter
> 所有圈子页面的presenter类

- AllCircleContainerPresenterModule
> 所有圈子页面的Module，提供view