# all_circle
> 该包下是所有圈子的相关类，圈子列表，圈子列表的ViewPager

- [container 包](./container)

- CircleListComponent
> 圈子列表页面的注入器

- CircleListContract
> 圈子列表页面的接口类，该类主要是便于管理view，presenter，repository

- CircleListFragment
> 圈子列表页面的Fragment，该类主要就是调用presenter获取圈子，然后显示获取到的圈子

- CircleListPresenter
> 圈子列表页面的presenter

- CircleListPresenterModule
> 圈子列表页面的Module，用于提供view