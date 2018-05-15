2018-5-2 13:53:18 update by Jliuer@aliyun.com
# 项目中的短视频一条龙文档

##  1.概述
1. 短视频包含 录制、滤镜、压缩、时长剪切等功能
2. 播放视频 用 https://github.com/lipangit/JiaoZiVideoPlayer



## 2.关键包结构说明 （module video）
- filter 滤镜相关
- mediacodec 视频时长剪辑
- recodrender 视频录制预览界面绘制与渲染，
- recordcore 视频录制核心功能，包含编解码与合并

## 3.使用
- 视频录制参考 com.zhiyicx.thinksnsplus.modules.shortvideo.record.RecordFragment
- 视频预览参考 com.zhiyicx.thinksnsplus.modules.shortvideo.preview.PreviewFragment
- 视频时长剪辑参考 com.zhiyicx.thinksnsplus.modules.shortvideo.clipe.TrimmerFragment
- 视频滤镜与滤镜组参考 见视频录制与视频预览

## 4.关键点补充
- com.tym.shortvideo.utils.CameraUtils 这里配置录制分辨率
- com.tym.shortvideo.filter.helper.type.TextureRotationUtils 这里配置录制顶点坐标与纹理坐标
- com.tym.shortvideo.filter.helper.type.TextureRotationUtils 这里配置录制顶点坐标与纹理坐标
- com.tym.shortvideo.filter.base.gpuvideo.GLDefaultFilterGroup 这里配置录制滤镜组

ps:视频录制所参考的开源项目 https://github.com/CainKernel/CainCamera
由于触犯某些隐私条款，被原作者删除了