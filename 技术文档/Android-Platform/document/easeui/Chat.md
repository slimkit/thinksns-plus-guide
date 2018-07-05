2018-7-5 10:07:55 update by Jliuer@aliyun.com
# 项目中的环信

##  1.概述
1. 项目中聊天使用的是环信3.X，继承了easeui 库进行修改开发
2. [环信官方文档](http://docs.easemob.com/im/200androidclientintegration/50singlechat)

## 2.关于 x86 类型 cpu 以及3D地图支持说明
* 为了减少包体积，项目中使用2D地图，如果需要用到3D地图，请将 baseproject.gradle 中 api dataDependences.a2Dmap 替换为 api dataDependences.a3Dmap，重新编译后修改相关文件
>1. 移除 LookLocationFragment 和 SendLocationFragment 中 com.amap.api.maps2d.* 的相关包，重新导入3d地图对应包
>2. 将 布局文件 fragment_look_location 和 fragment_chat_send_location 中com.amap.api.maps2d.MapView 替换为3d对应view

* 为了减少包体积，已经移除x86类型 so 文件，如果需要用到这些文件，请将 easeui 这个目录下 ‘x86’文件夹剪切至 easeui 目录 src\main\jniLibs 中

