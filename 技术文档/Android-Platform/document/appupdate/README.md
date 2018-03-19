# 版本更新说明


1. `AndroidManifest.xml` 加入以下代码
```
  <service
            android:name="com.zhiyicx.appupdate.UpdateService"
            android:enabled="true"
            android:exported="true"/>
```
2. 使用说明
```
        // app更新
        AppUpdateManager.getInstance(getContext()
                , ApiConfig.APP_DOMAIN + ApiConfig.APP_PATH_GET_APP_VERSION + "?version_code=" + DeviceUtils.getVersionCode(getContext()) + "&type=android")
                .startVersionCheck();
```

3. 主要代码逻辑请查看 `UpdateService` 和 `CustomVersionDialogActivity`