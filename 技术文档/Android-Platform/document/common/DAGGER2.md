2017年5月11日 13:58:57
# dagger2说明

##  1.概述
### 依赖注入的概述：
  - 首先依赖注入并不是什么高深的概念，我的小白理解：一个类A，里面需要用到类B，我们在类A的外部创建类B的对象，
    通过某种方式，将该对象引入类A中使用，这便是依赖注入了。类B是类A的依赖

  - 如何进行依赖注入，也不是一个高深的问题，我们平时肯定都使用过这种方式：
       1.依赖对象作为构造方法的参数注入
       2.依赖对象作为setter或者其他方法的参数注入


小白的理解：我们平时注入依赖需要自己手动new对象，然后作为构造方法或者setter方法的参数传入，如果A类依赖B类，B类依赖C类。。。
这时我想要创建A类对象，就需要依次注入依赖类对象，这样非常麻烦，另外，如果哪个依赖类需要改变构造方法参数，还需要在代码中
找到相应的构造方法进行修改，（-。-蓝瘦）
- Dagger2很好的解决了上面的这些问题，当然Dagger2还有很多其他的优点，比如单例的创建，。。。
- Dagger是一个完全静态的编译时依赖注入框架，用于Java和Android。 它是由Square创建的早期版本的改编，现在由Google维护。
- Dagger旨在解决基于反射的许多开发和性能问题，实现统一管理，代码解耦的目的。


## 2.定义

基础核心:

- `@Module`
Modules类里面的方法专门提供依赖，所以我们定义一个类，用@Module注解，这样Dagger在构造类的实例的时候，就知道从哪里去找到需要的 依赖。modules的一个重要特征是它们设计为分区并组合在一起（比如说，在我们的app中可以有多个组成在一起的modules

- ` @Provide`
在modules中，我们定义的方法是用这个注解，以此来告诉Dagger我们想要构造对象并提供这些依赖。

- `@Component`
 用于接口，这个接口被Dagger2用于生成用于模块注入的代码,是Module和Inject的连接器

- `@Inject`
在需要依赖的地方使用这个注解。（你用它告诉Dagger这个 构造方法，成员变量或者函数方法需要依赖注入。这样，Dagger就会构造一个这个类的实例并满足他们的依赖。）

- `@Scope`
Dagger2可以通过自定义注解限定注解作用域，ActivityScope限定作用于Activity的生命周期

- ` @Singleton`
当前提供的对象将是单例模式 ,一般配合@Provides一起出现

## 3.使用
1.第一步，定义一个`Module`,例如`AppModule`,用户提供`Applicaiton`
```java
@Module
public class AppModule {
    private Application mApplication;

    public AppModule(Application application) {
        this.mApplication = application;
    }

    @Singleton
    @Provides
    public Application provideApplication() {
        return mApplication;
    }

}
```
**注：** 类用@Module进行注解

2.第二步，编写`Component`,例如`AppComponent`，这里可以通过`@Component(modules = MessageModule.class, dependencies = ClientComponent.class)`关联到component也可以依赖其他的`component`
```java

@Singleton
@Component(modules = {AppModule.class, HttpClientModule.class, ServiceModule.class, CacheModule.class,ImageModule.class, ShareModule.class})
public interface AppComponent {
   //定义需要注入的对象
   // void inject(RetrofitClient client);
   //可以向外提供Application
    Application Application();

    //可以向外提供ServiceManager
    ServiceManager serviceManager();
}
//注入Retrofit
public class RetrofitClient implements Baseclient {
    @Inject
    Retrofit mRetrofit;

    public RetrofitClient() {
        DaggerClientComponent
                .builder()
                .clientModule(new ClientModule())
                .build()
                .inject(this);
    }

    @Override
    public <T> T initRetrofit(Class<T> restInterface) {
        return mRetrofit.create(restInterface);//返回用于请求的服务
    }
}

```
3.第三步，预编译`Component`，
```java
public class AppApplication extends TSApplication {
    @Override
    public void onCreate() {
        super.onCreate();
      mAppComponent = DaggerAppComponent
                .builder()
                .appModule(getAppModule())// baseApplication 提供
                .httpClientModule(getHttpClientModule())// baseApplication 提供
                .imageModule(getImagerModule())// // 图片加载框架
                .shareModule(getShareModule())// 分享框架
                .serviceModule(new ServiceModule())// 需自行创建
                .cacheModule(new CacheModule())// 需自行创建
                .build();
               // .inject(this); 由于Applicaiton中不要AppComponent进行注入，所以没有定义inject
```
**注:** 在使用`Component`时，使用的是`Dagger*Component`,你定义的为`*Component`,`Dagger*Component`实现类是需要编译一次，Dagger的apt自动帮你生成的，位置在`\build\generated\source\apt\debug\ +对应类的位置`

4.第四部，注入使用
```java
/**
* 前提
* AppComponent 中使用 void inject(RetrofitClient client);
* AppApplication 中的 DaggerAppComponent.inject(this);
* AppApplication 中就可以注入使用 ServiceManager了
*/
public class AppApplication extends TSApplication {

    @Inject
    ServiceManager mServiceManager;
    @Override
    public void onCreate() {
        super.onCreate();
      mAppComponent = DaggerAppComponent
                .builder()
                .appModule(getAppModule())// baseApplication 提供
                .httpClientModule(getHttpClientModule())// baseApplication 提供
                .imageModule(getImagerModule())// // 图片加载框架
                .shareModule(getShareModule())// 分享框架
                .serviceModule(new ServiceModule())// 需自行创建
                .cacheModule(new CacheModule())// 需自行创建
                .build();
               .inject(this); 由于Applicaiton中不要AppComponent进行注入，所以没有定义inject

```

建议查看官方文档学习[Dagger2](https://google.github.io/dagger/users-guide)





