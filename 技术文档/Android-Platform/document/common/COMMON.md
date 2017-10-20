2017年5月11日 13:57:00
# common module
## 结构目录
```
-- base
-- config
-- dagger
-- mvp
-- net
-- thirdmanager
-- utils
-- widget
```

包含了项目开发中能够被反复使用，但又很少修改的一些最基本的类和方法，主要是规则定义；

_**不建议除了当前项目开发人员和维护人员以外的人，对其进行修改；**_

###base目录：
    主工程中会重写相应的基类，作为项目的基类使用

### [config目录：](CONSTANTCONFIG.md)
    项目中可能需要的辅助常量，比如：时分秒

### [dagger目录：](DAGGER2.md)

    基于dagger2，添加了一些公用的module
### [mvp目录：](MVP.md)
    定义了BasePresenter基类，IBaseView接口，项目中的mvp相关类都继承于此

### [net目录：](HTTP.md)
    使用retrofit网络框架进行网络请求，该目录下包含了retrofit的一些自定义拦截器

### [thirdmanager.share目录：](../baseproject/THIRDSHARE)
    定义了分享功能的一些接口，如果需要替换工程中的分享平台，需要遵循这些接口定义；

### [utils目录：](UTILS.md)
    封装了一些常用的工具类，在版本升级或者维护中，会继续扩充更多的功能
### [widget目录：](WIDGET.md)
    封装了一些基础的，通用的自定义控件，在版本升级或者维护中，会继续扩充更多的功能

## 文档说明
[common](document/common/COMMON.md) 基础框架包，基础公用累，接入人员不用修改，不包含资源文件，可打成`jar`
>- [辅助常量定义](document/common/CONSTANTCONFIG.md)
>- [dagger2 说明](document/common/DAGGER2.md)
>- [mvp 说明](document/common/MVP.md)
>- [6.0 权限适配](document/common/PERMISSION.md)
>- [日志说明](document/common/LOG.md)
>- [日志说明](document/common/LOG.md)
>- [常用工具类集合](document/common/UTILS.md)
>- [基础自定义控件](document/common/WIDGET.md)
