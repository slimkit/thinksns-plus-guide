2017年10月24日11:44:01
# 刷新控件

使用第三方库[SmartRefreshLayout](https://github.com/scwang90/SmartRefreshLayout)

## 特点功能:

 - 支持多点触摸
 - 支持嵌套多层的视图结构
 - 支持所有的 View（AbsListView、RecyclerView、WebView....View）
 - 支持自定义并且已经集成了很多炫酷的 Header 和 Footer.
 - 支持和ListView的无缝同步滚动 和 CoordinatorLayout 的嵌套滚动 .
 - 支持自动刷新、自动上拉加载（自动检测列表惯性滚动到底部，而不用手动上拉）.
 - 支持自定义回弹动画的插值器，实现各种炫酷的动画效果.
 - 支持设置主题来适配任何场景的App，不会出现炫酷但很尴尬的情况.
 - 支持设多种滑动方式：平移、拉伸、背后固定、顶层固定、全屏
 - 支持所有可滚动视图的越界回弹

## 传送门

 - [属性方法](https://github.com/scwang90/SmartRefreshLayout/art/md_property.md)
 - [智能之处](https://github.com/scwang90/SmartRefreshLayout/art/md_smart.md)
 - [常见问题](https://github.com/scwang90/SmartRefreshLayout/issues/71)
 - [更新日志](https://github.com/scwang90/SmartRefreshLayout/art/md_update.md)
 - [博客文章](https://segmentfault.com/a/1190000010066071)
 - [源码下载](https://github.com/scwang90/SmartRefreshLayout/releases)
 - [多点触摸](https://github.com/scwang90/SmartRefreshLayout/art/md_multitouch.md)
 - [自定义Header](https://github.com/scwang90/SmartRefreshLayout/art/md_custom.md)


## 简单用例
#### 1.在 buld.gradle 中添加依赖
```
compile 'com.android.support:design:25.3.1'//版本随意（非必须，引用可以解决无法预览问题）
compile 'com.android.support:appcompat-v7:25.3.1'//版本随意（必须）
compile 'com.scwang.smartrefresh:SmartRefreshLayout:1.0.3'
compile 'com.scwang.smartrefresh:SmartRefreshHeader:1.0.3'//没有使用特殊Header，可以不加这行

//新版本预览版-可能不稳定
compile 'com.scwang.smartrefresh:SmartRefreshLayout:1.0.4-alpha-5'
compile 'com.scwang.smartrefresh:SmartRefreshHeader:1.0.4-alpha-5'
```

#### 2.在XML布局文件中添加 SmartRefreshLayout
```xml
<?xml version="1.0" encoding="utf-8"?>
<com.scwang.smartrefresh.layout.SmartRefreshLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/refreshLayout"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <android.support.v7.widget.RecyclerView
        android:id="@+id/recyclerview"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:overScrollMode="never"
        android:background="#fff" />
</com.scwang.smartrefresh.layout.SmartRefreshLayout>
```

#### 3.在 Activity 或者 Fragment 中添加代码
```java
RefreshLayout refreshLayout = (RefreshLayout)findViewById(R.id.refreshLayout);
refreshLayout.setOnRefreshListener(new OnRefreshListener() {
    @Override
    public void onRefresh(RefreshLayout refreshlayout) {
        refreshlayout.finishRefresh(2000);
    }
});
refreshLayout.setOnLoadmoreListener(new OnLoadmoreListener() {
    @Override
    public void onLoadmore(RefreshLayout refreshlayout) {
        refreshlayout.finishLoadmore(2000);
    }
});
```

## 使用指定的 Header 和 Footer

#### 1.方法一 全局设置
```java
public class App extends Application {
    //static 代码段可以防止内存泄露
    static {
        //设置全局的Header构建器
        SmartRefreshLayout.setDefaultRefreshHeaderCreater(new DefaultRefreshHeaderCreater() {
                @Override
                public RefreshHeader createRefreshHeader(Context context, RefreshLayout layout) {
                    layout.setPrimaryColorsId(R.color.colorPrimary, android.R.color.white);//全局设置主题颜色
                    return new ClassicsHeader(context).setSpinnerStyle(SpinnerStyle.Translate);//指定为经典Header，默认是 贝塞尔雷达Header
                }
            });
        //设置全局的Footer构建器
        SmartRefreshLayout.setDefaultRefreshFooterCreater(new DefaultRefreshFooterCreater() {
                @Override
                public RefreshFooter createRefreshFooter(Context context, RefreshLayout layout) {
                    //指定为经典Footer，默认是 BallPulseFooter
                    return new ClassicsFooter(context).setSpinnerStyle(SpinnerStyle.Translate);
                }
            });
    }
}
```

注意：方法一 设置的Header和Footer的优先级是最低的，如果同时还使用了方法二、三，将会被其它方法取代


#### 2.方法二 XML布局文件指定
```xml
<com.scwang.smartrefresh.layout.SmartRefreshLayout
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/refreshLayout"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="#444444"
    app:srlPrimaryColor="#444444"
    app:srlAccentColor="@android:color/white"
    app:srlEnablePreviewInEditMode="true">
    <!--srlAccentColor srlPrimaryColor 将会改变 Header 和 Footer 的主题颜色-->
    <!--srlEnablePreviewInEditMode 可以开启和关闭预览功能-->
    <com.scwang.smartrefresh.layout.header.ClassicsHeader
        android:layout_width="match_parent"
        android:layout_height="wrap_content"/>
    <TextView
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:padding="@dimen/padding_common"
        android:background="@android:color/white"
        android:text="@string/description_define_in_xml"/>
    <com.scwang.smartrefresh.layout.footer.ClassicsFooter
        android:layout_width="match_parent"
        android:layout_height="wrap_content"/>
</com.scwang.smartrefresh.layout.SmartRefreshLayout>
```

注意：方法二 XML设置的Header和Footer的优先级是中等的，会被方法三覆盖。而且使用本方法的时候，Android Studio 会有预览效果，如下图：

![](https://github.com/scwang90/SmartRefreshLayout/art/jpg_preview_xml_define.jpg)

不过不用担心，只是预览效果，运行的时候只有下拉才会出现~

#### 3.方法三 Java代码设置
```java
final RefreshLayout refreshLayout = (RefreshLayout) findViewById(R.id.refreshLayout);
//设置 Header 为 Material样式
refreshLayout.setRefreshHeader(new MaterialHeader(this).setShowBezierWave(true));
//设置 Footer 为 球脉冲
refreshLayout.setRefreshFooter(new BallPulseFooter(this).setSpinnerStyle(SpinnerStyle.Scale));
```

## 混淆

SmartRefreshLayout 没有使用到：序列化、反序列化、JNI、反射，所以并不需要添加混淆过滤代码，并且已经混淆测试通过，如果你在项目的使用中混淆之后出现问题，请及时通知我。

## 感谢
- [SmartRefreshLayout](https://github.com/scwang90/SmartRefreshLayout)
- [SwipeRefreshLayout](https://developer.android.com/reference/android/support/v4/widget/SwipeRefreshLayout.html)
- [TwinklingRefreshLayout](https://github.com/lcodecorex/TwinklingRefreshLayout)
- [Ultra-Pull-To-Refresh](https://github.com/liaohuqiu/android-Ultra-Pull-To-Refresh)

