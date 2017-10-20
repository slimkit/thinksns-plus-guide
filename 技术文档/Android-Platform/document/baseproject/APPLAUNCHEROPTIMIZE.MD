2017年5月11日 11:36:56
# App启动优化

## 概述
 防止App启动黑屏、白屏、闪屏问题

## 使用
通过设置设置Style

（1）设置背景图Theme

通过设置一张背景图。 当程序启动时，首先显示这张背景图，避免出现黑屏
```java
    <!--BackGroundLauncherTheme theme 启动页主题-->
    <style name="BackGroundLauncherTheme" parent="Theme.AppCompat.Light.NoActionBar">
        <item name="android:screenOrientation">portrait</item>
        <!--<item name="android:windowBackground">>@mipmap/splash</item>-->
    </style>
```
（2）设置透明Theme

通过把样式设置为透明，程序启动后不会黑屏而是整个透明了，等到界面初始化完才一次性显示出来
```java
    <!--TranslucentLAUNCHER theme 启动页主题-->
    <style name="TranslucentLauncherTheme" parent="android:Theme.Translucent.NoTitleBar.Fullscreen"/>
```
两者对比：

- Theme1 程序启动快，界面先显示背景图，然后再刷新其他界面控件,如果界面不同，有跳屏问题。
- Theme2 给人程序启动慢感觉，界面一次性刷出来，刷新同步。

（3）修改AndroidManifest.xml
```java
<application
        android:name=".App"
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:supportsRtl="true">
        <activity android:name=".MainActivity"
         android:theme="@style/AppTheme">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>

    //......

</application>
```