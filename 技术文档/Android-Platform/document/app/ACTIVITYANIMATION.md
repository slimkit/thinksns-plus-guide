2017年5月11日 10:32:55
# Activity的缩放转场动画实现
实现图片放大，进入图片预览界面的效果

## 实现方式：
以图片浏览器为例：（页面A表示进入初始页面 页面B表示进入的目标页面 由A - >B）

- 修改GalleryActivity的主题
```
<style name="TransParentTheme" parent="TSTheme">
        <item name="android:windowBackground">@android:color/transparent</item>
        <item name="android:windowFullscreen">false</item><!---->
        <item name="android:windowFrame">@null</item>
        <item name="android:windowContentOverlay">@null</item>

        <item name="android:backgroundDimEnabled">false</item>
        <item name="android:windowIsTranslucent">true</item>

        <item name="android:windowAnimationStyle">@null</item>
        <item name="windowActionBar">false</item>
        <item name="windowNoTitle">true</item>
    </style>
```
取消页面B进入和退出的转场动画，设置为空；设置页面B的背景透明

- 获取页面A需要放缩的图片控件宽高等属性
```
 ArrayList<AnimationRectBean> animationRectBeanArrayList
                = new ArrayList<AnimationRectBean>();
        for (int i = 0; i < imageBeanList.size(); i++) {
            int id = UIUtils.getResourceByName("siv_" + i, "id", getContext());
            ImageView imageView = holder.getView(id);

            AnimationRectBean rect = AnimationRectBean.buildFromImageView(imageView);
            animationRectBeanArrayList.add(rect);
        }

        GalleryActivity.startToGallery(getContext(), position, imageBeanList, animationRectBeanArrayList);
```
animationRectBeanArrayList表示需要传入的图片属性，需要注意以下几点：<br>
1.如果ImageView不可见或者AnimationRectBean为空，但传入了图片路径，相应的也要往animationRectBeanArrayList
中传入null与之对应，就是说要保证imageBeanList与animationRectBeanArrayList长度相同；<br>
2.对于不同的页面，获取animationRectBeanArrayList的方式可能不一样，比如动态列表9张图片作为一个item的内容，
但是在图片选择其中，每张图片都作为一个item，这样就有不同的方式，获取到这些AnimationRectBean属性；<br>

- 跳转页面进入页面B<br>

  需要做两件事：处理页面B的背景颜色变化；处理页面B中图片的放大动画；<br>

  对背景变化处理,主要调用下面的两个方法：
  ```
      // 立刻让背景变成纯黑色，没有放缩动画时调用
      public void showBackgroundImmediately() {
          if (mRootView.getBackground() == null) {
              backgroundColor = new ColorDrawable(Color.BLACK);
              mVpPhotos.setBackground(backgroundColor);
          }
      }

      // 通过渐变动画，让背景逐渐变成纯黑色，和防缩动画同时调用
      public ObjectAnimator showBackgroundAnimate() {
          backgroundColor = new ColorDrawable(Color.BLACK);
          ObjectAnimator bgAnim = ObjectAnimator
                  .ofInt(backgroundColor, "alpha", 0, 255);
          bgAnim.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
              @Override
              public void onAnimationUpdate(ValueAnimator animation) {
                  mVpPhotos.setBackground(backgroundColor);
              }
          });
          return bgAnim;
      }
  ```

  处理页面B中图片的放大动画：<br>
   在[TransferImageAnimationUtil](https://github.com/zhiyicx/thinksns-plus-android/blob/5cbd0090a88a300f9429e68d411b90aedfad340f/app/src/main/java/com/zhiyicx/thinksnsplus/utils/TransferImageAnimationUtil.java)
   类中，封装了放大和缩小的两个动画；以进入动画为例：
   ```
       imageView.getViewTreeObserver()
                   .addOnPreDrawListener(new ViewTreeObserver.OnPreDrawListener() {
                       @Override
                       public boolean onPreDraw() {

                           if (rect == null) {
                               imageView.getViewTreeObserver().removeOnPreDrawListener(this);
                               endAction.run();
                               return true;
                           }

                           final Rect startBounds = new Rect(rect.scaledBitmapRect);
                           final Rect finalBounds =
                                   DrawableProvider.getBitmapRectFromImageView(imageView);

                           if (finalBounds == null) {
                               imageView.getViewTreeObserver().removeOnPreDrawListener(this);
                               endAction.run();
                               return true;
                           }

                           float startScale = (float) finalBounds.width() / startBounds.width();

                           if (startScale * startBounds.height() > finalBounds.height()) {
                               startScale = (float) finalBounds.height() / startBounds.height();
                           }

                           int deltaTop = startBounds.top - finalBounds.top;
                           int deltaLeft = startBounds.left - finalBounds.left;
                           // 位移+缩小
                           imageView.setPivotY(
                                   (imageView.getHeight() - finalBounds.height()) / 2);
                           imageView.setPivotX((imageView.getWidth() - finalBounds.width()) / 2);

                           imageView.setScaleX(1 / startScale);
                           imageView.setScaleY(1 / startScale);

                           imageView.setTranslationX(deltaLeft);
                           imageView.setTranslationY(deltaTop);

                           if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.JELLY_BEAN) {
                               imageView.animate().translationY(0).translationX(0)
                                       .scaleY(1)
                                       .scaleX(1).setDuration(ANIMATION_DURATION)
                                       .setInterpolator(
                                               new AccelerateDecelerateInterpolator())
                                       .withEndAction(endAction);
                           }

                           AnimatorSet animationSet = new AnimatorSet();
                           animationSet.setDuration(ANIMATION_DURATION);
                           animationSet
                                   .setInterpolator(new AccelerateDecelerateInterpolator());

                           animationSet.playTogether(ObjectAnimator.ofFloat(imageView,
                                   "clipBottom",
                                   AnimationRectBean.getClipBottom(rect, finalBounds), 0));
                           animationSet.playTogether(ObjectAnimator.ofFloat(imageView,
                                   "clipRight",
                                   AnimationRectBean.getClipRight(rect, finalBounds), 0));
                           animationSet.playTogether(ObjectAnimator.ofFloat(imageView,
                                   "clipTop", AnimationRectBean.getClipTop(rect, finalBounds), 0));
                           animationSet.playTogether(ObjectAnimator.ofFloat(imageView,
                                   "clipLeft", AnimationRectBean.getClipLeft(rect, finalBounds), 0));

                           animationSet.start();

                           imageView.getViewTreeObserver().removeOnPreDrawListener(this);
                           return true;
                       }
                   });
       }
   ```

给需要放缩的ImageView添加addOnPreDrawListener监听，在绘制时进行放大动画的处理；
其中startBounds和finalBounds两个对象，startBounds保存了图片在页面A时的宽高位置属性，
finalBounds保存了图片在页面B时的最终宽高位置属性；在页面B中绘制该ImageView时，设置
显示的初始位置startBounds，设置最终位置finalBounds，在通过位移，以及缩放动画，实现放大的效果；<br>
退出动画类似的逻辑，但只需要处理缩小动画效果就行了；

- 退出页面B，回到页面A：<br>
实现相反的效果即可：页面B背景逐渐透明，图片放缩到进入的初始位置，然后finish页面B；

**在实现缩放转场动画时，要注意的是保证页面B图片加载速度快，最好是马上显示，这样才不会有卡顿效果；**

## 可扩展的逻辑：
魅族手机自带相册，预览图片的效果：图片在界面上不完全可见，有一部分或者全部在屏幕外，通过滑动列表，将其移动到
屏幕的顶部或者底部，这样在获取finalBounds时，则不会出现AnimationRectBean为空的情况，保证任何情况下实现退出页面B的缩放效果