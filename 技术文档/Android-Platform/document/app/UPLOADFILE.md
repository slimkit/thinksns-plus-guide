2017年5月11日 11:34:05
# 文件上传

## 上传步骤：
 具体文档请参见[储存任务概述](https://github.com/zhiyicx/thinksns-plus/blob/master/documents/api/v1/storage-task-overview.md)
- 1.储存任务创建
- 2.上传文件
- 3.储存任务通知

## 单个文件上传
单个文件上传的逻辑在UpLoadRepository.class类中具体实现：<br>
**通过RxJava的操作符，实现整个上传的流程。**

```
mCommonClient.createStorageTask(paramMap, null)
                // 处理创建存储任务到上传文件的过程
                .flatMap(new Func1<BaseJson<StorageTaskBean>, Observable<String[]>>() {
                    @Override
                    public Observable<String[]> call(BaseJson<StorageTaskBean> storageTaskBeanBaseJson) {
                        // 服务器获取成功
                        if (storageTaskBeanBaseJson.isStatus()) {
                            StorageTaskBean storageTaskBean = storageTaskBeanBaseJson.getData();
                            int storageId = storageTaskBean.getStorage_id();
                            final int storageTaskId = storageTaskBean.getStorage_task_id();
                            if (storageId > 0) {
                                // 表示服务器已经存在该附件,已经成功上传，不需要做其他事情了
                                return Observable.just(new String[]{"success", storageTaskId + ""});
                            } else {
                                // 创建上传任务成功，开始上传
                                String method = storageTaskBean.getMethod();
                                String uri = storageTaskBean.getUri();
                                Gson gson = new Gson();
                                // 处理 headers
                                HashMap<String, String> headerMap;
                                try {
                                    headerMap = gson.fromJson(gson.toJson(storageTaskBean.getHeaders()),
                                            new TypeToken<HashMap<String, String>>() {
                                            }.getType());
                                } catch (Exception e) {
                                    e.printStackTrace();
                                    headerMap = new HashMap<String, String>();
                                }

                                // 处理 options
                                HashMap<String, Object> optionsMap;
                                try {
                                    optionsMap = gson.fromJson(gson.toJson(storageTaskBean.getOptions()),
                                            new TypeToken<HashMap<String, Object>>() {
                                            }.getType());
                                } catch (Exception e) {
                                    e.printStackTrace();
                                    optionsMap = new HashMap<String, Object>();
                                }
                                // 封装图片File
                                HashMap<String, String> fileMap = new HashMap<String, String>();
                                fileMap.put(storageTaskBean.getInput(), filePath);
                                if (method.equalsIgnoreCase("put")) {
                                    // 使用map操作符携带任务id，继续向下传递
                                    return mCommonClient.upLoadFileByPut(uri, headerMap, UpLoadFile.upLoadFileAndParams(fileMap, optionsMap))
                                            .map(new Func1<String, String[]>() {
                                                @Override
                                                public String[] call(String s) {
                                                    return new String[]{s, storageTaskId + ""};
                                                }
                                            });
                                } else if (method.equalsIgnoreCase("post")) {
                                    // 使用map操作符携带任务id，继续向下传递
                                    return mCommonClient.upLoadFileByPost(uri, headerMap, UpLoadFile.upLoadFileAndParams(fileMap, optionsMap))
                                            .map(new Func1<String, String[]>() {
                                                @Override
                                                public String[] call(String s) {
                                                    return new String[]{s, storageTaskId + ""};
                                                }
                                            });
                                } else {
                                    return Observable.just(new String[]{"failure", ""});// 没有合适的方法上传文件，这一般是不会发生的
                                }
                            }
                        } else {
                            // 表示服务器创建存储任务失败
                            return Observable.just(new String[]{"failure", ""});
                        }
                    }
                })
                // 处理上传文件到获取任务通知的过程
                .flatMap(new Func1<String[], Observable<BaseJson<Integer>>>() {
                    @Override
                    public Observable<BaseJson<Integer>> call(final String[] s) {
                        switch (s[0]) {
                            case "success":// 直接成功
                                BaseJson<Integer> success = new BaseJson<Integer>();
                                success.setData(Integer.parseInt(s[1]));
                                success.setStatus(true);
                                return Observable.just(success);
                            case "failure":// 失败
                                BaseJson<Integer> failure = new BaseJson<Integer>();
                                failure.setData(Integer.parseInt(s[1]));
                                failure.setStatus(false);
                                return Observable.just(failure);
                            default:// 调用通知任务
                                return mCommonClient.notifyStorageTask(s[1], s[0], null)
                                        .map(new Func1<BaseJson, BaseJson<Integer>>() {
                                            @Override
                                            public BaseJson<Integer> call(BaseJson baseJson) {
                                                BaseJson<Integer> newBaseJson = new BaseJson<Integer>();
                                                newBaseJson.setCode(baseJson.getCode());
                                                newBaseJson.setStatus(baseJson.isStatus());
                                                newBaseJson.setMessage(baseJson.getMessage());
                                                newBaseJson.setData(Integer.parseInt(s[1]));
                                                return newBaseJson;
                                            }
                                        });
                        }
                    }
                });
```

在上述代码中：<br>
**首先通过createStorageTask()接口方法，请求服务器创建存储任务；**<br>
通过rxJava的操作符flatMap(),处理服务器返回的存储任务的相关信息，正常情况下，服务器会返回两种结果中的一种：

- 情况1：秒传(表示文件已经上传过服务器，不需要再次上传)
```
{
    "storage_id": 7,
    "storage_task_id": 3
}
```
storage_id：表示任务的储存id，可以用来获取图片资源
storage_task_id：表示本次上传任务的任务id，可以用来通知服务器相关操作（比如这是我的用户头像），也可以用来删除上传

- 情况2：创建储存任务成功（文件还没有上传过，需要用户上传）
```
{
    "uri": "http://plus.io/api/v1/storages/1",
    "method": "PUT",
    "storage_task_id": 1,
    "headers": {
      "ACCESS-TOKEN": "fb0581e7a50d8a6fd19bed5b7f299b32"
    },
    "options": []
}
```
上述情况中：
uri：表示本次上传的上传地址.<br>
method：表示本次上传的网络请求方法.<br>
headers：表示本次上传的请求头.<br>
options：表示本次上传的请求体.<br>
在flatMap中通过判断创建存储任务的服务器返回结果，进行判断，服务器返回状态值正常，就进行上述两种情况的判断：
在情况2中，我们定义了两种上传方法“put”，“Post”，**如果以后文件上传的三方服务器返回了其他的method，需要
后续的开发人员添加上**，对于创建任务失败的情况，会继续向下传递失败消息，最终在Failure中处理，这里没有直接
抛出异常，是为了方便下面的多文件上传处理。<br>
<br>
**在处理完创建存储任务和上传文件后，接下来需要处理任务通知，将最终的结果返回给用户**
还是通过Rxjava的flatMap()操作符，处理上一步骤传递过来的数据，如果是success表示秒传成功，failure表示无法上传，default
表示接收到服务器上传的结果，将整个返回数据通过通知任务的接口传给服务器，服务器会对上传结果进行解析，然后返回给用户
上传结果，在这个过程中，通过Rxjava的map()操作符，将storage_id封装到服务器通知任务返回数据中，一起返回给用户，方便用户使用。<br>
<br>
这样整个上传任务就结束了。

## 多个文件上传
多个文件的上传，就是连续处理多个单个文件上传逻辑，以发送带图片的动态为例：<br>
```
 // 先处理图片上传，图片上传成功后，在进行动态发布
            List<Observable<BaseJson<Integer>>> upLoadPics = new ArrayList<>();
            for (int i = 0; i < photos.size(); i++) {
                String filePath = photos.get(i);
                upLoadPics.add(mUpLoadRepository.upLoadSingleFile("file" + i, filePath,true));
            }
            observable = // 组合多个图片上传任务
                    Observable.combineLatest(upLoadPics, new FuncN<List<Integer>>() {
                        @Override
                        public List<Integer> call(Object... args) {
                            // 得到图片上传的结果
                            List<Integer> integers = new ArrayList<Integer>();
                            for (Object obj : args) {
                                BaseJson<Integer> baseJson = (BaseJson<Integer>) obj;
                                if (baseJson.isStatus()) {
                                    integers.add(baseJson.getData());// 将返回的图片上传任务id封装好
                                } else {
                                    throw new NullPointerException();// 某一次失败就抛出异常，重传，因为有秒传功能所以不会浪费多少流量
                                }
                            }
                            return integers;
                        }
                    }).flatMap(new Func1<List<Integer>, Observable<BaseJson<Object>>>() {
                        @Override
                        public Observable<BaseJson<Object>> call(List<Integer> integers) {
                            List<ImageBean> imageBeens=new ArrayList<ImageBean>();
                            // 动态相关图片：图片任务id的数组，将它作为发布动态的参数
                            for (int i = 0; i < integers.size(); i++) {
                                imageBeens.add(new ImageBean(integers.get(i)));
                            }
                            dynamicDetailBean.setStorage_task_ids(imageBeens);
                            return mSendDynamicRepository.sendDynamic(dynamicDetailBean);// 进行动态发布的请求
                        }
                    });
```
在上述发送动态的图片上传逻辑处理中，需要处理最多九张图片的上传，然后将上传的任务id，作为动态发布接口的图片参数，进行动态发布；<br>
具体的逻辑描述：<br>
首先将多个单文件上传任务封装好，放到集合中，通过Rxjava的combineLatest()操作符，集中处理集合中的图片上传任务，在combineLatest中，
会得到图片上传任务的所有返回结果，对这些结果进行判断，如果有任何一张没有上传成功，就直接抛出异常，重新处理所有的图片上传任务，**因为服务器
会判断图片秒传，所以不需要担心会重复上传文件，浪费流量**，所有的图片上传成功后，通过flatMap()操作符，将所有的图片storage_task_id作为动态
上传的参数，进行相关处理。<br>
<br>
这样多文件上传的任务就结束了。

## 通过storage_id获取文件：
storage_id表示当前文件在服务器的存储表示，通过拼接：主机地址 + "api/v1/storages/storage_id" 的url，直接获取文件

## 删除任务：
暂无






