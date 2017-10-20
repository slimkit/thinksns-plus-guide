# 换肤与修改字体说明

## 换肤

本项目使用的换肤框架来源于 [Android-skin-support](https://github.com/ximsfei/Android-skin-support)

注意：
### <span id = "SkinUtils">SkinUtils</span>
由于换肤框架是通过类加载机制通过`resourceId`来修改的颜色或者背景，部分情况下无法获取到`resourceId`导致了无法替换，例如
```java
mTextView.setTextColor(int color);
```
此时传递到加载类的是`color`而不是`resourceId`，所以会加载失败。故修改了`skin-spport`是源码，兼容了
```java
mTextView.setTextColor(int resourceId);
```
但是此兼容对原来的代码并不友好，也不方便统一管理，故增加了`SkinUtils.setCompatTextColor(Textview,resourceId)`方法；
同时根据源码找到了`SkinCompatResources`,此类是对资源的转换,大家也可以通过其中的方法完成自己想要的东西。建议修改文字颜色的时候使用
```java
SkinUtils.setTextColor(int resourceId);
```

## 字体修改
本项目使用的字体修改框架源于[Calligraphy](https://github.com/chrisjenx/Calligraphy)