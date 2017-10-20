2017年5月11日 14:06:58
# 6.0 权限适配

## 说明
为了配合使用`rxbinding`，并且使用简单、快捷，故权限适配是用的是[RxPermissions](https://github.com/tbruyelle/RxPermissions)

## 使用说明

当前项目在`BaseFragment`中已经初始化，并设置了开关，默认不适用，需要使用时需要在子类中重写如下方法

```java
    @Override
    protected boolean usePermisson() {
        return true;
    }

```

1. 单独权限
```java
   RxPermissions rxPermissions = new RxPermissions(this); // this is a acitiviy reference
   rxPermissions.setLogging(true); //是否需要日志
   rxPermissions
    .request(Manifest.permission.CAMERA)
    .subscribe(granted -> {
           if (permission.granted) {// 获取到了权限
           ...
               } else if (permission.shouldShowRequestPermissionRationale) {// 拒绝权限，但是可以再次请求
                          ...
                          } else {//永久拒绝
                                ...
                               }
    });
```
2. 配合`rxbinding`使用
```java
   // 点击登录按钮
           RxView.clicks(mBtLoginLogin)
                   .throttleFirst(JITTER_SPACING_TIME, TimeUnit.SECONDS)
                   .compose(this.<Void>bindToLifecycle())
                   .compose(mRxPermissions.ensureEach(Manifest.permission.READ_PHONE_STATE))
                   .subscribe(new Action1<Permission>() {
                       @Override
                       public void call(Permission permission) {
                           if (permission.granted) {// 获取到了权限
                               mPresenter.login(mEtLoginPhone.getText().toString().trim(), mEtLoginPassword.getText().toString().trim());
                           } else if (permission.shouldShowRequestPermissionRationale) {// 拒绝权限，但是可以再次请求
                               showErrorTips(getString(R.string.permisson_refused));
                           } else {//永久拒绝
                               showErrorTips(getString(R.string.permisson_refused_nerver_ask));
                           }
                       }
                   });

```
3. 多权限,统一处理 ,You can also observe a detailed result with requestEach or ensureEach :
```java
rxPermissions
    .request(Manifest.permission.CAMERA,
             Manifest.permission.READ_PHONE_STATE)
    .subscribe(granted -> {
        if (granted) {
           // All requested permissions are granted
        } else {
           // At least one permission is denied
        }
    });

    rxPermissions
        .requestEach(Manifest.permission.CAMERA,
                 Manifest.permission.READ_PHONE_STATE)
        .subscribe(permission -> { // will emit 2 Permission objects
            if (permission.granted) {
               // `permission.name` is granted !
            } else if (permission.shouldShowRequestPermissionRationale)
               // Denied permission without ask never again
            } else {
               // Denied permission with ask never again
               // Need to go to the settings
            }
        });

```
