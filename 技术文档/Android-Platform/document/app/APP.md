2017年5月11日 10:57:05
# app module

## 项目结构目录：
```shell
--base              // 基类
    ...
--config            // 常量配置信息
--data              // 数据
    --beans              // 数据模型
    --source             // 数据来源
        --local                 // 本地数据
        --remote                // 远程数据
        --repository            // 数据操作
--i                 // 通用接口
--jpush             // 极光推送相关
--modules           // 功能模块
    --channel            // 频道
    --chat               // 聊天
    --collect            // 搜藏
    --crop               // 图片剪切
    --dynamic            // 动态
    --edit_userinfo      // 用户编辑
    --follow_fans        // 关注、粉丝
    --gallery            // 图片查看器
    --guide              // 引导页
    --home               // 主页
    --login              // 登录
    --music_fm           // 音乐FM
    --password           // 忘记密码、找回密码
    --personal_center    // 个人中心
    --photopicker        // 图片选择器
    --rank               // 排行榜
    --register           // 注册
    --settings           // 设置
    --system_conversation           // 系统公告、反馈
--service           // 服务相关
--utils             // 通用工具，更多查看 baseproject 和 common 的 utils
--widget            // 自定义组件
--wxapi             // 微信回调
```
项目主工程，实现ts的所有功能模块
## 文档说明
[app](document/app/APP.md) app 主工程
>- [打包账号说明](document/app/KEYSTORE_EXPLANATION.md)
>- [页面专场动画](document/app/ACTIVITYANIMATION.md)
>- [接口说明](document/app/API.md)
>- [动态功能说明文档](document/app/DYNAMIC.md)
>- [动态列表评论](document/app/DYNAMICLISTCOMMENT.md)
>- [消息对照表](document/app/ERROR_MESSAGE_CODE.md)
>- [图片浏览器](document/app/GALLERY.md)
>- [Lint 检测说明](document/app/LINT.md)
>- [音乐 FM](document/app/MUSIC_FM.md)
>- [文件上传逻辑说明](document/app/UPLOADFILE.md)
>- [RxBinding 的使用](document/app/RXBINDING.md)