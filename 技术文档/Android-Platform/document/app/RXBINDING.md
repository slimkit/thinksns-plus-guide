2017年5月11日 11:28:45
# RxBinding的使用

    处理控件的异步调用，解决按钮的重复点击问题；

 [RxBinding的github链接](https://github.com/JakeWharton/RxBinding)

## 配置
在config.gradle中，配置了RxBinding的依赖库，就是一些配合android官方控件的封装
```
            // rxBindding
            rxbinding                    : "com.jakewharton.rxbinding:rxbinding:1.0.0",
            rxbindingSupportV4           : "com.jakewharton.rxbinding:rxbinding-support-v4:1.0.0",
            rxbindingSupportV7           : "com.jakewharton.rxbinding:rxbinding-appcompat-v7:1.0.0",
            rxbindingDesign              : "com.jakewharton.rxbinding:rxbinding-design:1.0.0",
            rxbindingDesignrRcyclerviewV7: "com.jakewharton.rxbinding:rxbinding-recyclerview-v7:1.0.0",
```

## 使用方法
这里给出几个简单的例子，来说明RxBinding的好处
- 监听edittext输入框的内容变化

```
        // 手机号码输入框观察
        RxTextView.textChanges(mEtLoginPhone)
                .compose(this.<CharSequence>bindToLifecycle())
                .subscribe(new Action1<CharSequence>() {
                    @Override
                    public void call(CharSequence charSequence) {
                        isPhoneEdited = !TextUtils.isEmpty(charSequence.toString());
                        setConfirmEnable();
                    }
                });
```

  使用RxTextView.textChanges(mEtLoginPhone)方法得到Observable<CharSequence>对象，接下来就是RxJava的使用；
  通过Rxbinding来监听手机号码的内容变化，实际上是在textChanges方法中实现了Edittext的TextWatcher监听，
  通过RxJava的方式，实现优雅的代码编写


- 监听控件的点击事件，处理重复点击导致的反复网络请求
```
        // 点击登录按钮
        RxView.clicks(mBtLoginLogin)
                .throttleFirst(JITTER_SPACING_TIME, TimeUnit.SECONDS)
                .compose(this.<Void>bindToLifecycle())
                .subscribe(new Action1<Void>() {
                    @Override
                    public void call(Void aVoid) {
                        mPresenter.login(mEtLoginPhone.getText().toString().trim(), mEtLoginPassword.getText().toString().trim());
                    }
                });
```

   throttleFirst是RxJava中的操作符，能够定期发射这个时间段里源Observable发射的第一个数据，
   JITTER_SPACING_TIME是设置的两次点击的最小间隔时间 <br>



