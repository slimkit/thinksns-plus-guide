# call
> 该包下是音视频通话相关的类

- [receiver 包](./receiver)

- [video 包](./video)

- [voice 包](./voice)

- BaseCallActivity
> 视频语音通话的activity基类，主要提供跳转到视频通话或者跳转到音频通话的界面

- BaseCallFragment
> 视频语音通话的fragment基类，主要实现音视频通话共有的方法，以及提供音视频通话有差异的方法，让子类去继承重写

- TSEMHyphenate
> 自定义初始化类做一些环信sdk的初始化操作