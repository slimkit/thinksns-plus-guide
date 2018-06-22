# Project

```
├──app                                                  :程序的主module
│  ├──libs                                              :存放aar库文件
│  ├──src
│  │  └──main
│  │     ├──assets                                      :资源文件
│  │     ├──java
│  │     │  └──com.zhiyicx.thinksnsplus
│  │     │     ├──base                                  :部分基类
│  │     │     ├──comment                               :评论相关的操作和配置
│  │     │     ├──config                                :配置相关的文件（后台任务，错误码，EventBus事件，极光消息类型等）
│  │     │     ├──data
│  │     │     │  ├──beans                              :各种数据的实体类
│  │     │     │  └──source                             :提供数据获取的方式（数据库db，网络client）
│  │     │     │
│  │     │     ├──i                                     :用户事件监听（点击评论文字，点击（长按）用户头像或名字）
│  │     │     ├──jpush                                 :极光消息接收
│  │     │     ├──modules						        :程序主要页面（像登录页，主页等）
│  │     │     │  ├──certification
│  │     │     │  │  ├──detail                          :用户认证详请页面
│  │     │     │  │  ├──input                           :用户申请认证第一步
│  │     │     │  │  └──send                            :用户申请认证第二步
│  │     │     │  │
│  │     │     │  ├──chat                               :聊天相关的页面
│  │     │     │  │  ├──adapter                         :聊天相关用户的列表
│  │     │     │  │  ├──call
│  │     │     │  │  │  ├──receiver                     :视频语音来电监听
│  │     │     │  │  │  ├──video                        :视频电话
│  │     │     │  │  │  └──voice                        :语音电话
│  │     │     │  │  │
│  │     │     │  │  ├──edit
│  │     │     │  │  │  ├──manager                      :群组管理
│  │     │     │  │  │  ├──name                         :编辑群名字
│  │     │     │  │  │  └──owner                        :转让群主
│  │     │     │  │  │
│  │     │     │  │  ├──info                            :聊天室信息
│  │     │     │  │  ├──item                            :聊天消息的类型
│  │     │     │  │  ├──location                        :发送位置信息页面
│  │     │     │  │  ├──member                          :群成员信息
│  │     │     │  │  ├──select                          :选择好友
│  │     │     │  │  └──video                           :视频消息
│  │     │     │  │
│  │     │     │  ├──circle
│  │     │     │  │  ├──all_circle                      :圈子列表
│  │     │     │  │  │  └──container                    :所有圈子（ViewPager）
│  │     │     │  │  │
│  │     │     │  │  ├──create                          :创建圈子
│  │     │     │  │  │  ├──location                     :创建圈子时选择的位置
│  │     │     │  │  │  ├──rule                         :创建圈子的协议
│  │     │     │  │  │  └──types                        :选择圈子的类型
│  │     │     │  │  │
│  │     │     │  │  ├──detailv2                        :圈子详情
│  │     │     │  │  │  ├──adapter                      :圈子动态0-9张图，不同的布局样式
│  │     │     │  │  │  ├──dig                          :点赞列表
│  │     │     │  │  │  └──post                         :圈子动态详情
│  │     │     │  │  │
│  │     │     │  │  ├──main                            :圈子主页（从发现页点击进去的页面）
│  │     │     │  │  ├──manager                         :圈子管理页面
│  │     │     │  │  │  ├──earning                      :圈子收入
│  │     │     │  │  │  │  └──record                    :圈子收入记录
│  │     │     │  │  │  │
│  │     │     │  │  │  ├──members                      :圈子成员
│  │     │     │  │  │  │  ├──attorn                    :圈主转让
│  │     │     │  │  │  │  └──blacklist                 :圈子黑名单
│  │     │     │  │  │  │
│  │     │     │  │  │  ├──permission                   :圈子发帖权限
│  │     │     │  │  │  └──report                       :处理圈子举报
│  │     │     │  │  │
│  │     │     │  │  ├──mine
│  │     │     │  │  │  ├──container                    :我的圈子页面（ViewPager）
│  │     │     │  │  │  └──joined                       :我加入的圈子（加入的、待审核的）
│  │     │     │  │  │
│  │     │     │  │  ├──publish                         :发布帖子页面
│  │     │     │  │  │  └──choose_circle                :发布帖子选择圈子（入口暂时隐藏）
│  │     │     │  │  │
│  │     │     │  │  └──search                          :圈子、帖子搜索页面
│  │     │     │  │     ├──container                    :圈子首页进入搜索页的ViewPager
│  │     │     │  │     └──onlypost                     :圈子详情进入的只能搜索帖子
│  │     │     │  │
│  │     │     │  ├──collect                            :我的收藏页面
│  │     │     │  │  ├──album                           :相册
│  │     │     │  │  ├──dynamic                         :动态
│  │     │     │  │  ├──group_posts                     :帖子
│  │     │     │  │  ├──info                            :资讯
│  │     │     │  │  └──qa                              :问答
│  │     │     │  │
│  │     │     │  ├──crop                               :图片裁剪相关
│  │     │     │  ├──develop                            :开发提示页面
│  │     │     │  ├──draftbox                           :草稿箱
│  │     │     │  │  └──adapter
│  │     │     │  │
│  │     │     │  ├──dynamic                            :动态相关
│  │     │     │  │  ├──detail                          :动态详情
│  │     │     │  │  ├──list                            :动态列表
│  │     │     │  │  ├──send                            :发布动态
│  │     │     │  │  ├──tollcomment                     :动态评论置顶
│  │     │     │  │  └──topdynamic_comment              :动态评论置顶（未使用）
│  │     │     │  │
│  │     │     │  ├──edit_userinfo                      :用户资料页面
│  │     │     │  │  └──location                        :选择位置
│  │     │     │  │     └──search                       :搜索位置
│  │     │     │  │
│  │     │     │  ├──feedback                           :用户反馈
│  │     │     │  ├──findsomeone                        :找人页面
│  │     │     │  │  ├──contacts                        :通讯录界面
│  │     │     │  │  ├──contianer                       :找人界面ViewPager容器
│  │     │     │  │  ├──list                            :找人界面结果页面
│  │     │     │  │  │   └──nearby                      :找人界面附近的人
│  │     │     │  │  │
│  │     │     │  │  └──search
│  │     │     │  │     └──name                         :根据用户名搜索用户
│  │     │     │  │
│  │     │     │  ├──follow_fans                        :粉丝和关注
│  │     │     │  ├──gallery                            :画廊
│  │     │     │  ├──guide                              :程序引导页（启动的第一个界面）
│  │     │     │  ├──home                               :home页面
│  │     │     │  │  ├──find                            :发现页面
│  │     │     │  │  ├──main                            :首页
│  │     │     │  │  ├──message                         :消息页面
│  │     │     │  │  │  ├──container                    :消息页面的ViewPager
│  │     │     │  │  │  ├──messagecomment               :收到的评论
│  │     │     │  │  │  ├──messagegroup                 :消息左上角进入的群组列表
│  │     │     │  │  │  ├──messagelike                  :收到的赞
│  │     │     │  │  │  ├──messagelist                  :聊天列表
│  │     │     │  │  │  ├──messagereview                :审核通知
│  │     │     │  │  │  └──notifacationlist             :系统消息
│  │     │     │  │  │
│  │     │     │  │  └──mine                            :我
│  │     │     │  │     ├──friends                      :好友界面
│  │     │     │  │     │  └──search                    :搜索好友
│  │     │     │  │     │
│  │     │     │  │     ├──mycode                       :我的二维码
│  │     │     │  │     └──scan                         :扫描二维码
│  │     │     │  │
│  │     │     │  ├──information                        :资讯相关
│  │     │     │  │  ├──adapter
│  │     │     │  │  ├──dig                             :点赞列表
│  │     │     │  │  ├──infochannel                     :选择显示的资讯类型
│  │     │     │  │  ├──infodetails                     :资讯详情
│  │     │     │  │  ├──infomain                        :资讯首页
│  │     │     │  │  ├──infosearch                      :搜索资讯
│  │     │     │  │  ├──my_info                         :我的投稿
│  │     │     │  │  └──publish                         :发布资讯
│  │     │     │  │
│  │     │     │  ├──login                              :登录
│  │     │     │  ├──markdown_editor                    :MarkDown编辑
│  │     │     │  ├──music_fm
│  │     │     │  │  ├──bak_paly                        :音乐播放器功能相关
│  │     │     │  │  ├──media_data                      :音乐数据相关
│  │     │     │  │  ├──music_album_detail              :专辑详情
│  │     │     │  │  ├──music_album_list                :专辑列表
│  │     │     │  │  ├──music_comment                   :音乐评论
│  │     │     │  │  ├──music_helper                    :音乐相关帮助类
│  │     │     │  │  ├──music_play                      :音乐播放页
│  │     │     │  │  └──paided_music                    :我购买的音乐或者专辑（未启用）
│  │     │     │  │
│  │     │     │  ├──password
│  │     │     │  │  ├──changepassword                  :修改密码
│  │     │     │  │  └──findpassword                    :找回密码
│  │     │     │  │
│  │     │     │  ├──personal_center                    :个人中心
│  │     │     │  │  └──adapter                         :个人中心动态的adapter
│  │     │     │  │
│  │     │     │  ├──photopicker                        :图片选择
│  │     │     │  ├──q_a                                :问答模块
│  │     │     │  │  ├──answer                          :编辑回答
│  │     │     │  │  ├──detail
│  │     │     │  │  │  ├──adapter                      :答案问题详情页面adapter
│  │     │     │  │  │  ├──answer                       :答案详情页面
│  │     │     │  │  │  │  └──dig_list                  :答案详情点赞
│  │     │     │  │  │  │
│  │     │     │  │  │  ├──question                     :问题详情页面
│  │     │     │  │  │  │  └──comment                   :问题详情评论页面
│  │     │     │  │  │  │
│  │     │     │  │  │  └──topic                        :话题
│  │     │     │  │  │     └──list                      :话题列表页面
│  │     │     │  │  │
│  │     │     │  │  ├──mine                            :我的问答
│  │     │     │  │  │  ├──adapter                      :我的问答相关的adapter
│  │     │     │  │  │  ├──answer                       :我的回答页面
│  │     │     │  │  │  ├──container                    :我的问答页面ViewPager
│  │     │     │  │  │  ├──follow                       :我的问答关注页面
│  │     │     │  │  │  └──question                     :我的问答问题页面
│  │     │     │  │  │
│  │     │     │  │  ├──publish
│  │     │     │  │  │  ├──add_topic                    :发布问题添加话题
│  │     │     │  │  │  ├──create_topic                 :创建话题
│  │     │     │  │  │  ├──detail                       :发布问题第二步
│  │     │     │  │  │  └──question                     :发布问题第一步
│  │     │     │  │  │
│  │     │     │  │  ├──qa_main                         :问答主页
│  │     │     │  │  │  ├──qa_container                 :外层的ViewPager
│  │     │     │  │  │  ├──qa_listinfo                  :问答列表
│  │     │     │  │  │  └──qa_topiclist                 :话题列表
│  │     │     │  │  │
│  │     │     │  │  ├──reward                          :设置问题的报酬
│  │     │     │  │  │  └──expert_search                :专家搜索页面
│  │     │     │  │  │
│  │     │     │  │  └──search                          :搜索页面
│  │     │     │  │     ├──container                    :外层的ViewPager
│  │     │     │  │     └──list
│  │     │     │  │        ├──qa                        :问题搜索
│  │     │     │  │        └──topic                     :话题搜索
│  │     │     │  │
│  │     │     │  ├──rank                               :排行榜
│  │     │     │  │  ├──adapter                         :排行榜item的adapter
│  │     │     │  │  ├──main                            :排行榜主页
│  │     │     │  │  │  ├──container                    :外层的ViewPager
│  │     │     │  │  │  └──list                         :排行榜列表
│  │     │     │  │  └──type_list                       :排行榜分类（用户，问答，动态，资讯）
│  │     │     │  │
│  │     │     │  ├──register                           :注册
│  │     │     │  │  └──rule                            :注册用户协议
│  │     │     │  │
│  │     │     │  ├──report                             :举报
│  │     │     │  ├──settings                           :设置页面
│  │     │     │  │  ├──aboutus                         :关于我们
│  │     │     │  │  ├──account                         :账号管理
│  │     │     │  │  ├──bind                            :账号绑定
│  │     │     │  │  ├──blacklist                       :黑名单
│  │     │     │  │  └──init_password                   :初始化密码
│  │     │     │  │
│  │     │     │  ├──shortvideo                         :短视频
│  │     │     │  │  ├──adapter                         :adapter
│  │     │     │  │  ├──clipe                           :裁剪视频
│  │     │     │  │  ├──cover                           :选择视频封面
│  │     │     │  │  ├──helper                          :帮助类
│  │     │     │  │  ├──preview                         :视频预览界面
│  │     │     │  │  ├──record                          :视频录制
│  │     │     │  │  └──videostore                      :视频选择
│  │     │     │  │
│  │     │     │  ├──third_platform                     :三方平台
│  │     │     │  │  ├──bind                            :绑定已有账号
│  │     │     │  │  ├──choose_bind                     :选择绑定方式
│  │     │     │  │  └──complete                        :绑定成功完善资料
│  │     │     │  │
│  │     │     │  ├──usertag                            :用户标签
│  │     │     │  └──wallet                             :钱包
│  │     │     │     ├──bill                            :钱包明细
│  │     │     │     ├──bill_detail                     :明细详情
│  │     │     │     ├──integration                     :积分
│  │     │     │     │  ├──detail                       :积分明细
│  │     │     │     │  │  └──recharge_withdrawal       :积分提取明细
│  │     │     │     │  │
│  │     │     │     │  ├──mine                         :我的积分
│  │     │     │     │  ├──recharge                     :积分充值
│  │     │     │     │  └──withdrawal                   :积分提取
│  │     │     │     │
│  │     │     │     ├──recharge                        :钱包充值
│  │     │     │     ├──reward                          :打赏（咨询，动态，用户，帖子，问答回答）
│  │     │     │     │  └──list                         :打赏列表
│  │     │     │     │
│  │     │     │     ├──rule                            :钱包充值提现规则
│  │     │     │     ├──sticktop                        :置顶费用
│  │     │     │     └──withdrawals                     :钱包提现
│  │     │     │        └──list_detail                  :提现详情
│  │     │     │
│  │     │     ├──service.backgroundtask    	        :后台任务
│  │     │     ├──strategy                     	        :登录策略（未使用）
│  │     │     ├──utils                     	        :工具类
│  │     │     │  └──recyclerview_diff                  :判断列表不同数据，局部刷新（未使用）
│  │     │     │
│  │     │     ├──widget                     	        :控件相关
│  │     │     │  ├──chat                               :聊天相关控件
│  │     │     │  ├──chooseview                         :选择控件
│  │     │     │  ├──comment                            :评论控件
│  │     │     │  ├──coordinatorlayout                  :coordinatorlayout的behavior
│  │     │     │  ├──flowtag                            :流式布局控件
│  │     │     │  ├──imageview                          :图形控件
│  │     │     │  ├──listener                           :触摸事件接口
│  │     │     │  ├──pager_recyclerview                 :pageriew样式的recyclerview
│  │     │     │  └──popwindow                          :弹出框
│  │     │     │
│  │     │     └──wxapi                     	        :微信相关回调
│  │     │
│  │     ├──res                                 :资源文件夹（图标，布局，尺寸等）
│  │     └──AndroidManifest.xml                 :android注册文件
│  │
│  ├──build.gradle                              :app module的gradle文件，可以配置依赖，打包相关的配置，极光推送的配置也在其中
│  └──proguard-rules.pro                        :打包混淆的配置文件
│
├──appupdate                                    :程序升级的模块
│  ├──src
│  │  └──main
│  │     ├──java
│  │     │  └──com.zhiyicx.appupdate            :应用升级相关的类
│  │     ├──res                                 :资源文件夹（图标，布局，尺寸等）
│  │     └──AndroidManifest.xml                 :android注册文件
│  │
│  └──build.gradle                              :appupdate module的gradle文件，可以配置依赖
│
├──banner                                       :轮播图模块
│  ├──src
│  │  └──main
│  │     ├──java
│  │     │  └──com.youth.banner
│  │     │     ├──listener                      :轮播图事件监听接口
│  │     │     ├──loader                        :轮播图图片加载
│  │     │     ├──transformer                   :ViewPager的transformer，可以打造各种样式的ViewPager
│  │     │     └──view                          :轮播图的ViewPager
│  │     │
│  │     └──res                                 :资源文件夹（图标，布局，尺寸等）
│  │
│  └──build.gradle                              :banner module的gradle文件，可以配置依赖
│
├──baseadapter-recyclerview                     :baseadapter通用recyclerview的adapter
│  ├──src
│  │  └──main
│  │     ├──java
│  │     │  └──com.zhy.adapter.recyclerview
│  │     │     ├──base                          :item相关
│  │     │     ├──utils                         :wrapper工具类
│  │     │     └──wrapper                       :wrapper
│  │     │
│  │     └──res                                 :资源文件夹（图标，布局，尺寸等）
│  │
│  └──build.gradle                              :baseadapter-recyclerview module的gradle文件，可以配置依赖
│
├──baseproject                                  :提供基础的类
│  ├──libs                                      :存放各种.jar库文件
│  ├──src
│  │  └──main
│  │     ├──java
│  │     │  └──com.zhiyicx.baseproject
│  │     │     ├──base                          :提供一些基类
│  │     │     ├──cache                         :缓存相关
│  │     │     ├──config                        :配置相关（广告，接口，友盟等）
│  │     │     ├──crashhandler                  :异常捕获（未使用）
│  │     │     ├──em.manager                    :聊天相关
│  │     │     │  ├──eventbus                   :聊天Eventbus事件
│  │     │     │  └──util                       :聊天工具
│  │     │     │
│  │     │     ├──impl
│  │     │     │  ├──imageloader.glide          :图片加载相关
│  │     │     │  │  ├──progress                :图片加载进度
│  │     │     │  │  └──transformation          :图片变换
│  │     │     │  │
│  │     │     │  ├──photoselector              :图片选择
│  │     │     │  └──share                      :分享策略
│  │     │     │
│  │     │     ├──utils                         :工具类
│  │     │     └──widget                        :控件
│  │     │        ├──button                     :按钮
│  │     │        ├──dialog                     :对话框
│  │     │        ├──dragview                   :可以拖拽的view
│  │     │        ├──edittext                   :文本编辑
│  │     │        ├──imageview                  :图片
│  │     │        ├──indicator_expand           :指示条
│  │     │        ├──photoview                  :图片view
│  │     │        │  ├──gestures                :手势
│  │     │        │  ├──scrollerproxy           :滚动代理
│  │     │        │  └──log                     :log日志
│  │     │        │
│  │     │        ├──popwindow                  :弹窗框
│  │     │        ├──recycleview
│  │     │        │  └──stickygridheaders       :粘性头部
│  │     │        │
│  │     │        ├──refresh                    :刷新相关
│  │     │        └──textview                   :文本
│  │     │
│  │     ├──res                                 :资源文件夹（图标，布局，尺寸等）
│  │     └──AndroidManifest.xml                 :android注册文件
│  │
│  └──build.gradle                              :baseproject module的gradle文件，可以配置依赖,配置友盟，环信，高德，腾讯qq
│
├──BigImageViewer                               :显示图片的view继承自FrameLayout
│  ├──src
│  │  └──main
│  │     ├──java
│  │     │  └──com.github.piasy.biv
│  │     │     ├──indicator                     :指示器
│  │     │     ├──loader                        :图片加载接口类，提供需要实现的方法
│  │     │     ├──utils                         :工具类
│  │     │     └──view                          :BigImageView
│  │     │
│  │     ├──res                                 :资源文件夹（图标，布局，尺寸等）
│  │     └──AndroidManifest.xml                 :android注册文件
│  │
│  └──build.gradle                              :BigImageViewer module的gradle文件，可以配置依赖
│
├──common
│  ├──src
│  │  └──main
│  │     ├──java
│  │     │  └──com.zhiyicx.common
│  │     │     ├──base                          :一些基类（BaseActivity，BaseApplication等）
│  │     │     │  └──i                          :接口
│  │     │     │
│  │     │     ├──config                        :全局通用常量相关工具类
│  │     │     ├──dagger
│  │     │     │  ├──module                     :定义基础的module
│  │     │     │  └──scope                      :定义dagger使用的scope域
│  │     │     ├──mvp                           :定义mvp模式的presenter基类
│  │     │     │  └──i                          :Base接口
│  │     │     │
│  │     │     ├──net                           :网络相关的类
│  │     │     │  ├──intercept                  :OkHttp拦截器
│  │     │     │  └──listener                   :网络回调监听接口
│  │     │     │
│  │     │     ├──thirdmanager.share            :三方分享的类
│  │     │     ├──utils                         :工具类
│  │     │     │  ├──appprocess                 :进程相关工具
│  │     │     │  ├──gson                       :gson
│  │     │     │  ├──imageloader
│  │     │     │  │  ├──config                  :图片加载的配置信息
│  │     │     │  │  ├──core                    :图片加载器
│  │     │     │  │  └──loadstrategy            :图片加载策略接口
│  │     │     │  │
│  │     │     │  ├──log                        :日志打印工具
│  │     │     │  └──recycleviewdecoration      :recyclerview的装饰（decoration）
│  │     │     │
│  │     │     └──widget                        :控件
│  │     │        ├──badgeview                  :小红点提示
│  │     │        └──popwindow                  :弹出框
│  │     │
│  │     ├──res                                 :资源文件夹（图标，布局，尺寸等）
│  │     └──AndroidManifest.xml                 :android注册文件
│  │
│  └──build.gradle                              :common module的gradle文件，可以配置依赖
│
├──contacts                                     :联系人
│  ├──src
│  │  └──main
│  │     ├──java
│  │     │  └──com.github.tamir7.contacts       :联系人相关
│  │     │
│  │     └──res                                 :资源文件夹（图标，布局，尺寸等）
│  │
│  └──build.gradle                              :contacts module的gradle文件，可以配置依赖
│
├──easeui
│  ├──libs                                      :和聊天相关的.jar包
│  ├──src
│  │  └──main
│  │     ├──java
│  │     │  └──com.hyphenate.easeui
│  │     │     ├──adapter                       :聊天相关的adapter
│  │     │     ├──bean                          :聊天专用的实体类
│  │     │     ├──domain                        :用户和表情
│  │     │     ├──model                         :
│  │     │     │  └──styles                     :
│  │     │     │
│  │     │     ├──ui                            :UI界面相关
│  │     │     ├──utils                         :工具类
│  │     │     └──widget                        :控件
│  │     │        ├──chatrow                    :聊天消息类别
│  │     │        ├──emojicon                   :Emoji表情
│  │     │        ├──photoview                  :图片
│  │     │        └──presenter                  :Presenter
│  │     │
│  │     ├──jniLibs                             :和聊天相关的.so库
│  │     └──res                                 :资源文件夹（图标，布局，尺寸等）
│  │
│  └──build.gradle                              :easeui module的gradle文件，可以配置依赖
│
├──GlideImageLoader                             :Glide图片加载
│  ├──src
│  │  └──main
│  │     ├──java
│  │     │  └──com.github.piasy.biv.loader.glide:图片加载（未使用）
│  │     │
│  │     └──res                                 :资源文件夹（图标，布局，尺寸等）
│  │
│  └──build.gradle                              :GlideImageLoader module的gradle文件，可以配置依赖
│
├──imsdk 										:自己实现的聊天（未使用）
│
├──link-library 								:文字样式模块（比如链接，@等）
│
├──media-cache									:缓存
│
├──mysnackbar									:提示信息控件模块
│
├──overscrolllib								:overscroll模块
│
├──PhotoPicker									:图片选择模块
│
├──pickerview									:滚动选择器（比如时间）
│
├──richtexteditorlib							:富文本编辑模块
│
├──rxerrorhandler								:Rxjava异常处理模块
│
├──tspay										:支付模块（只用到了常量）
│
├──ucrop										:图片裁剪功能模块
│
└─video											:短视频功能模块
```


## 1.模块

[app](./app)

[appupdate](./appupdate)

[banner](./banner)

[baseadapter-recyclerview](./baseadapter-recyclerview)

[baseproject](./baseproject)

[BigImageViewer](./BigImageViewer)

[common](./common)

[contacts](./contacts)

[easeui](./easeui)

[GlideImageLoader](./GlideImageLoader)

imsdk——自己实现的聊天（未使用）

link-library——文字样式模块（比如链接，@等）

media-cache——缓存

mysnackbar——提示信息控件模块

overscrolllib——overscroll模块

PhotoPicker——图片选择模块

pickerview——滚动选择器（比如时间）

richtexteditorlib——富文本编辑模块

rxerrorhandler——Rxjava异常处理模块

tspay——支付模块（只用到了常量）

ucrop——图片裁剪功能模块

video——短视频功能模块

## 2. .gitignore文件
> 可以配置Git的忽略内容

## 3. build.gradle文件
> 工程的gradle文件

## 4.config.gradle文件
> 配置文件，其中可以配置依赖的版本

## 5.gradle文件夹
> 其中的gradle-wrapper.properties可以配置gradle的版本

