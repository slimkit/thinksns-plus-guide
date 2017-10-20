2017年5月11日 11:37:11
# 关于TSActivity和TSFragment

本项目中，所有的实际功能和ui都放在TSFragment中，而TSActivity只是针对fragment进行控制，
这样做的好处是：面对页面变化很大的需求时，在不改变fragment逻辑结构情况下，只需
要简单的处理activity和fragment的关系，就可以达到我们需要的效果；

- 单个fragment的页面： TSActivity -> TSFragment
- 多个fragment的页面： TSActivity -> TSFragment -> viewPager -> 多个TSFragment

**我们只需要对TSFragment着重进行封装处理，TSActivtiy简单处理即可；**

**除非需要改变fragment的内容，否则可以把fragment放到任何地方；**

**ToolBar放在TSFragment中，控制显示隐藏，TSActivity则没有ToolBar；**

## BaseActivity
 统一基类
  - 提供 Butterkinfe 注册解绑
  - 提供选择 Eventbus 注册
  - 提供数据保存与读取
  - 提供 Activity 栈管理
  - 提供 Dagger 初始化方法
## TSActivity
 用于封装Fragment 和 Presenter 和提供统一的Activity 布局

## BaseFragment
 统一基类
 - 提供 Butterkinfe 注册解绑
 - 提供选择 Eventbus 注册
 - 提供[权限管理类](../common/PERMISSION.md)
 - 提供换肤管理
 - 提供类是 QQ 的从顶部弹出的提示信息

[]()
## TSFragment

 - 提供状态栏的浸入式
 - 提供通用的 TitlBar (可选择使用)
 - 新增状态栏文字颜色调整方法
 - 新增状态栏占位 View
 - 新增加载占位图与动画

注意; titlebar 可自定义也可直接使用，提供修改颜色、图片、文字以及响应事件等；具体请查看[TSFragment](../../baseproject/src/main/java/com/zhiyicx/baseproject/base/TSFragment.java)


### 新增加载占位图与动画说明

 为了详情页面全部接口加载完成后再更新UI，故在`TSFragment`中新增加载占位图与动画，通过一下方法进行使用
 ```java
 /**
  * 是否使用加载动画
  */
 boolean setUseCenterLoading(); // 返回 true 表示使用加载动画

  /**
   * 显示加载失败
   */
 protected void showLoadError()

 /**
  * 设置加载失败占位图
  *
  * @param resId
  */
 protected void setLoadHolderIma(@DrawableRes int resId)

/**
 * 加载失败，占位图点击事件
 */
 protected void setLoadingHolderClick()

/**
 * 关闭加载动画
 */
 protected void closeLoading()

/**
 * 获取状态栏和操作栏的高度
 *
 * @return
 */
 protected int getstatusbarAndToolbarHeight()

 ```
注意：`getstatusbarAndToolbarHeight` 方法默认返回的高度为  `状态栏高度 + Toolbar 高度 + 分割线高度`