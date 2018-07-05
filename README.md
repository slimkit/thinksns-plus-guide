# 产品使用指南


<p align="center"><img src="http://orkxkkozv.bkt.clouddn.com/plus-logo.png"></p>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`ThinkSNS+`打造社交运营为核心的应用级软件系统源码及定制开发服务。支持动态拓展应用的接入和UI规范设计。更多关于`ThinkSNS+`请访问[ThinkSNS+](http://www.thinksns.com/index.html)。</br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`ThinkSNS+` 产品使用指南,记录`TS+`全部产品使用手册等内容,以及相关技术文档。如果有任何问题需要反馈,请联系`TS+`商务人员.

## 目录
- [代码交付说明](#代码交付说明)
- [代码分支说明](#代码分支说明)
- [服务文档资料](#服务文档资料)
- [产品使用文档](#产品使用文档)
- [产品技术文档](#产品技术文档)



----

### 代码交付说明

购买授权的客户请提供 [github](https://github.com/) 用户名以及项目的中英文名称.(缺少英文名称的客户可提供域名/拼音等)用于永久接收最新版本的`ThinkSNS Plus`系列产品源码.



### 代码分支说明
	
1. 统一前缀 `tsauth/`	
2. 大版本分支统一使用数字加小数点形式，例如：`tsauth/1.7`、`tsauth/1.8`
3. 当前最新版本为 `tsauth/master`, 直播版为 `tsauth/master-live`
4. 以上分支统一称为授权分支，如果客户修改了授权分支的代码，最新代码统一在授权分支后追加 `-backup`,例如：
	
	```
	tsauth/master 		 -->  tsauth/master-backup
	tsauth/master-live   -->  tsauth/master-live-backup
	
	```
> **备注**：客户使用时需要新建自己的分支，以上分支仅用于接收 `TS+` 最新源码。

### 服务文档资料

* [产品介绍]
* [企业及应用案例介绍](http://www.thinksns.com/data/upload/ueditor/20171031/59f758931dab4.pptx)
* [开源授权协议预览](http://www.thinksns.com/data/upload/ueditor/20171031/59f75808623e0.pdf)

### 产品使用文档

* [移动端使用手册](http://www.thinksns.com/data/upload/ueditor/20171101/59f96170569dd.pdf)
* [后台使用手册](http://www.thinksns.com/data/upload/ueditor/20171101/59f961d7a15a5.pdf)
* [PC端使用手册](http://www.thinksns.com/data/upload/ueditor/20171101/59f961c19a9d7.pdf)
* [H5使用手册](http://www.thinksns.com/data/upload/ueditor/20171101/59f961980fbe8.pdf)

### 产品技术文档

* 通用
	* [产品UI设计规范]()
	* [移动端打包上线所需资料说明](./技术文档/Android-Platform/document/tutorial/AppPackageInfoTutorial.md)
	* [PC 、H5 三方申请及配置文档](./技术文档/common/pc-h5-third-config.md)

* 服务端
	* [服务端文档](https://slimkit.github.io/)
	* [简易部署与配置文档](./技术文档/server/thinksnsPlusSimpleDeploymentDoc.md)
	* [服务端旧版部署视频](http://www.thinksns.com/reader/100.html)
	* [服务端 Api 文档](https://slimkit.github.io/docs/api-v2-overview.html)
	* [服务端数据字典](https://slimkit.github.io/docs/data-fields.html)
	

* iOS端
	* [iOS端开发须知](./技术文档/iOS端/README.md)
	* [iOS端二次开发文档](./技术文档/iOS端/Thinksns%20Plus%20Document)
	* [iOS端应用配置](./技术文档/iOS端/TS+%20iOS端应用配置.md)
	* [APP Store上架指导](./技术文档/iOS端/APPStore上架指导.md)
	* [iOS端打包操作指导](http://www.jianshu.com/p/9df7d8930a3e)
	* [SSL证书类型说明](./技术文档/iOS端/SSL证书类型说明.md)
	* [iOS端二次开发常见问题集](./技术文档/iOS端/iOS端二次开发常见问题集.md)

* Android端
	* [Android端开发须知概述](./技术文档/Android-Platform/README.md)
	* [Android端二次开发文档](./技术文档/Android-Platform/document/tutorial/SecondaryDevelopmentTutorial.md)
	* [BUG-友盟统计](https://passport.umeng.com/signup)
	* [第三方账号申请指导](./技术文档/Android-Platform/document/tutorial/AppPackageInfoTutorial.md)
	* [Android端素材替换文档](./技术文档/Android-Platform/document/tutorial/SecondaryDevelopmentResourceReplaceTutorial.md)
	* [Android签名文件](./技术文档/Android-Platform/document/tutorial/AndroidCreateSignatureFileTutorial.md)
	* [Android端打包操作指导](./技术文档/Android-Platform/document/tutorial/AndroidPackageTutorial.md)
	* [Android端API接口文档](./技术文档/Android-Platform/document/app/API.md)

* PC端
	* [PC端开发概述](./技术文档/PC/README.md)


* H5端
	* [H5端开发概述](https://github.com/zhiyicx/plus-component-h5/blob/master/README.md)
	* [H5开发者手册](https://github.com/zhiyicx/plus-component-h5/blob/master/DOCUMENT.md)
