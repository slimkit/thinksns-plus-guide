2017年5月11日 11:45:14
# 基础浏览器说明

## 1. 概述

基于 WebView 封装的基础浏览器。支持
- 显示隐藏进度
- 单击和长按图片的响应事件
- 仿微信的多级关闭按钮
- 支持错误缺省图提示，单击重新加载
- 支持修改缺省图
- 支持网页缓存
- 支持模拟加载进度

## 2. 使用说明
1. 通过继承或者new 创建使用
```java
public class AboutUsFragment extends TSWebFragment {
...
}
// 或者
TSWebFragment mTSWebFragment=new TSWebFragment(){
    @Override
    protected void onWebImageClick(String clickUrl, List<String> images) {

    }

    @Override
    protected void onWebImageLongClick(String longClickUrl) {

    }
};

```
2. 显示隐藏进度
```java
mTSWebFragment.setNeedProgress(boolean needProgress);
```
4. 修改缺省图
```java
mTSWebFragment.setTipImage(int resId);
```