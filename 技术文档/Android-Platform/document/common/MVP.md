2017年5月11日 14:06:30
# mvp说明
## 1.概述
项目基于mvp模式进行开发，方便项目维护和升级

## 2.定义
项目中定义了如下的接口和基类
 - IBasePresenter接口，所有Presenter接口的父接口
 ```
     /**
      * 关联 Activity\Fragment 生命周期
      */
     void onStart();

     /**
      * 关联 Activity\Fragment 生命周期
      */
     void onDestroy();

     /**
      * 解绑 rx 注册
      *
      * @param subscription
      */
     void unSubscribe(Subscription subscription);
 ```
 - IBaseView接口：所有View接口的父接口

    根据实际需要，添加或者修改接口方法
 ```
     /**
      * 显示加载
      */
     void showLoading();

     /**
      * 隐藏加载
      */
     void hideLoading();

     /**
      * 显示信息
      */
     void showMessage(String message);

     /**
      * 杀死自己
      */
     void killMyself();
 ```
 - BasePresenter类

    所有相关Presenter的基类，实现IBasePresenter接口

 ```
 public abstract class BasePresenter<M, V extends IBaseView> implements IBasePresenter {
        ...
     /**
      * 是否使用 eventBus,默认为使用(true)，
      *
      * @return
      */
     protected boolean useEventBus() {
         return false;
     }

     @Override
     public void onStart() {
         if (useEventBus())// 如果要使用 eventbus 请将此方法返回 true
             EventBus.getDefault().register(this);// 注册 eventbus
     }

     @Override
     public void onDestroy() {
         if (useEventBus())// 如果要使用 eventbus 请将此方法返回 true
             EventBus.getDefault().unregister(this);// 解除注册 eventbus
         unSubscribe();// 解除订阅
         this.mModel = null;
         this.mRootView = null;
     }
        ...
}
 ```
    所有的子类Presenter在定义时，需要传入泛型，方便调用presenter方法

 ```
     protected void onDestroy() {
         super.onDestroy();
         synchronized (BaseActivity.class) {
             mApplication.getActivityList().remove(this);
         }
         if (mPresenter != null) mPresenter.onDestroy();// 释放资源
         if (mUnbinder != Unbinder.EMPTY) mUnbinder.unbind();
         if (useEventBus())// 如果要使用 eventbus 请将此方法返回 true
             EventBus.getDefault().unregister(this);
     }
 ```
    在BaseActivity中调用presenter的onDestroy()方法，释放presenter


## 3.使用
**一千个人眼中，有一千个哈姆雷特，一个项目中只需要一种MVP；（-。-）**

以项目中的登陆功能为例，我们需要实现下面的这些：

- LoginContract:定义mvp层次的接口
- LoginPresenter：实现登陆的Presenter接口
- LoginRepository:实现登陆的Model层接口
- LoginFragment中实现View接口
- LoginPresenterModule：针对Presenter的Dagger管理，添加的module
- LoginComponent：针对Presenter的Dagger管理，添加的Component
- LoginActivity：管理LoginFragment


