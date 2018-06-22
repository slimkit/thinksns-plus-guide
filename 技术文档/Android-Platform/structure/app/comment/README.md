### comment
> 主要是和评论相关的一些类

- CommentTypeConfig
> 评论配置类，提供评论类型，专辑评论和歌曲评论

- CommonCommentClient
> 接口类，提供评论相关的接口

- CommonMetadata
> 评论操作的公共类，主要存放评论相关的信息内容

- CommonMetadataBean
> 评论的实体类，包括评论的id、回复的id、用户的id、评论内容等

- CommonMetadataProvider
> 评论内容处理基类

- DeleteComment
> 删除评论处理类

- ICommentBean
> 一个接口主要提供一个获取评论的方法

- ICommentEvent
> 评论处理的基类,提供三个处理评论的方法

- ICommentState
> 评论处理类别接口

- ICommonMetadataProvider
> 评论内容转换

- SendComment
> 发送评论处理类，实现评论处理的基类ICommentEvent

- TCommonMetadataProvider
> 音乐评论处理，实现基类的方法，包装评论内容
