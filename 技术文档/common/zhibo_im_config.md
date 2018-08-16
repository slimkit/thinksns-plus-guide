# <p align = "center" >智艺IM部署文档</p>





### 部署要求： 

1. 只支持 `linux`
2. `PHP` 版本为 `5.6.x`  需要支持 `pdo_mysql/swoole(1.9.x)/msgpack(0.5.7)/redis(2.2.8)`
3. `mysql` 版本大于 `5.5` 
4. `redis` 版本大于 `3`
5. 需要 `php composer` 支持

### 扩展下载

- `Msgpack` [下载地址](http://pecl.php.net/get/msgpack-0.5.7.tgz)
- `redis`  [下载地址](http://pecl.php.net/get/redis-2.2.8.tgz)
- `Swoole`  [下载地址](http://pecl.php.net/get/swoole-1.9.6.tgz)

安装过程省略，太基础了。。。。。

### 安装IM

1. 新建一个安装目录并进入如：

	```shell
	mkdir -p /usr/local/im && cd /usr/local/im 
	```
2. 同步代码：请[联系官方](http://www.thinksns.com/)

3. 安装依赖

	```shell
	composer install

	```
4. 新建一个软连接

	```shell
	ln -s /usr/local/im/bin/phpwebim /usr/local/bin/webim
	```


5. 新建一个配置目录并进入如：

	```shell
	mkdir -p /home/wwwroot/tsplus && cd /home/wwwroot/tsplus
	```

6. 初始化配置文件：

	```shell
	webim init
	```

7. 编辑配置文件：
	
	```shell
	vi phpwebim.php
	```	
	> 其中 `redis` 和 `mysql` 必须配置。<font color=red >**注意**</font> 多个 `IM` 服务中不要使用相同的 `redis/mysql/listen_port` 配置 
	
	> `admins` 中配置管理员账号密码列表。`reactor_num` 和 `worker_num` 可以配置为当前服务器 `cpu` 数量。 其他配置项可以不用管。<font color=red >**注意去掉前面#取消注释**</font>

8. 确认配置无误后，执行命令安装数据库

	```shell
	webim install
	```
	
9. 安装完成后即可启动

	```shell
	webim start
	
	```
	
10. 这样可以加入到开启启动 

	```shell
	webim start -c /home/wwwroot/tsplus/phpwebim.php
	```

