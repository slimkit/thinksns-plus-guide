# ThinkSNS+ 服务器采购指南


### 一、服务器配置

1. 配置参数

	> 1. 用户注册数10万内，日活5000、并发500左右参考配置，如果数据大，请联系技术单独沟通配置
	> 2. 目前TS只支持[`阿里OSS`](https://www.alibabacloud.com/help/zh/doc-detail/31883.htm),如果使用` oss `服务，请务必使用[阿里云服务器](https://cn.aliyun.com/)
	
	| CPU| 带宽 | 系统盘 |  系统  |  并发量 |  日活 |
	|:-----:|:------:|:-----:|:----:|:----:|:--------:| 
	|   2核  8G   |  5M   |  50G     |  Centos7+     |   20左右 |  500 |
	|   8核  16G  |  10M  |  100G    |  Centos7+     | 500左右  | 5000 |

2. 端口设置

	- 22  ----- 服务器部署使用
	- 80  ------  http 网站接口访问
	- 443 -----  https 网站接口访问
	- 3306 -----  数据库访问
	- 8888  -----  宝塔面板访问

	
	
3. 服务器环境

	- PHP：PHP 7.2.5+
	- Composer：推荐使用最新版
	- 数据库：mysql 5.7.9+
	- 扩展开启要求：
		- OpenSSL PHP 拓展
		- PDO PHP 拓展
		- Mbstring PHP 拓展
		- Tokenizer PHP 拓展
		- XML PHP 拓展
		- Ctype PHP 拓展
		- JSON PHP 拓展
		- BCMath PHP 拓展
   - 系统：
		- Nginx  1.17
		- Centos 7.0+
 
### 二、数据库配置 【！！！与服务器务必在同一个区】

> 如果需要，且有预算，可以单独购买数据库服务器；如果预算有限，可以不买数据库服务器


### 三、域名https证书

> 有经费预算的，可以购买收费版，如果没有经费，可以直接申请免费版。
免费版需要***每3个月重新申请一个***


### 四、OSS服务【！！！与服务器务必在同一个区】
> 建议只要有图片、视频等媒体内容存在都使用 `oss` 服务,目前TS只支持 [阿里OSS](https://www.alibabacloud.com/help/zh/doc-detail/31883.htm)

`oss` 配置文档 [参考](./alioss_config.md)






















	