2017年5月11日 11:45:38
# uCrop图片裁剪开源库

该开源库包含以下丰富的功能，基本满足任何场景的需求：
- 前景裁剪框可放缩，移动
- 被裁剪图片可缩放，旋转，移动
- 高度可定制的样式，比如蒙层颜色，背景颜色，裁剪框。。。
- 圆形裁剪，方形裁剪，任意比例的矩形裁剪
- 裁剪图片质量压缩<br>
 。。。

[github中的地址](https://github.com/Yalantis/uCrop)


## 1.如何使用

以module的形式导入uCrop库，方便界面的修改

在baseProject的AndroidManifest.xml中,添加裁剪页面：
```
        <!--图片裁剪：ucrop-->
        <activity
            android:name="com.yalantis.ucrop.UCropActivity"
            android:screenOrientation="portrait"
            android:theme="@style/Theme.AppCompat.Light.NoActionBar"/>

```

调用裁剪方法：下面方法列出了一些配置
```
    /**
     * 调用裁剪方法
     *
     * @param uri
     */
    private void startCropActivity(@NonNull Uri uri) {
        String destinationFileName = "SampleCropImage.jpg";
        UCrop uCrop = UCrop.of(uri, Uri.fromFile(new File(getActivity().getCacheDir(), destinationFileName)));
        uCrop.withAspectRatio(1, 1);//方形
        UCrop.Options options = new UCrop.Options();
        options.setCompressionFormat(Bitmap.CompressFormat.JPEG);
        options.setCompressionQuality(100);    // 图片质量压缩
        options.setCircleDimmedLayer(false); // 是否裁剪圆形
        options.setHideBottomControls(true);// 是否隐藏底部的控制面板
        options.setCropFrameColor(Color.WHITE);// 设置内矩形边框线条颜色
        options.setShowCropGrid(false);// 是否展示内矩形的分割线
        options.setDimmedLayerColor(Color.argb(0xbb,0xff,0xff,0xff));// 设置蒙层的颜色
        options.setRootViewBackgroundColor(Color.WHITE);// 设置图片背景颜色
        options.setToolbarColor(ContextCompat.getColor(getContext(),R.color.themeColor));
        uCrop.withOptions(options);
        uCrop.start(getActivity(), UserInfoFragment.this);
    }
```

处理裁剪结果：
```
    @Override
    public void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);

        if (resultCode == RESULT_OK) {
            if (requestCode == UCrop.REQUEST_CROP) {
               Uri resultUri = UCrop.getOutput(result);
                // 处理正确的结果
            }

            if (resultCode == UCrop.RESULT_ERROR) {
                Throwable cropError = UCrop.getError(result);
                // 处理失败的结果
            }

        }
    }
```

要注意uCrop.start(getActivity(), UserInfoFragment.this) 方法;跳转裁剪页面，当前页面是activity还是fragment，
在fragment中，以activity启动startActivity,是无法接受到onActivityResult结果的。

## 2.在库的基础上进行修改
在主工程app的modules目录下，在UCROP库的基础上对逻辑和界面进行了一定的修改。

- 继承TSActivity和TSFragment，统一界面和代码风格。
- 抽取库的裁剪界面核心逻辑，在CropFragment中实现核心功能。
- 修改界面布局，样式，包括蒙层，背景，toolbar。。。
- UCrop类的跳转目标发生变化，使用隐式跳转。

当前已经将UCrop的裁剪功能，封装到[图片选择器](PHOTOSELECTOR.md)
