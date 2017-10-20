2017年5月11日 11:41:28
# 图片选择器的功能整合
一般来说上传图片，包含这些功能，从本地相册选择图片，拍照获取图片，对图片进行裁剪；

## 1.功能整合
  ```
  public interface IPhotoSelector<T> {
      /**
       * 从本地相册中获取图片
       *
       * @param maxCount 每次能够获取的最大图片数量
       * @return
       */
      void getPhotoListFromSelector(int maxCount);

      /**
       * 通过拍照的方式获取图片
       */
      void getPhotoFromCamera();

      /**
       * 对图片进行裁剪
       */
      void startToCraft(String imgPath);

      /**
       * 判断是否需要裁剪
       */
      boolean isNeededCraft();


  }
  ```
  PhotoSelectorImpl类实现了上面的接口，来实现我们需要的功能，我们可以在isNeededCraft中定义什么时候下需要裁剪的规则；
  接下来需要处理这些结果：返回的图片信息，错误信息
  ```
      /**
       * 集中处理返回结果，一般在Fragment或者activity的onActivityResult方法中调用它
       *
       * @param requestCode
       * @param resultCode
       * @param data
       */
      public void onActivityResult(int requestCode, int resultCode, Intent data) {
          if (resultCode == RESULT_OK) {
              // 从本地相册选择图片
              if ((requestCode == PhotoPicker.REQUEST_CODE || requestCode == PhotoPreview.REQUEST_CODE)) {
                  List<String> photos = null;
                  if (data != null) {
                      photos = data.getStringArrayListExtra(PhotoPicker.KEY_SELECTED_PHOTOS);
                  }
                  // 是否需要剪裁，不需要就直接返回结果
                  if (isNeededCraft()) {
                      startToCraft(photos.get(0));
                  } else {
                      List<ImageBean> imageBeanList = new ArrayList<>();
                      ImageBean imageBean = new ImageBean();
                      imageBean.setImgUrl(photos.get(0));
                      imageBeanList.add(imageBean);
                      mTIPhotoBackListener.getPhotoSuccess(imageBeanList);
                  }
              }
              // 从相机中获取照片
              if (requestCode == CAMERA_PHOTO_CODE && mTakePhotoUri != null) {
                  String path = mTakePhotoUri.getPath();
                  if (new File(path).exists()) {
                      // 是否需要剪裁，不需要就直接返回结果
                      if (isNeededCraft()) {
                          startToCraft(path);
                      } else {
                          ImageBean imageBean = new ImageBean();
                          imageBean.setImgUrl(path);
                          List<ImageBean> imageBeanList = new ArrayList<>();
                          imageBeanList.add(imageBean);
                          mTIPhotoBackListener.getPhotoSuccess(imageBeanList);
                      }
                  } else {
                      mTIPhotoBackListener.getPhotoFailure("cannot get photo");
                  }
              }
              // 裁剪图片正确
              if (requestCode == UCrop.REQUEST_CROP) {
                  final Uri resultUri = UCrop.getOutput(data);
                  if (resultUri != null) {
                      List<ImageBean> photos = new ArrayList<>(1);
                      ImageBean imageBean = packageCropResult(data);
                      photos.add(imageBean);// 获取裁剪图片的路径
                      mTIPhotoBackListener.getPhotoSuccess(photos);
                  } else {
                      // 无法裁剪
                      mTIPhotoBackListener.getPhotoFailure("cannot crop");
                  }
              }
              // 裁剪图片错误
              if (resultCode == UCrop.RESULT_ERROR) {
                  final Throwable cropError = UCrop.getError(data);
                  if (cropError != null) {
                      ToastUtils.showToast(cropError.getMessage());
                  } else {
                      ToastUtils.showToast("Unexpected error");
                  }
                  mTIPhotoBackListener.getPhotoFailure(cropError.getMessage());
              }
          }
      }

      /**
       * 获取到最终图片后的回调接口
       */
      public interface IPhotoBackListener {
          void getPhotoSuccess(List<ImageBean> photoList);

          void getPhotoFailure(String errorMsg);
      }
  ```
  在上面的代码中onActivityResult方法，处理各个回调结果，在Activity或者fragment的onActivityResult方法中，
  调用此方法；IPhotoBackListener接口，是在获取结果后，用来集中处理图片或者错误信息，在Activity或者fragment
  实现该接口来展示图片；



 ## 2.如何使用
 - 通过Dagger注入PhotoSelectorImpl对象
 - 在Fragment的onActivityResult中调用onActivityResult
  ```
      @Override
      public void onActivityResult(int requestCode, int resultCode, Intent data) {
          super.onActivityResult(requestCode, resultCode, data);
          mPhotoSelector.onActivityResult(requestCode, resultCode, data);
      }
  ```

 - 实现接口IPhotoBackListener,处理返回结果
 ```
     @Override
     public void getPhotoSuccess(List<ImageBean> photoList) {
         String filePath = photoList.get(0).getImgUrl();
         File file = new File(filePath);
         // 开始上传
         mPresenter.changeUserHeadIcon(FileUtils.getFileMD5ToString(file), file.getName(), filePath);
         // 加载本地图片
         ImageLoader imageLoader = AppApplication.AppComponentHolder.getAppComponent().imageLoader();
         imageLoader.loadImage(getContext(), GlideImageConfig.builder()
                 .url(photoList.get(0).getImgUrl())
                 .imagerView(mIvHeadIcon)
                 .transformation(new GlideCircleTransform(getContext()))
                 .build());
     }

     @Override
     public void getPhotoFailure(String errorMsg) {
         ToastUtils.showToast(errorMsg);
     }

 ```
 - 调用图片选择方法
 ```
   // 从本地选择图片
   // maxCount 可选择的最大图片数量
   // selectedPhotos  已经被选择的图片
                       public void getPhotoListFromSelector(int maxCount, ArrayList<String> selectedPhotos);
 ```

 ```
  // 选择相机，拍照
  // selectedPhotos  已经被选择的图片
                         mPhotoSelector.getPhotoFromCamera(ArrayList<String> selectedPhotos);
 ```

