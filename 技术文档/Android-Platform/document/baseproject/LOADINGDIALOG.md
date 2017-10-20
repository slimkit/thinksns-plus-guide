2017年5月11日 11:41:20
# 信息弹框
提示信息弹框，包括发送失败，发送成功，正在发送的状态提示。

包括了4个公开的方法：
```
     /**
      * 显示错误的状态
      *
      * @param text 错误或失败状态的提示消息
      */
     public void showStateError(String text) {
         initDialog(R.mipmap.msg_box_remind, text, false);
         sendHideMessage();
     }

     /**
         * 显示成功的状态
         *
         * @param text 正确或成功状态的提示消息
         */
        public void showStateSuccess(String text) {
            initDialog(R.mipmap.msg_box_succeed, text, false);
            sendHideMessage();
        }

     /**
         * 显示进行中的状态
         *
         * @param text 进行中的提示消息
         */
        public void showStateIng(String text) {
            initDialog(R.drawable.frame_loading_grey, text, false);
            handleAnimation(true);
        }

    /**
        * 进行中的状态变为结束
        **/
       public void showStateEnd() {
           handleAnimation(false);
           hideDialog();
       }

```

- showStateError()和showStateSuccess()方法，通过发送一个延迟1s的消息，实现1s的信息弹框显示；
- showStateIng()和showStateEnd()是一组对应的方法，控制信息弹框长时间显示和关闭。

**需要在fragment或者activity的生命周期onDestroy方法中，调用该方法**
```
    public void onDestroy() {
        if (sLoadingDialog != null) {
            sLoadingDialog.dismiss();
        }
    }
```

