# ThinkSNS Plus 简易部署与配置文档


## 目录

* [部署前准备](#部署前准备)
* [服务器安装](#服务器安装)
    * 普通文本
* [PC安装](#PC安装)
* [H5安装](#H5安装)
* [基础配置](#基础配置)
	* 高德配置
* [CDN配置](#CDN配置)
	* 阿里云OSS
	* 七牛云
* [短信配置](#短信配置)
	* 阿里云
	* 云片
	
	
-----

### 部署前准备
- 服务器，建议CentOS 7.x
- 域名，解析到服务器公网ip

### 服务器安装
1. 安装宝塔面板
	```
	yum install -y wget && wget -O install.sh http://download.bt.cn/install/install.sh && sh install.sh
	```
2. 通过浏览器进入宝塔面板管理界面，地址默认为ip:8888，账户名密码安装完成会有提示

3. 安装运行环境
	- PHP >= 7.1.3

	>需开启PHP拓展：`fileinfo`, `openssl`, `exif`
	
	>需移除PHP禁用函数：`symlink`, `proc_open`, `exec`, `shell_exec`

	- MySQL >= 5.6
	- Nginx >= 1.10

	![image1](https://image.zhibocloud.cn/2018/06/14/0316/1Vhven5NfzXQOwYQUYBL5hT1feJORErsahzBhl7N.png)

4. 克隆代码并赋予777权限
	```
	git clone https://github.com/xxxxxx plus
	chmod -R 777 plus
	```
	
5. 创建站点并创建数据库

	![](https://image.zhibocloud.cn/2018/06/14/0837/GHvZ2KSVLalgJBstrqm17VVIbQH2FJyzwV30mw8x.png)

	> 域名为准备好的域名
 
	> 根目录地址为/www/wwwroot/plus

	![](https://image.zhibocloud.cn/2018/06/14/0314/ysEMnz9vfmsMLFBosqkKMy9lQd6HvyfOSF8pGZt0.png)
	
	> 修改站点伪静态规则

6. 创建数据库

    ![](https://image.zhibocloud.cn/2018/06/14/0641/GKifET1ZGKnYU31btK0ztLWjnzG5wVbbTqUoD18m.png)

7. 安装程序

	程序根目录执行一下命令
	
	```
	php -r "file_exists('.env') || copy('.env.example', '.env');"
	composer install 
	php artisan key:generate
	```

	浏览器访问域名/installer进入安装程序，按照步骤及提示，依次执行完成安装
	
8. 发布静态资源并链接到public目录

	```
	php artisan vendor:publish --all
	php artisan storage:link
	```
	
### PC安装

1. 克隆PC仓库代码到packages目录
2. 编辑根目录下的composer.json，找到json对象中的「repositories」属性，新增PC信息，找到「require」属性，新增PC依赖

    ```
    {
        "type": "path",
        "url": "packages/plus-componet-pc",
        "options": {
            "symlink": true,
            "plus-soft": true
        }
    }
    ```
    
    ```
    {
    ...
    "require": {
        ...
        "zhiyicx/plus-component-pc": "^3.0.0"
    }
    ```
}

3. 修改PC包中的composer.json, 新增version版本号

   ```
   {
        ...
        "require": {
            "overtrue/socialite": "^2.0",
            "gregwar/captcha": "1.*"
        },
        "version": "3.0.1"
    }
   ```
4. 执行`composer update`加载PC
5. 执行PC包命令
   ```
   php artisan package:handle pc install
   ```
6. 若需要经常修改静态资源，建议执行软链命令
   ```
   php artisan package:handle pc link
   ```
   
### H5安装
1. 安装[yarn](https://yarn.bootcss.com/docs/install.html#linux-tab)或npm
2. 克隆H5仓库代码到任意目录
3. 修改.env.example文件名为.env并修改其中配置

    |  | 描述 |
    |-----|-----|
    | BASE_URL | 项目部署的基础路径，我们默认假设你的应用将会部署在域名的根部，比如 `https://www.plus.com` 如果你的应用时部署在一个子路径下，那么你需要在这里指定子路径。比如，如果你的应用部署在 `https://www.plus.com/h5/` 那么将这个值改为 `/h5/` |
    | GENERATE_CSS_MAP | 编译静态资源的时候是否生成 `sourceMap` |
    | VUE_APP_API_HOST | 你的 Plus 程序网址 |
    | VUE_APP_API_VERSION | 你要使用的 Plus 程序 API 版本号，目前固定为 `v2` |
    | VUE_APP_NAME | 你的程序名字 |
    | VUE_APP_KEYWORDS | 你的程序关键词，使用英文半角 `,` 符号分割多 |
    | VUE_APP_DESCRIPTION | 你的程序描述 |
    | VUE_APP_ROUTER_MODE | 前端路由使用的路由模式，支持 `hash` 和 `history` 两种模式，具体请参考👉 [`vue-route mode`](https://router.vuejs.org/zh-cn/api/options.html#mode) |
    
    > 以上信息配置完成后，你运行 `yarn install` 或者 `npm install` 进行依赖安装！
4. 运行 `yarn build` 或者 `npm run build` 进行编译，编译完成后会生成/dist目录
5. 在/www/wwwroot/plus/public目录中创建h5目录，并且将dist目录内的内容复制到其中，即可通过域名/h5访问

### 基础配置

#### 高德配置(附近的人)
1. 注册[高德](http://lbs.amap.com)账号，并成为开发者用户
2. [创建应用](http://lbs.amap.com/dev/key/app)
3. 添加KEY1
4. 添加KEY2
5. [创建自定义地图](http://yuntu.amap.com/datamanager/)
6. 将KEY1中的KEY，密钥，自定义地图中的table_id填写至后台
7. 若有H5拓展包，将KEY2中的KEY填写至H5根目录.env中的VUE_APP_LBS_GAODE_KEY上
![image](https://image.zhibocloud.cn/2018/07/02/0904/LnFuiPPy6lRYT6t9d0ImXVVtCktcGNXITjC5yKEb.gif)

### CDN配置
#### 阿里云OSS
1. 加载拓展包`composer require aliyuncs/oss-sdk-php`
2. 注册账号，进入[阿里云OSS](https://oss.console.aliyun.com/overview)管理面板
3. 新建Bucket
4. 创建镜像源
5. 创建密钥
6. 填写相关内容至后台
![image](https://image.zhibocloud.cn/2018/07/03/0232/S8tbkSK2E2cXGkN2egPybL6xRxY3FLksOjSV3X4G.gif)

#### 七牛云
1. 注册账号，进入[七牛云对象存储](https://portal.qiniu.com/bucket/plus-demo/index)管理面板
2. 新建存储空间
3. 创建镜像源
4. 创建密钥
5. 填写相关内容至后台
![image](https://image.zhibocloud.cn/2018/07/03/0247/4ZdIt2sAZhwU3ivyDSxPmHpiACbbREOMzJN0AS5F.gif)

### 短信配置
#### 阿里云
1. 注册账号，进入[阿里云短信服务](https://dysms.console.aliyun.com/dysms.htm#/overview)管理面板
2. 应用开发-接口调用中，获取AK
3. 应用开发-签名管理，创建签名
4. 应用开发-模板管理，添加模板
5. 将相关信息填写至后台
![image](https://image.zhibocloud.cn/2018/07/03/0303/dvoJ2N4vEgOBNbEUAYMRyI6TYinRKZ96cYusZtre.gif)

#### 云片
1. 注册账号，进入[云片](https://www.yunpian.com/admin/main)管理面板
2. 查看API KEY
3. 创建签名
4. 创建模板
5. 将相关信息填写至后台（步骤与阿里云大致相同）
