# 产品使用指南


<p align="center"><img src="https://tsplus.zhibocloud.cn/assets/pc/images/logo.png"></p>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`ThinkSNS+`打造社交运营为核心的应用级软件系统源码及定制开发服务。支持动态拓展应用的接入和UI规范设计。更多关于`ThinkSNS+`请访问[ThinkSNS+](http://www.thinksns.com/)。</br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`ThinkSNS+` 产品使用指南,记录`TS+`全部产品使用手册等内容,以及相关技术文档。如果有任何问题需要反馈,请联系`TS+`商务人员.

## 目录
- [代码交付说明](#代码交付说明)
- [代码分支说明](#代码分支说明)
- [部署安装、素材替换与打包所需资料说明](#部署安装、素材替换与打包所需资料说明)
- [服务文档资料](#服务文档资料)
- [产品使用文档](#产品使用文档)
- [产品技术文档](#产品技术文档)
- [常见问题](#常见问题)



----

### 代码交付说明

购买授权的客户请提供 [github](https://github.com/) 用户名以及项目的中英文名称.(缺少英文名称的客户可提供域名/拼音等)用于接收最新版本的`ThinkSNS Plus`系列产品源码.


### 代码分支说明

1. 统一前缀 `tsauth/`	
2. 大版本分支统一使用数字加小数点形式，例如：`tsauth/thinksns3`、`tsauth/1.8`
3. 当前2.x版本为 `tsauth/master`,3.x版本为 `tsauth/thinksns3` 直播版为 `tsauth/master-live`，小程序`tsauth/miniprogram`
4. 以上分支统一称为授权分支，如果客户修改了授权分支的代码，最新代码统一在授权分支后追加 `-backup`,例如：
	
	```
	tsauth/master 		 -->  tsauth/master-backup
	tsauth/master-live   -->  tsauth/master-live-backup
	tsauth/miniprogram   -->  tsauth/miniprogram-backup
	
	```
	
> **备注**：
> 
	- 客户使用时需要新建自己的分支，以上分支仅用于接收 `TS+` 最新源码,否则可能会无法接受到新版本更新内容。
	- 默认2.x的版本为 `tsauth/master` 分支; 3.x的版本推送分支为`tsauth/thinksns3`。

### 部署安装、素材替换与打包所需资料说明

便于快速部署、素材替换与打包请提供 [必须内容](./deploy-package-res/README.md)



----


### 服务文档资料

* [产品介绍](https://thinksns.com/)
* [企业及应用案例介绍](https://thinksns.com/case.html)
* [开源授权协议预览](https://github.com/slimkit/plus/blob/master/LICENSE)

### 产品使用文档

#### 2.x
* [移动端使用手册](./thinksns2/app端使用手册.pdf)
* [后台使用手册](./thinksns2/管理后台使用手册.pdf)
* [PC端使用手册](./thinksns2/PC端使用手册.pdf)
* [H5使用手册](./thinksns2/H5端使用手册.pdf)
* [ThinkSNS+2.3功能清单列表](./thinksns2/ThinkSNS_Plus_V2.3功能清单列表.xlsx)

#### 3.x

* [IOS版使用手册](./thinksns3/IOS端使用手册.docx)
* [Android版使用手册](./thinksns3/Android端使用手册.docx)
* [微信小程序版使用手册](./thinksns3/微信小程序使用手册-231204.pdf)
* [管理后台使用手册](./thinksns3/后端使用手册.docx)
* [技术概要说明](./thinksns3/ThinkSNS-plusV3.pdf)
* [管理后台高德IP定位配置说明](./thinksns3/amap/amapconfig.md)
* [ThinkSNS+3.0功能清单列表](./thinksns3/function-list/function-list.md)

### 产品技术文档

* 通用

	* [PC 、H5 三方申请及配置文档](./技术文档/common/pc-h5-third-config.md)
	* [微信登录配置文档](./技术文档/common/wx-login-config.md)
	* [原生支付配置文档](./技术文档/common/plus-pay-config.md)
	* [广告图大小规则说明文档](./技术文档/common/ADVERT_DES.md)
	* [产品UI设计规范](./技术文档/common/README.md)
	* [ThinkSNS+ 服务器采购指南.md](./技术文档/common/ThinkSNS+guide_for_server.md)
	* [ThinkSNS+ OSS配置说明](./技术文档/common/alioss_config.md)

	
#### 2.x

* 服务端
	
	* [2.0+详细部署文档](https://slimkit.github.io/plus/guide/installation/install-plus.html#%E4%B8%8B%E8%BD%BD-plus-%E7%A8%8B%E5%BA%8F)
	* [简易部署与三方配置文档](./技术文档/server/thinksnsPlusSimpleDeploymentDoc.md)
	* [ThinkSNS Plus直播服务器配置文档](./技术文档/common/live_server_config.md)
	* [直播IM部署文档](./技术文档/common/zhibo_im_config.md)
   * [二次开发快速入门](https://slimkit.github.io/docs/server-guides-package.html)
	* [服务端 Api 文档](https://slimkit.github.io/docs/api-v2-overview.html)
	* [服务端数据字典](https://slimkit.github.io/docs/data-fields.html)
	* [性能概述](./技术文档/server/performance.md)
	* [开发教程](https://slimkit.github.io/plus/guide/dev/blog/)
	
	

* iOS端
	* [iOS端开发须知](./技术文档/iOS端/README.md)
	* [iOS端二次开发文档](./技术文档/iOS端/Thinksns%20Plus%20Document)
	* [iOS端二次开发三方与素材替换说明文档](./技术文档/iOS端/TS+%20iOS端应用配置.md)
	* [IOS上架重要说明](./技术文档/iOS端/ThinkSNS-Plus-AppStore-Review-v1.0.md)
	* [iOS端打包操作指导](http://www.jianshu.com/p/9df7d8930a3e)
	* [SSL证书类型说明](./技术文档/iOS端/SSL证书类型说明.md)
	* [iOS端二次开发常见问题集](./技术文档/iOS端/iOS端二次开发常见问题集.md)

* Android端
	* [Android端开发须知概述](./技术文档/Android-Platform/README.md)
	* [Android端二次开发文档](./技术文档/Android-Platform/document/tutorial/SecondaryDevelopmentTutorial.md)
	* [Android端二次开发三方与素材替换说明文档](./技术文档/Android-Platform/document/tutorial/SecondaryDevelopmentResourceReplaceTutorial.md)
	* [Android签名文件](./技术文档/Android-Platform/document/tutorial/AndroidCreateSignatureFileTutorial.md)
	* [Android端打包操作指导](./技术文档/Android-Platform/document/tutorial/AndroidPackageTutorial.md)
	* [Android端API接口文档](./技术文档/Android-Platform/document/app/API.md)
	* [第三方账号申请指导](./技术文档/Android-Platform/document/tutorial/AppPackageInfoTutorial.md)
	
* PC端
	* [PC端开发概述](./技术文档/PC/README.md)


* H5端
	* [H5端开发概述](https://github.com/zhiyicx/plus-component-h5/blob/master/README.md)
	* [H5开发者手册](https://github.com/slimkit/plus-small-screen-client/blob/master/CONTRIBUTING.md)
	* [H5目录结构说明](./技术文档/H5/CONSTRUCT.md)


#### 3.x

* 服务端
	* [线上文档仓库](https://zhiyicx.github.io/ts-api-docs/)
		* [部署安装](https://zhiyicx.github.io/ts-api-docs/guide/installation/)
		* [数据字典](https://zhiyicx.github.io/ts-api-docs/data-fields/) 
		* [Api接口](https://zhiyicx.github.io/ts-api-docs/api-v2/passport/) 
	* 授权仓库
		* 查看源码仓库 `README.md`与 `docs` 文件夹

* iOS端
	* 查看源码仓库 `README.md`与 `Document` 文件夹

* Android端
	* 查看源码仓库 `README.md`与 `document` 文件夹

* 小程序端
	* [打包说明文档	](./技术文档/miniprogram/miniprogram_package.md) 
	* 查看源码仓库 `README.md`


部署打包，请参考`2.x`

----


### [常见问题](./questions/ThinkSNSPlusHelp.md) 

* [服务端](https://github.com/slimkit/thinksns-plus-guide/issues?q=is%3Aopen+is%3Aissue+label%3APHP)
* [iOS](https://github.com/slimkit/thinksns-plus-guide/issues?q=is%3Aopen+is%3Aissue+label%3AIOS)
* [Android](https://github.com/slimkit/thinksns-plus-guide/issues?q=is%3Aopen+is%3Aissue+label%3AAndroid)
* [PC](https://github.com/slimkit/thinksns-plus-guide/issues?q=is%3Aopen+is%3Aissue+label%3APC)
* [H5](https://github.com/slimkit/thinksns-plus-guide/issues?q=is%3Aopen+is%3Aissue+label%3AH5)
