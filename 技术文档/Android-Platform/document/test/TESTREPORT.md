2017年5月11日 13:52:45
# 测试报告生成说明

## 概述

众所周知，在编写单元测试案例的过程中，有一个重要的指标就是代码覆盖率。编写单元测试并不是为了追求100%覆盖率，但覆盖率在单元测试中仍占据着不可或缺的地位。
经典的`Java Coverage Tool : Emma、Eclemma（Eclipse推荐使用）、Cobertura`，感兴趣的学者可以自行查阅资料。

当前项目测试覆盖率报告统计使用的是`Android Studio`支持的`Coverage Tool ： jacoco、IntelliJ IDEA`
此框架`Android Studio`在`gradle`中已经集成了，当然如果需要自定义`Jacoco`版本可自行更改，示例如下
```grovy
apply plugin: 'jacoco'

jacoco {
    toolVersion = "0.7.5.201505241946"
}

```
**注：** `jacoco`最新的 [version](https://bintray.com/bintray/jcenter/org.jacoco:org.jacoco.core)

在`Android Stuido`新建过工程时，应该有注意到，该工程默认会新建`androidTest`及`test`的测试包。在`Android Stuido`中，在`androidTest`编写的单元测试，默认使用`jacoco`插件生成包含代码覆盖率的测试报告；而`test`包下的单元测试代码，则直接使用`Android Studio`已有工具`IntelliJ IDEA`生成覆盖率，当然也可以通过自定义`gradle task`使用`jacoco`插件生成与`androidTest`相同格式的测试报告,当前项目就是这样干的。

## 使用说明

1. 工程根目录下面已经写好了`androidTest`及`test`的`task`了,只需要在`module`的`build.gralde`中引入`jacocoCoverage.gradle`就可以了

```grovy
apply from: "$rootDir/jacocoCoverage.gradle"

```
2. 如需要测试报告，这是必要条件
```grovy
android{
...
buildTypes {

    debug {
        minifyEnabled false
    }
}
...
}部分手机执行后测试率任然为0（本人使用MX3测试覆盖率为0）
```
**注：** 部分手机执行后测试率任然为0（如MX3测试覆盖率为0）