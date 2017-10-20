2017年5月11日 11:24:56
# 图片浏览器
用于网络图片的查看，主要功能包括：

- 圆点指示器
- 查看原图
- 图片保存
- 缩放动画
- 缩略图加载

## 圆点指示器
 通过三方库[MagicIndicator指示器](https://github.com/hackware1993/MagicIndicator)实现；
 添加[ScaleCircleNavigator](https://github.com/zhiyicx/thinksns-plus-android/blob/fed8e7360cfe9b267ff9e67f3d9f8666214af2cf/baseproject/src/main/java/com/zhiyicx/baseproject/widget/indicator_expand/ScaleCircleNavigator.java)可放缩的指示器；
 添加“大于一张图片才会显示小圆点，否则隐藏”逻辑；

## 查看原图
通过重写Glide的StreamModelLoader接口，添加OkHttp获取网络图片的逻辑，同时添加
OkHttp拦截器，监听从网络获取的图片进度；<br>

重写Glide获取网络图片的逻辑
```
@Override
    public InputStream loadData(Priority priority) {
        // 重写Glide图片加载方法
        Request requst = new Request.Builder().url(photoUrl).build();
        OkHttpClient okHttpClient = new OkHttpClient.Builder()
                .addInterceptor(new ProgressIntercept(getProgressListener()))
                .build();
        try {
            progressCall = okHttpClient.newCall(requst);
            Response response = progressCall.execute();
            if (isCancel) {
                return null;
            }
            if (!response.isSuccessful()) {
                throw new IOException("unexpected error" + response);
            }
            resultStream = response.body().byteStream();
        } catch (IOException e) {
            e.printStackTrace();
        }
        return resultStream;
    }

```

通过ProgressIntercept拦截器，监听进度
```
    @Override
    public Response intercept(Chain chain) throws IOException {
        Response response = chain.proceed(chain.request());
        // 拦截器替换response Body对象
        return response.newBuilder().body(new ProgressResponceBody(response.body(),mProgressListener)).build();
    }
```

在glide中使用：
```
 Glide.with(context)
                .using(new ProgressModelLoader(new Handler() {
                    @Override
                    public void handleMessage(Message msg) {
                        // 这部分的图片，都是通过OKHttp从网络获取的，如果改图片从glide缓存中读取，不会经过这儿
                        if (msg.what == ProgressListener.SEND_LOAD_PROGRESS) {
                            int totalReadBytes = msg.arg1;
                            int lengthBytes = msg.arg2;
                            int progressResult = (int) (((float) totalReadBytes / (float) lengthBytes) * 100);
                            mTvOriginPhoto.setText(progressResult + "%");
                            LogUtils.i("progress-result:-->" + progressResult + " msg.arg1-->" + msg.arg1 + "  msg.arg2-->" +
                                    msg.arg2 + " 比例-->" + progressResult + "%/" + "100%");
                            if (progressResult == 100) {
                                mTvOriginPhoto.setText(R.string.completed);
                            }
                        }
                    }
                }))
```
将图片进度回调给Handler进行相关处理；
## 图片保存
通过GLide获取bitmap
```
        // 通过GLide获取bitmap,有缓存读缓存
        Glide.with(getActivity())
                .load(String.format(ApiConfig.IMAGE_PATH, mImageBean.getStorage_id(), 100))
                .asBitmap()
                .into(new SimpleTarget<Bitmap>() {
                    @Override
                    public void onResourceReady(final Bitmap resource, GlideAnimation<? super Bitmap> glideAnimation) {
                        // 获取到bitmap，进行保存操作
                    }
                });
```

glide通过网络或者缓存获取到bitmap后，进行保存bitmap为image的操作

```
 Observable.just(1)// 不能empty否则map无法进行转换
                .map(new Func1<Integer, String>() {
                    @Override
                    public String call(Integer integer) {
                        String imgName = System.currentTimeMillis() + ".jpg";
                        String imgPath = PathConfig.PHOTO_SAVA_PATH;
                        return DrawableProvider.saveBitmap(bitmap, imgName, imgPath);
                    }
                })
                .subscribeOn(Schedulers.io())
                .observeOn(AndroidSchedulers.mainThread())
                .subscribe(new Action1<String>() {
                    @Override
                    public void call(String result) {
                    // 处理保存结果
                    }
```

上面的代码中，通过Rx进行线程转换，使得保存bitmap的操作在io中进行，避免阻塞主线程；

## 缩放动画
[缩放动画的实现](ACTIVITYANIMATION.md) <br>
Glide加载图片，在获取到图片并显示图片的时候加载动画
```
 Glide.with(context)
      ...
      .into(new GallarySimpleTarget(rect));
```

在SimpleTarget中获取到resource（缩略图的resource）的时候，进行放缩动画，添加
一个变量hasAnim，用来处理只有第一次加载图片是才有放缩动画的逻辑；
```
 private class GallarySimpleTarget extends SimpleTarget<GlideDrawable> {
        private AnimationRectBean rect;

        public GallarySimpleTarget(AnimationRectBean rect) {
            super();
            this.rect = rect;
        }

        @Override
        public void onResourceReady(GlideDrawable resource, GlideAnimation<? super GlideDrawable> glideAnimation) {
            if (resource == null) {
                return;
            }
            mPbProgress.setVisibility(View.GONE);
            mIvPager.setImageDrawable(resource);
            mPhotoViewAttacherNormal.update();
            // 获取到模糊图进行放大动画
            if (!hasAnim) {
                LogUtils.i(TAG + "加载缩略图成功");
                hasAnim = true;
                startInAnim(rect);
            }
        }

    }
```

## 缩略图加载
图片浏览器的加载策略是：
-->先加载缩略图（上一个页面已经缓存（显示）好的图片）；
-->然后尝试从缓存读取原图，如果有就加载原图，图片加载完成；
-->如果从缓存读取原图失败，就直接加载高清图（缓存或者网络），图片加载完成；

因此，加载策略的关键在于“尝试从缓存读取原图”；
```
 Glide.with(context)
                    .using(cacheOnlyStreamLoader)// 不从网络读取原图
...
```

Glide加载图片，如果来自于网络，会通过using操作实现，在cacheOnlyStreamLoader中，
获取数据的方法直接抛出异常，无法获取到网络数据，这样避免了glide加载网络图片的企图，
只会尝试从缓存获取图片；

```
 private static final StreamModelLoader<String> cacheOnlyStreamLoader = new StreamModelLoader<String>() {
        @Override
        public DataFetcher<InputStream> getResourceFetcher(final String model, int i, int i1) {
            return new DataFetcher<InputStream>() {
                @Override
                public InputStream loadData(Priority priority) throws Exception {
                    // 如果是从网络获取图片肯定会走这儿，直接抛出异常，缓存从其他方法获取
                    throw new IOException();
                }

                @Override
                public void cleanup() {

                }

                @Override
                public String getId() {
                    return model;
                }

                @Override
                public void cancel() {

                }
            };
        }
    };
```