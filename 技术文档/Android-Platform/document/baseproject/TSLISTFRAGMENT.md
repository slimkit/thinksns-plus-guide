2017年5月11日 11:44:47
#  基础列表类说明

  ## 1. 概述

  基于 `TSFragment`  封装的基础列表类。支持
  - 支持上拉刷新，下拉加载
  - 支持缓存存储与读取
  - 支持缺省页提示，单击重新加载
  - 数据自动处理
  - 支持多`Item`类型
  - 支持自定义列表样式

  ## 2. 使用说明

  1. 使用条件

      - 继承`TSListFragment<P extends ITSListPresenter,T>` 并提供相应的泛型支持；
      - 实现相应的抽象方法和接口方法；

  2. 示例，请参考[MessageLikeFragment](../../app/src/main/java/com/zhiyicx/thinksnsplus/modules/home/message/messagelike/MessageLikeFragment.java)

  3. 移动数据源到基类中
