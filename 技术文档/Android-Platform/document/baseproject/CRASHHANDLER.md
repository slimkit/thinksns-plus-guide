2017年5月11日 11:37:59

# 程序异常崩溃处理
 app遇到各种没有捕获的异常，导致崩溃时，可能需要进行相关的处理，比如友好提示，异常信息收集。。。

 ---
```
public class CrashHandler implements Thread.UncaughtExceptionHandler {
    @Override
      public void uncaughtException(Thread thread, Throwable throwable) {
      ...
      }
}
```

继承Thread.UncaughtExceptionHandler编写自己的未捕获异常处理器，重写uncaughtException方法，
在该方法中，处理异常信息；具体逻辑见详细代码

## 使用
在Apllication的oncreate中，设置自定义的UncaughtExceptionHandler
```
        // 处理app崩溃异常
        CrashHandler crashHandler = CrashHandler.getInstance();
        crashHandler.init(this);
```

注意： 发布版本不需要,用与本地测试错误收集
