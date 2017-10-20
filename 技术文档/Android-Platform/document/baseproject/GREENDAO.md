2017年5月11日 11:38:56
# GreenDao3.0+的使用

## 1.建立多表关联
使用greenDao建立表之间的关联关系，方便数据的查询；
###  1*1关系：
以项目中的粉丝关注列表和用户信息表为例，来说明该关系：
关注列表存储的主要类容：
```
    private long userId;// 主体用户：将要关注别人的人
    private int followState;// 关注状态 包含关注和相互关注
    private long followedUserId;// 被关注的用户
```
**我们在获取用户关注列表时，通过表关联能够同时返回用户信息表对应的用户信息；**

在关注的实体类中,添加关注用户信息实体，以及粉丝用户信息实体，也就是关联表：
```
@Entity
public class FollowFansBean implements Cloneable {

    // 存储服务器返回的maxId
    @Id
    private Long id;
    ...
    @ToOne(joinProperty = "userId")
    private UserInfoBean user;
    @ToOne(joinProperty = "followedUserId")
    private UserInfoBean fllowedUser;
```


**注意：关联表的查询：如果我想要FollowFansBean中携带用户信息，普通的查询方式返回的总是空的用户信息，需要使用queryDeep或者loadDeep方式查询**
以获取关注列表为例：
```
    /**
     * 获取某个人的关注列表的用户信息
     */
    public List<FollowFansBean> getSomeOneFollower(int userId) {
        FollowFansBeanDao followFansBeanDao = getRDaoSession().getFollowFansBeanDao();

        return followFansBeanDao.queryDeep("where " + FollowFansBeanDao
                .Properties.UserId.columnName + " = ? and " + FollowFansBeanDao.Properties.FollowState.columnName + " != ? ", userId + "", FollowFansBean.UNFOLLOWED_STATE + "");
    }
```

## 2.数据库升级

```
public class UpDBHelper extends DaoMaster.OpenHelper {

    public UpDBHelper(Context context, String name) {
        super(context, name);
    }

    // 注意选择GreenDao参数的onUpgrade方法
    @Override
    public void onUpgrade(Database db, int oldVersion, int newVersion) {
        LogUtils.i("greenDAO",
                "Upgrading schema from version " + oldVersion + " to " + newVersion + " by migrating all tables data");

        // 每次升级，将需要更新的表进行更新，第二个参数为要升级的Dao文件.
        //MigrationHelper.getInstance().migrate(db, UserInfoBeanDao.class);
        //MigrationHelper.getInstance().migrate(db, AuthBeanDao.class);
        //MigrationHelper.getInstance().migrate(db, BackgroundRequestTaskBeanDao.class);
        //MigrationHelper.getInstance().migrate(db, FollowFansBeanDao.class);
    }
}
```

1.每次对某个表结构进行修改后，需要修改greenDao数据库版本号schemaVersion：
    app的buildgradle.xml文件
   ```
   greendao {
       /**
        * daoPackage 生成的DAO，DaoMaster和DaoSession的包名。默认是实体的包名。
        * targetGenDir 生成源文件的路径。默认源文件目录是在build目录中的(build/generated/source/greendao)。
        * generateTests 设置是否自动生成单元测试。
        * targetGenDirTest 生成的单元测试的根目录。
        */
       schemaVersion 1
       //daoPackage 'com.zhiyicx.common.greendao.gen'
       //targetGenDir 'src/main/java'
   }
   ```
2.通知在UpDBHelper类的onUpgrade()方法中，对该表进行数据迁移；
    xxx表示该表的Dao
      MigrationHelper.getInstance().migrate(db, xxxDao.class);

