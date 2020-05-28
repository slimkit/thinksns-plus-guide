## OSS 配置说明


#### 一、基础配置


##### 1. 创建 Bucket

1. 读写权限:	`公共读`
2. 服务器端加密： `无`
3. 其他可使用默认，也可以更具自己的需求选择调整


##### 2. Bucket 配置

1. 增加一条跨域规则
	- 来源  `*`
	- 允许 Methods `全部勾选`
	- 允许 Headers  `*`
	- 缓存时间（秒） `0`
	- 返回 Vary：Origin `勾选`

2. 添加一条镜像回源规则，需要处理的如下，其他忽略

	- 回源类型  `镜像`
	- 回源地址的三个部分 `1. https`   `2. 域名`  `3. storage`
	- 3xx请求响应策略 `勾选跟随源站重定向请求`
	![image](https://user-images.githubusercontent.com/7939686/82892238-b553ff80-9f81-11ea-836a-b5d9b6bd5b08.png)
	

##### 3. OSS管理后台配置说明


1. 登录阿里云管理后台，进入 `对象存储OSS`模块，查看对应创建的 `Bucket`
<img width="1360" alt="document_image_rId13" src="https://user-images.githubusercontent.com/7939686/83110966-71ced200-a0f6-11ea-8c00-ff8b581506d1.png">

2. 2.x 版本管理后台配置
<img width="768" alt="document_image_rId14" src="https://user-images.githubusercontent.com/7939686/83111049-932fbe00-a0f6-11ea-92aa-f21d7548804d.png">
<img width="812" alt="document_image_rId15" src="https://user-images.githubusercontent.com/7939686/83111069-9e82e980-a0f6-11ea-8c92-a6d996dfd18e.png">


3. 3.x 版本管理后台配置
<img width="925" alt="document_image_rId16" src="https://user-images.githubusercontent.com/7939686/83111150-bd817b80-a0f6-11ea-95b1-dfd236bbbe3d.png">

