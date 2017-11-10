# TS+ iOS端应用配置

* 目录
	* [简单界面调整](#简单界面调整)
	* [图片素材替换](#图片素材替换)
	* [分享内容配置]#(暂无)


## 简单界面调整

本应用的UI设计是按照的`ThinkSNS Plus Plus 视觉规范`.颜色记录在`~/CustomUIKit/TSUserInterfacePrinciples/TSColor.swift`,字号记录在`~/CustomUIKit/TSUserInterfacePrinciples/TSFont.swift`,部分通用控件如弹窗记录在`~/CustomUIKit/`文件夹内.

### 应用名调整

修改 `~/SupportFile/InfoPlist.strings` 文件内的 CFBundleDisplayName 值.**注意**此处有两个文件, 分别对应英文设备和中文设备下应用的名称.

## 图片素材替换

本工程所有图片素材记录在`~/Assets.xcassets/`内.将新的图片生成旧图片一样的规格一致的文件名称后,将新图片覆盖对应文件夹内的图片即可.

### 启动图替换

启动图覆盖 `~/Assets.xcassets/loading` 的图片(请勿修改图片名称),同时修改 `~/SupportFile/LaunchScreen.storyboard` 图片以及底部文字.

## 分享内容配置

目前本应用可分享到QQ空间,QQ好友,新浪微博以及微信朋友圈和微信好友四处,分享内容包括文字,图片,视频,链接等.(详情查看对应平台文档说明)
