# PC 、H5 三方申请及配置文档

## 目录

* [腾讯QQ互联](#腾讯qq互联)
* [微信](#微信)
* [微信服务号](#微信服务号)
* [新浪微博](#新浪微博)
* [短信等其他](#短信等其他)
* [微信微博QQ回调配置](#微信微博qq回调配置)

### 腾讯QQ互联

1. 费用说明：免费
2. 申请账号及应用创建
	
	> 步骤1：注册账号，[注册地址](https://connect.qq.com/)
	> 
	> 步骤2：认证 </br>    必须申请成为个人开发者或者企业开发者；企业开发者拥有更高的使用权限，这个影响不大。</br>
	[认证地址](https://connect.qq.com/devuser.html#/create/1/)</br> [认证步骤详细说明](http://wiki.connect.qq.com/%E6%88%90%E4%B8%BA%E5%BC%80%E5%8F%91%E8%80%85 )

	> 步骤3：创建应用，获得对应的appid与appkey </br> 登录后在管理中心创建应用并获取App Id、与 APP Key [地址](http://op.open.qq.com/manage_centerv2/android?owner=335891510&uin=335891510) </br>
	[操作步骤详细说明](http://wiki.connect.qq.com/__trashed-2) </br>

3. 管理后台配置

	将对应的appid与appkey填入后台</br>  ![](https://image.zhibocloud.cn/2018/07/05/0650/uMeyYQXxcI39bxVXLwwoRJ4H6aA9sb6a1eaUuiTw.jpeg)
 
 
### 微信
1. 费用说明：使用免费，认证费用300CNY/年
2. 注册账号及应用创建

	> 步骤1：注册微信开放平台[注册地址](https://open.weixin.qq.com )</br>
	
	> 步骤2：开发者认证</br> 必须申请成为个人开发者或者企业开发者；企业开发者拥有更高的使用权限，这个影响不大。</br>
	[认证地址](https://open.weixin.qq.com/verify)
	[认证需要资料](https://open.weixin.qq.com/verify)
	
	> 步骤3：创建应用，获取App Id、与 APP Key </br>
	登录后在管理中心创建应用并获取App Id、与 APP Key [地址](https://open.weixin.qq.com/cgi-bin/applist?t=manage/list&lang=zh_CN&token=8f562b868d3d6120b8f09130c668a20f68b4afb0) </br>
	[文档中心](https://open.weixin.qq.com/cgi-bin/showdocument?action=dir_list&t=resource/res_list&verify=1&lang=zh_CN)
3. 管理后台配置
将对应的appid与appkey填入后台</br> ![](https://image.zhibocloud.cn/2018/07/05/0718/ZiWxpzCvgve0m3HDJ0VtXglBmylNSR5skJjuEwLW.jpeg) 


### 微信服务号

<font color=red >**注意**：如果H5端，需要接入微信服务号（公众号/订阅号）进行使用，需要h5的微信授权登录，则需要申请此项。</font></br>

1. 费用说明：使用免费，认证费用300CNY/年
2. 注册账号及认证
	
	> 步骤1：注册账号，[注册地址](https://mp.weixin.qq.com/cgi-bin/registermidpage?action=index&lang=zh_CN)</br> ![](https://image.zhibocloud.cn/2018/07/05/0720/4AlHlr8yxevpCGjjCz5HXuK55afSaXiVZtY508HF.jpeg)
	[注册流程指引](https://kf.qq.com/product/weixinmp.html#hid=87)
	
	> 步骤2：微信认证
	[认证步骤指引](https://kf.qq.com/faq/161220Brem2Q161220uUjERB.html)
	
	> 步骤3：获取App Id、与 APP Key </br>
	在左侧找到“开发”条目下的“基本配置”并点击，如图所示：</br>
	![](https://image.zhibocloud.cn/2018/07/05/0722/oEnTJBnvi7K3PtJ34b7jJfIPMnvF6ts1C0YbTsBu.jpeg)
	![](https://image.zhibocloud.cn/2018/07/05/0722/OcL1kO0TqgsJ27uo4cVVNGDFqoc2hzHcJeRQ0meG.jpeg)
	
	> 步骤4：调试微信网页授权
 	```
	https://mp.weixin.qq.com/wiki?t=resource/res_main&id=mp1455784140
	```
	
	
### 新浪微博

1. 费用说明：免费
2. 申请账号及创建应用

	> 步骤1：注册账号，[注册地址](http://weibo.com/signup/signup.php) 

	> 步骤2：开发者认证
		必须申请成为个人开发者或者公司开发者；公司开发者可以创建“微应用”。TS没有使用“微应用”服务。个人开发者后期可以成为公司开发者。
		[认证地址](http://open.weibo.com/developers/basicinfo?dev_type=1)</br>
		[认证需要资料](http://open.weibo.com/developers/basicinfo?dev_type=1)
		
	> 步骤3：创建应用，获取App Id、与 APP Key 
		登录后在我的应用-管理中心，创建应用并获取AppId、SecretKey,[地址](http://open.weibo.com/developers)</br>
		[文档中心](http://open.weibo.com/wiki/%E9%A6%96%E9%A1%B5)
3. 管理后台配置
将对应的appid与appkey填入后台</br> ![](https://image.zhibocloud.cn/2018/07/05/0719/XtqJbjNtPcbndPtfdlxzYL2SI8KIDOZ0PNNWu0dh.jpeg)



### 短信等其他
其他山房请查看 [简易部署与配置文档](https://github.com/slimkit/thinksns-plus-guide/blob/master/技术文档/server/thinksnsPlusSimpleDeploymentDoc.md)

### 微信微博QQ回调配置

```
QQ网站回调域：绑定域名/socialite/qq/callback如：http://tsplus.zhibocloud.cn/socialite/qq/callback微信开放平台账号授权回调域：绑定域名即可如：tsplus.zhibocloud.cn微博开放平台账号应用地址：绑定域名如：http://tsplus.zhibocloud.cn安全域名：可填写多个，必须把绑定域名填写在其中，去掉http://或者https://如：tsplus.zhibocloud.cn
```