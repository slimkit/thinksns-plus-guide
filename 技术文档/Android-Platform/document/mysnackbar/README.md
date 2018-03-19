# TSSnackBar 说明

此控件仿QQ信息提示从顶部弹出

1. 使用说明

具体请参考 `TSFragment` ，更具不同的 `Prompt` 区分类型

```java
TSnackbar.make(mSnackRootView, message, TSnackbar.LENGTH_SHORT)
                .setPromptThemBackground(prompt)
                .setCallback(new TSnackbar.Callback() {
                    @Override
                    public void onDismissed(TSnackbar TSnackbar, @DismissEvent int event) {
                        super.onDismissed(TSnackbar, event);
                        switch (event) {
                            case DISMISS_EVENT_TIMEOUT:
                                try {
                                    snackViewDismissWhenTimeOut(prompt);
                                } catch (Exception e) {
                                    e.printStackTrace();
                                }
                                break;
                        }
                    }
                })
                .show();

```