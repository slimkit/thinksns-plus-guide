2017年5月11日 11:25:19
# Lint 检测说明

    Lint 工具可检查您的 Android 项目源文件是否包含潜在错误，以及在正确性、安全性、性能、易用性、便利性和国际化方面是否需要优化改进。
    目前使用的是 Android 自带的 lint 规则 ；了解更多可以查看 Android Studio 开发指南

[hintdev]:https://developer.android.google.cn/studio/write/lint.html
[Android Studio 开发指南](hintdev)

## 自定义 Lint 规则

关联库：https://github.com/zhiyicx/thinksns-android-lint/tree/master/thinksns-plus-lint

## 使用
在`build.gradle`中导入关联库的` aar` 包即可

## Lint 报告
在 `Terminal` 中执行如下操作即可,日志报告会在对应`module`中的`build/outputs/`中.
在 Windows 上：
```
gradlew lint
```
在 Linux 或 Mac 上：
```
./gradlew lint
```
注意：通过适用于 Gradle 的 Android 插件，您可以使用模块级 build.gradle 文件中的 lintOptions {} 块配置某些 Lint 选项，例如要执行或忽略哪些检查。以下代码段显示了您可以配置的部分属性：
```grovvy
  lintOptions {
   // Turns off checks for the issue IDs you specify.
    disable 'TypographyFractions','TypographyQuotes'
    // Turns on checks for the issue IDs you specify. These checks are in
    // addition to the default lint checks.
    enable 'RtlHardcoded','RtlCompat', 'RtlEnabled'
    // To enable checks for only a subset of issue IDs and ignore all others,
    // list the issue IDs with the 'check' property instead. This property overrides
    // any issue IDs you enable or disable using the properties above.
    check 'NewApi', 'InlinedApi'
    // If set to true, turns off analysis progress reporting by lint.
    quiet true
    // if set to true (default), stops the build if errors are found.
    abortOnError false
    // if true, only report errors.
    ignoreWarnings true
   }
```

## 手动检查

    手动运行检查，您可以选择 Inspect Code > Analyze，手动运行已配置的 Lint 和其他 IDE 检查。检查结果显示在 Inspection Results 窗口中。

### 推荐文章
 - [Android Lint Checks](http://tools.android.com/tips/lint-checks)
 - [Android自定义Lint实践](http://tech.meituan.com/android_custom_lint.html)
 - [如何自定义Lint规则](https://github.com/Jungle68/android-tech-frontier/blob/master/issue-33/%E5%A6%82%E4%BD%95%E8%87%AA%E5%AE%9A%E4%B9%89Lint%E8%A7%84%E5%88%99.md)