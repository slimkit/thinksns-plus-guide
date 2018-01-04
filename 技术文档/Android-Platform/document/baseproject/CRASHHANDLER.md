2018年01月04日11:10:10

# 程序异常崩溃处理

 app遇到各种没有捕获的异常，导致崩溃时，可能需要进行相关的处理，比如友好提示，异常信息收集。。。核心 code

 ---
```
    new Handler(Looper.getMainLooper()).post(new Runnable() {
            @Override
            public void run() {
                try {
                    Looper.loop();
                } catch (Throwable e) {
                    if (mCrashListener != null) {//捕获异常处理
                        mCrashListener.uncaughtException(Looper.getMainLooper().getThread(), e);
                    }
                }
            }
        });

        Thread.setDefaultUncaughtExceptionHandler(new Thread.UncaughtExceptionHandler() {
            @Override
            public void uncaughtException(Thread t, Throwable e) {
                if (mCrashListener != null) {//捕获异常处理
                    mCrashListener.uncaughtException(t, e);
                }
            }
        });
```

继承Thread.UncaughtExceptionHandler编写自己的未捕获异常处理器，重写uncaughtException方法，
在该方法中，处理异常信息；具体逻辑见详细代码

## 使用
在Apllication的oncreate中，设置自定义的UncaughtExceptionHandler
```
       /// 处理app崩溃异常
          CrashClient.init(new CrashClient.CrashListener() {
              @SuppressLint("MyToastHelper")
              @Override
              public void uncaughtException(Thread t, Throwable e) {
                  e.printStackTrace();
                  Toast.makeText(BaseApplication.getContext(), R.string.app_crash_tip, Toast.LENGTH_SHORT).show();
                  rx.Observable.timer(DEFAULT_TOAST_SHORT_DISPLAY_TIME, TimeUnit.MILLISECONDS)
                          .subscribe(new Action1<Long>() {
                              @Override
                              public void call(Long aLong) {
                                  ActivityHandler.getInstance().AppExit();
                              }
                          });
              }
          });
```

注意： 发布版本不需要,用与本地测试错误收集
