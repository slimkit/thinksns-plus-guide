2017年5月11日 11:37:30
# 缓存策略
  暂未使用
## 添加缓存主要达到以下的目的：

- 节省手机资源，频繁的网络请求对手机的cpu，电池，流量都是巨大的消耗；
- 增强用户体验，避免数据加载时间过久，造成界面卡顿或者缺失；

## 三级缓存：
我们常用的缓存模型一般都包含下面三个部分：
- 内存：加载数据
- 本地磁盘：持久化保存数据
- 网络请求：更新数据

几乎所有的图片加载框架都是用了上面的缓存策略

## android开发中的实现：
- 内存方面通过LruCache进行缓存，以Map键值对，对应缓存key和缓存内容；
- 本地磁盘方面通过SharedPreference,File，数据库进行持久化的数据存储；
- 网络请求方面通过OkHttp，Retrofit。。。各种各样的框架进行网络请求；

## 通过RxJava进行缓存的具体实现

**感谢[泛型模式下的Retrofit + rxJava实现三级缓存 ](http://blog.csdn.net/aishang5wpj/article/details/51692824)这篇文章和它的作者的idea**


```
public interface ICache<T extends CacheBean> {
    /**
     * 获取缓存的Observable
     *
     * @param key 缓存标识符
     */
    Observable<BaseJson<T>> get(String key);

    /**
     * 保存数据到缓存
     *
     * @param key 缓存标识符
     * @param t   需要缓存的对象
     */
    void put(String key, T t);

}
```

  上面的代码定义了缓存接口，内存和本地需要实现该接口，以便存取数据，当然我们后面还需要扩展该接口，添加更多的方法，以便修改缓存数据；

```
public class MemoryCache<T extends CacheBean> implements ICache<T> {
    // LruCache定义泛型：String类型的key，Object类型的缓存数据
    private LruCache<String, T> mLruCache;

    public MemoryCache() {
        // 系统分配给应用的最大内存
        int maxMemory = (int) Runtime.getRuntime().maxMemory();
        // 给当前应用分配最大缓存,应用最大内存的八分之一
        int cacheMemory = maxMemory / 8;
        mLruCache = new LruCache<String, T>(cacheMemory) {
            @Override
            protected int sizeOf(String key, T value) {
                return super.sizeOf(key, value);
            }
        };
    }

    @Override
    public Observable<BaseJson<T>> get(final String key) {
        Observable<BaseJson<T>> observable = Observable.create(new Observable.OnSubscribe<BaseJson<T>>() {
            @Override
            public void call(Subscriber<? super BaseJson<T>> subscriber) {
                // 取消订阅
                if (subscriber.isUnsubscribed()) {
                    return;
                }
                // 获取缓存数据
                T t = mLruCache.get(key);
                // 发射缓存数据
                BaseJson<T> baseJson = new BaseJson<T>();
                baseJson.setCode(0);
                baseJson.setData(t);
                baseJson.setStatus(true);
                subscriber.onNext(baseJson);
                subscriber.onCompleted();
            }
        });
        return observable;
    }

    @Override
    public void put(String key, T t) {
        mLruCache.put(key, t);
    }
}
```

在上面的代码中，通过LruCache实现内存缓存，在get方法中，通过RxJava发射从LreCache键值对中取出的缓存数据；

```
public class DiskCache<T extends CacheBean> implements ICache<T> {

    // 数据库实现类的公共接口
    private IDataBaseOperate<T> mDataBaseOperate;

    public DiskCache(IDataBaseOperate<T> commonCache) {
        mDataBaseOperate = commonCache;
    }

    @Override
    public Observable<BaseJson<T>> get(final String key) {
        Observable<BaseJson<T>> observable = Observable.create(new Observable.OnSubscribe<BaseJson<T>>() {
            @Override
            public void call(Subscriber<? super BaseJson<T>> subscriber) {
                if (subscriber.isUnsubscribed()) {
                    return;
                }
                T t = mDataBaseOperate.getSingleDataFromCache(key);
                BaseJson<T> baseJson = new BaseJson<T>();
                baseJson.setCode(0);
                baseJson.setData(t);
                baseJson.setStatus(true);
                subscriber.onNext(baseJson);
                subscriber.onCompleted();
            }
        }).subscribeOn(Schedulers.io()).observeOn(AndroidSchedulers.mainThread());
        return observable;
    }

    @Override
    public void put(String key, T t) {
        mDataBaseOperate.saveSingleData(t);
    }
}
```

本地文件缓存的实现,当前传入数据库的实现接口，通过数据库来获取数据，以及存储数据，下面是定义的数据库接口，通过[GreenDao](GREENDAO.md)实现
对Sqlite数据库进行增删查改操作；当然你也可以通过文件或者SharedPreference来处理；

```
public interface IDataBaseOperate<T> {
    /**
     * 保存服务器单条数据
     */
    void saveSingleData(T singleData);

    /**
     * 保存服务器多条数据
     */
    void saveMultiData(List<T> multiData);

    /**
     * 判断数据是否失效
     */
    boolean isInvalide();

    /**
     * 根据key(比如userID)，从缓存中获取单条数据
     */
    T getSingleDataFromCache(String key);

    /**
     * 获取表中所有的数据，（比如所有的好友）
     */
    List<T> getMultiDataFromCache();

    /**
     * 清空当前的表
     */
    void clearTable();

    /**
     * 根据key，删除缓存中的某条数据
     */
    void deleteSingleCache(String key);

    /**
     * 更新缓存中的某条数据
     */
    void updateSingleData(T newData);

}
```

接下来还需要定义网络层次的接口,用来从服务器请求数据
```
public interface NetWorkCache<T extends CacheBean> {
    Observable<BaseJson<T>> get(String key);
}
```

最后我们通过一个实现类，来集中处理上面的流程：

```
public class CacheImp<T extends CacheBean> {
    ...
    public Observable<BaseJson<T>> load(String key, NetWorkCache<T> networkCache) {
        return Observable.concat(
                loadFromMemory(key)
                loadFromDisk(key)
                , loadFromNetWork(key, networkCache)
        ).first(new Func1<BaseJson<T>, Boolean>() {
            @Override
            public Boolean call(BaseJson<T> o) {
                // 拿到第一条数据,通过是否过期判断
                if (o == null) {
                    String result = "no cache";
                    LogUtils.i(result, o);
                } else {
                    T data = o.getData();
                    String result = (data == null ? "no cache" : data.isExpire() ? "cache is expire" : "cache is ok");
                    LogUtils.i(result, o);
                    return o != null && data != null && !data.isExpire();
                }
                return false;
            }
        });
    }
    ...
}
```

上面通过RxJava的concat，first操作符，按照内存-磁盘-网络，有条件的获得数据，知道某一次成功结束本次操作；

注意：
在ts+项目中，我们在针对的图文动态，也就是类似微博列表这样的数据，可能需要单独对每一条信息进行点赞，评论，在内存的缓存处理中，
对于Map<K,V>的V，我们不能很方便快捷的，将V中的某一条数据进行修改，需要直接从数据库获取数据，因而，仅仅选择了本地磁盘+网络请求的结构；

