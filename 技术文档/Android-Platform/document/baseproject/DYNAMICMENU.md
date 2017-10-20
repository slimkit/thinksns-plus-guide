2017年5月11日 11:38:45
# 动态工具栏类说明

## 1. 概述

  编写公共工具栏，方便后续开发快速引用

  - 支持自定义文字内容，文字图片，文字颜色
  - 支持无资源是是否占位
  - 支持样式

    ![动态详情工具栏](../image/dynamic_detail.png)

    ![动态详情工具栏](../image/dynamic_list.png)

## 2. 使用说明

1. extends DynamicDetailMenuView 自定义自己的资源类容
  ```java
public class MyView extends DynamicDetailMenuView {

    protected
    @DrawableRes
    int[] mImageNormalResourceIds = new int[]{
            R.mipmap.home_ico_good_normal,
            R.mipmap.home_ico_comment_normal,
            R.mipmap.detail_ico_share_normal,
            R.mipmap.home_ico_more
    };// 图片 ids 正常状态
    protected
    @DrawableRes
    int[] mImageCheckedResourceIds = new int[]{
            R.mipmap.home_ico_good_high,
            R.mipmap.home_ico_comment_normal,
            R.mipmap.detail_ico_share_normal,
            R.mipmap.home_ico_more
    };// 图片 ids 选中状态
    protected
    @StringRes
    int[] mTexts = new int[]{
            R.string.like,
            R.string.comment,
            R.string.share,
            R.string.more
    };// 文字 ids

    protected
    @ColorRes
    int mTextNormalColor = R.color.normal_for_disable_button_text;// 正常文本颜色
    protected
    @ColorRes
    int mTextCheckedColor = R.color.normal_for_disable_button_text;// 选中文本颜色

    public MyView(Context context) {
        super(context);
    }
}

new  MyView().setItemTextAndStatus(String string, boolean isChecked, int postion)；// 设置某个 Item 具体内容

  ```

***注意：*** Item 数量最多只能 4 个，多余的将自动过滤掉