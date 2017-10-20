2017年5月11日 13:52:54
# 针对rxandroid网络请求的错误处理

##  1.概述
定义了一个针对`rxandroid`网络请求的错误处理的统一回掉，方便需要统一处理rx错误的需求；


## 2.定义
- `ErrorHandleSubscriber`
继承至`Subscriber`,用于拦截`Rx`错误哦
-  `ResponseErroListener`
统一错误回调接口
-  `RetryWithDelay`
实现` Func1<Observable<? extends Throwable>`,可用于`rx`错误重复间隔请求


## 3.使用
```java
  mModel.getUsers(lastUserId, pullToRefresh)
                .subscribeOn(Schedulers.io())
                .retryWhen(new RetryWithDelay(3, 2))//遇到错误时重试,第一个参数为重试几次,第二个参数为重试的间隔
                .doOnSubscribe(new Action0() {
                    @Override
                    public void call() {
                        if (pullToRefresh)
                            mRootView.showLoading();//显示上拉刷新的进度条
                        else
                            mRootView.startLoadMore();
                    }
                }).subscribeOn(AndroidSchedulers.mainThread())
                .observeOn(AndroidSchedulers.mainThread())
                .doAfterTerminate(new Action0() {
                    @Override
                    public void call() {
                        if (pullToRefresh)
                            mRootView.hideLoading();//隐藏上拉刷新的进度条
                        else
                            mRootView.endLoadMore();
                    }
                })
                .compose(((BaseActivity) mRootView).<List<User>>bindToLifecycle())//使用RXlifecycle,使subscription和activity一起销毁
                .subscribe(new ErrorHandleSubscriber<List<User>>(mErrorHandler) {
                    @Override
                    public void onNext(List<User> users) {
                        lastUserId = users.get(users.size() - 1).getId();//记录最后一个id,用于下一次请求
                        if (pullToRefresh) mUsers.clear();//如果是上拉刷新则晴空列表
                        for (User user : users) {
                            mUsers.add(user);
                        }
                        mAdapter.notifyDataSetChanged();//通知更新数据
                    }
                });
```
统一错误回掉已经注入`Dagger`,只需要重写`BaseApplication`的`  protected ResponseErroListener getResponseErroListener()`即可

