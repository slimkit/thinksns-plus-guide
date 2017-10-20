2017年5月11日 11:41:12
# 图片选择器


[github上的地址](https://github.com/donglua/PhotoPicker)

类似微信的图片选择器，从本地相册中单选或者多选图片，能够对图片进行预览

## 1.配置
在baseProject的AndroidManifest.xml中添加：图片选择界面和图片预览界面
```
    <activity android:name="me.iwf.photopicker.PhotoPickerActivity"
      android:theme="@style/Theme.AppCompat.NoActionBar"
       />

    <activity android:name="me.iwf.photopicker.PhotoPagerActivity"
      android:theme="@style/Theme.AppCompat.NoActionBar"/>
```

图片单张选择：多张选择只要设置PhotoCount
```
 // 选择相册，单张
                        PhotoPicker.builder()
                                .setPreviewEnabled(true)
                                .setGridColumnCount(3)
                                .setPhotoCount(1)
                                .setShowCamera(true)
                                .start(getActivity(), UserInfoFragment.this);
```

处理回调结果:
```
    @Override
    public void onActivityResult(int requestCode, int resultCode, Intent data) {
        if (resultCode == RESULT_OK) {

            if ((requestCode == PhotoPicker.REQUEST_CODE || requestCode == PhotoPreview.REQUEST_CODE)) {
                List<String> photos = null;
                if (data != null) {
                    photos = data.getStringArrayListExtra(PhotoPicker.KEY_SELECTED_PHOTOS);
                }
                ...
            }
     }
```

在PhotoPickerActivity和PhotoPagerActivity中，如有必要，我们可以对图片选择界面和预览界面进行调整

## 2.在库的基础上进行修改
在主工程app的modules目录下，在PhotoPicker库的基础上对逻辑和界面进行了一定的修改。

- 继承TSActivity和TSFragment，统一界面和代码风格。
- 修改相册界面的逻辑，将界面分成相册列表，图片列表，预览界面。
- PhotoPicker类的跳转目标发生变化，使用隐式跳转。

PhotoPicker的调用方法没有发生变化。
