## H5目录结构

```
.
├── dist 已经编译好的静态资源
│   ├── css
│   ├── img
│   ├── js
│   └── libs
├── public 公共内容
│   └── libs
├── scripts vue编译时需要的命令
├── src 主目录，操作都在这里
│   ├── api api目录
│   ├── components 组件目录
│   │   ├── FeedCard 动态相关组件
│   │   ├── common 页面中使用的公共组件
│   │   ├── form 提交表单的公共组件
│   │   ├── reference
│   │   ├── style
│   │   │   └── pswp
│   │   ├── tabs tab相关的组件
│   │   └── vendor 三方登录组件
│   ├── console 控制台样式
│   ├── constants 常量
│   ├── directives 指令目录
│   ├── easemob 环信相关
│   ├── icons icon相关
│   ├── images 图片相关
│   ├── locales 多语言相关
│   ├── page 页面目录
│   │   ├── article
│   │   │   └── components
│   │   ├── checkin 签到
│   │   ├── common 公共页面
│   │   ├── feed 动态
│   │   ├── find 发现
│   │   ├── group 圈子
│   │   │   └── components
│   │   ├── message 消息
│   │   │   ├── children
│   │   │   │   ├── audits
│   │   │   │   ├── comments
│   │   │   │   └── likes
│   │   │   ├── components
│   │   │   └── list
│   │   ├── news 资讯
│   │   │   └── components
│   │   ├── post 发布页面
│   │   │   └── components
│   │   ├── profile 个人主页
│   │   │   ├── children
│   │   │   ├── collection
│   │   │   └── components
│   │   ├── question 问答
│   │   │   └── components
│   │   ├── rank 排行榜
│   │   │   ├── children
│   │   │   ├── components
│   │   │   └── lists
│   │   ├── sign 登录
│   │   ├── topic 话题
│   │   │   └── components
│   │   └── wechat
│   ├── plugins 插件
│   │   ├── imgCropper
│   │   ├── lstore
│   │   ├── message
│   │   │   └── style
│   │   └── message-box
│   ├── routers 路由目录
│   ├── stores vuex目录
│   │   ├── easemob
│   │   └── module
│   │       ├── easemob
│   │       ├── post
│   │       └── rank
│   ├── style 样式
│   ├── util 工具库
│   ├── utils 工具库
│   └── vendor 三方组件
│       └── easemob
└── tests 测试
    └── unit
        └── components
            └── common

```