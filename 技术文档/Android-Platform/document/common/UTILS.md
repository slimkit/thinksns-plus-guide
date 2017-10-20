2017年5月11日 14:07:03
# 常用工具类集合
 - imageloader文件夹：项目所使用的图片加载框架接口

  ImageConfig：图片加载配置信息类
  ImageLoaderStrategy：图片加载替换接口，替换图片加载框架
  ImageLoader类：图片加载使用类
  core文件夹下：当前实现了Glide的图片加载策略

 - [LogUtils](LOG.md):log信息工具类，封装了logger第三方库

 - CloseUtils：IO关闭工具类

 - ConvertUtils:一些转换方法：数据显示专函，包括时分秒，存储单位，stream和byte转换，drawable转换，像素转换。。。

 - DrawableProvider：drawable辅助类：图片压缩，尺寸变化，旋转。。。

 - FastBlur：毛玻璃效果工具类

 - FileUtils：文件辅助类，包含文件读写，文件信息获取等相关方法

 - StausBarUtils：状态栏适配的工具类

 - ZipHelper：压缩和解压的辅助类

 - [ColorPhrase](#ColorPhrase)： 文字颜色修改工具

 - widget文件夹：下面放着各种常用的和自定义的控件


 ## 使用说明

 ### <span id = "ColorPhrase">ColorPhrase</span>
 ```java
       String content = "<" + data.mUserInfo.uname + ">" + " : " + data.msg.txt;
         try {
             CharSequence chars = ColorPhrase.from(content).withSeparator("<>").innerColor(0xff64d7fe).outerColor(0xffffffff).format();
             mContentTV.setText(chars);
         } catch (Exception e) {
             e.printStackTrace();
             mContentTV.setText(data.mUserInfo.uname + " : " + data.msg.txt);
         }
 ```


