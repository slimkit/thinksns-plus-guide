2017年5月11日 13:59:02
# 网络请求说明

##  1.概述
 - 结构
 [Retrofit](https://github.com/square/retrofit) +
[Dagger2](https://google.github.io/dagger/) + [Gson](https://github.com/google/gson) +                 [Rx](http://reactivex.io/)
 - 目的
 实现网络数据获取

## 2.定义
 - CommonRequestIntercept 可以定义统一请求内容（如：token，secret等）；
 - RequestIntercept 网络回掉拦截器；
 - RequestInterceptListener 网络拦截回掉，可做统一处理


## 3.使用
### Api配置
  - baseproject/config/ApiConfig 网络根域名、api接口配置
###

### 使用说明
#### 1.定义需要的`*Client`接口，可以仿写`CommonClient`

#### 2.在`ServiceModule`中提供你写的`*Client`

#### 3.在`ServiceManager`中的构造函数中注入`*Client`，并提供`get\set`方法

#### 4.在需要调用接口的地方通过注解获取`ServiceManager` 使用即可
```java
@Override
protected void setupActivityComponent(AppComponent appComponent) {
        DaggerUserComponent
                .builder()
                .appComponent(appComponent)// 必须的
                .userModule(new UserModule(this))
                .build()
                .inject(this);

    }
//第一种
@Module
public class UserModule {
    private UserContract.View view;

    /**
     * 构建UserModule时,将View的实现类传进来,这样就可以提供View的实现类给presenter
     * @param view
     */
    public UserModule(UserContract.View view) {
        this.view = view;
    }

    @ActivityScope
    @Provides
    UserContract.View provideUserView(){
        return this.view;
    }
   /**
   *Module中直接注解
   */
    @ActivityScope
    @Provides
    UserContract.Model provideUserModel(ServiceManager serviceManager, CacheManager cacheManager){
        return new UserModel(serviceManager,cacheManager);
    }
}
//第二种
Inject
ServiceManager mServiceManager;

```

#### 5. 网络数据请求，请查看[API](../app/API.md)
**注：**
如需很好的了解这一过程，必须对[Dagger2](DAGGER2.md)有很好的了解
## 4.逻辑描述

 - 扩展实现：
 1.拦截器
实现`Interceptor`接口，重新内部方法即可，并重写`BaseApplicaiton`的`getInterceptors`方法即可



