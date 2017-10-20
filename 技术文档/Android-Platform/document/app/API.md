2017年10月17日11:21:48
# Api 说明文档

## 接口版本说明
- V1 请参考[后台开发库](https://github.com/zhiyicx/thinksns-plus/blob/master/documents/api/v1/overview.md)

- V2 请参考[后台开发库](https://slimkit.github.io/plus-docs/v2)

## 接口外层结构 ( Just for V1 )

```
{
    "status": false,
    "code": 0,
    "message": "",
    "data": null
}
```
## `Rx` 的`subscribe` 请求服务器数据时，请务必使用`BaseSubscribe`

### why ?

由于服务器返回的`http status code` 使用的是`RESETFUL API` 规范，如果不使用，那么服务器返回`status = false`时，`retrofit` 将调用`onError` 方法
，
所以进行了数据解析改装后返回处理结果，
结果函数：
```java
    /**
     * 服务器正确处理返回正确数据
     * @param data 正确的数据
     */
    protected abstract void onSuccess(T data);

    /**
     * 服务器正确接收到请求，主动返回错误状态以及数据
     * @param message 错误信息
     * @param code 错误编码
     */
    protected abstract void onFailure(String message, int code);

    /**
     *  系统级错误，网络错误，系统内核错误等
     * @param throwable
     */
    protected abstract void onException(Throwable throwable);

```

### Api 接口位置说明

    整个项目的接口定义都位于 `com.zhiyicx.baseproject.config.ApiConfig.class` 中，根据不同的需求将接口分别实现于不同的 `Net Client`,存放于 `com.zhiyicx.thinksnsplus.data.source.remote` 下

 Api 域名配置

 ```
    public static final boolean APP_IS_NEED_SSH_CERTIFICATE = true;// 兼容 https , 自定义证书时使用 false
    public static final String APP_DOMAIN_DEV = "http://dev.zhibocloud.cn/";//
    public static final String APP_DOMAIN_FORMAL = "https://tsplus.zhibocloud.cn/";/ 正式服务器
    public static String APP_DOMAIN = APP_DOMAIN_DEV;
 ```

### 示例

V1
```java
     Subscription getVertifySub = mRepository.getVertifyCode(phone, CommonClient.VERTIFY_CODE_TYPE_REGISTER)
                .subscribe(new BaseSubscribe<CacheBean>() {
                    @Override
                    protected void onSuccess(CacheBean data) {
                        mRootView.hideLoading();//隐藏loading
//                                   timer.start();//开始倒计时
                        mRootView.setVertifyCodeLoadin(false);
                    }

                    @Override
                    protected void onFailure(String message,int code) {
                        mRootView.showMessage(message);
                        mRootView.setVertifyCodeBtEnabled(true);
                        mRootView.setVertifyCodeLoadin(false);
                    }

                    @Override
                    protected void onException(Throwable e) {
                        e.printStackTrace();
                        mRootView.showMessage(mContext.getString(R.string.err_net_not_work));
                        mRootView.setVertifyCodeBtEnabled(true);
                        mRootView.setVertifyCodeLoadin(false);
                    }
                });

```

V2
项目中也将逐步淘汰 `BaseSubscribe` ,改而使用 `BaseSubscribeForV2`
```java
    Subscription subscription = mUserInfoRepository.addTag(id)
                  .subscribe(new BaseSubscribeForV2<Object>() {
                      @Override
                      protected void onSuccess(Object data) {
                          mRootView.addTagSuccess(categoryPosition, tagPosition);
                      }

                      @Override
                      protected void onFailure(String message, int code) {
                          mRootView.showSnackErrorMessage(message);
                      }

                      @Override
                      public void onException(Throwable e) {
                          mRootView.showSnackErrorMessage(mContext.getString(R.string.err_net_not_work));
                      }
                  });
          addSubscrebe(subscription);

```



---

