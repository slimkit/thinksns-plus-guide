# 音乐FM

#### 一. 概述标签：
该模块儿主要实现音乐播放功能，[采用谷歌官方的MediaSession框架](https://developer.android.com/reference/android/support/v4/media/package-summary.html),使用v4包向下兼容,[这里有MediaSession相关中文介绍](http://www.oschina.net/question/2561862_2150611)。

#####1.界面包含：
* 音乐专辑列表
* 音乐专辑内歌曲列表
* 音乐播放界面
* 音乐序列界面
* 音乐歌词界面
* 歌曲评论界面
* 分享界面

#####2.功能涉及：
* 音乐播放、暂停、切换等
* 音乐序列播放模式
* 音乐进度更新

#### 二. 页面关系：
```graphLR
    A(music_album音乐专辑列表) -->B(music_album_detail歌曲列表)
    B --> C(music_paly播放界面)
    C -->|One| D[音乐序列界面]
    C -->|Two| E[歌曲评论界面]
    C -->|Three| F[分享界面]
```

####三.功能实现
待完善。。

更新时间：2017年2月23日18:43:22







