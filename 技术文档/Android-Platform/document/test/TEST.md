2017年5月11日 13:52:50
# app测试概述
按照测试用例，进行单元测试编写。为什么要写单元测试：
- 提高代码质量，写出良好的单元测试对代码有较高的要求
- 方便代码重构和迭代，添加或者修改功能后，能够通过单元测试检测出对其他功能的影响
- 更换开发人员后更快接手项目，单元测试作为之前程序员代码设计方式的呈现

## 测试框架
    JUnit4 + Mockito + Dagger2 + Robolectric
    这个是Android单元测试最广泛使用的组合
### JUnit4
- 当前版本 4.12
- Java界用的最广泛，也是最基础的一个框架，其他的很多框架，包括后面的Robolectric，
都是基于或兼容JUnit4的。

### Mockito
> mock就是创建一个类的虚假的对象，在测试环境中，用来替换掉真实的对象，以达到两大目的：
验证这个对象的某些方法的调用情况，调用了多少次，参数是什么等等
指定这个对象的某些方法的行为，返回特定的值，或者是执行特定的动作

反过来说如果想要实现上面的两个目的，需要一个特定的对象，而这个对象就需要Mockito框架提供。

### Monkey 压力测试
```java
/**
 * monkey 作用的包：com.ckt.android.junit
 * 产生时间序列的种子值：500
 * 忽略程序崩溃、 忽略超时、 监视本地程序崩溃、 详细信息级别为2， 产生10000个事件 。
 */
adb shell monkey -p com.zhiyicx.thinksnsplus -s 500 --ignore-crashes
--ignore-timeouts --monitor-native-crashes -v -v 10000 > E:\monkey_log\java_monkey_log.txt

```
强制停止`Monkey`测试
```java
adb shell ps | awk '/com\.android\.commands\.monkey/ { system("adb shell kill " $2) }'


```



### Dagger2
这里对[Dagger2进行了简单说明](../common/DAGGER2.md)
依赖注入框架Dagger2除了在项目中管理对象，减少创建对象时的耦合外，另外一个重要作用就是用于单元测试；
想想如果我们在单元测试中需要处理对象的依赖，这是一件非常麻烦的事情，Dagger2和Mockito结合使用，大大降低测试
用例的编写难度。

### Robolectric
我们的单元测试，往往涉及到Android相关联的类，也就是说和android运行环境关联，如果这样，每运行一个
测试单元就需要启动一次android模拟器或者手机（额滴天），Robolectric框架能够实现在JVM上无法调用安卓相关的类，
这样大大的提高了android平台的单元测试。

## 框架配置
src目录下：
  - androidTest目录：进行的是和android平台相关的测试，需要android模拟器或者真机的支持；如果第三方测试框架
  需要在该目录下运行，gradle配置以androidTestCompile开头
  - test目录：该目录下的测试会在JVM上运行，相比androidTest速度更快，不需要android模拟器或者真机的支持；如果第三方
  测试框架需要在该目录下运行，gradle配置以testCompile开头

## 在项目中使用上述的框架进行单元测试

### AndroidTest  依赖于android环境
   在app目录下的androidTest中，当前定义了如下的一些类：
   ```
   @RunWith(AndroidJUnit4.class)
   @LargeTest
   public abstract class AcitivityTest {

       /**
        * 清空输入框的内容
        *
        * @param viewInteraction
        */
       protected void clearEditText(ViewInteraction... viewInteraction) {
           if (viewInteraction != null) {
               for (ViewInteraction interaction : viewInteraction) {
                   interaction.perform(clearText());
               }
           }
       }

       /**
        * 根据控件id获取控件
        *
        * @param viewId
        * @return
        */
       protected ViewInteraction findViewById(int viewId) {
           return onView(withId(viewId));
       }

   }
   ```

   AcitivityTest是我们自定义的android测试的基类，在其中，定义了了一些常用的方法，当然有必要可以继续添加其他方法，这样做主要是
   方便调用测试方法，不需要反复导入static方法；另外因为添加了
   ```
      @RunWith(AndroidJUnit4.class)
      @LargeTest
   ```
   所以继承它的子类测试类，不需要再添加这些注解；

   ~~下面的类，是用来判断一些反面情况的，比如控件的enable为false~~
   ```
public final class MyViewMatchers {
    /**
     * Returns a matcher that matches {@link View}s that are disenabled.
     */
    public static Matcher<View> disEnabled() {
        return new TypeSafeMatcher<View>() {
            @Override
            public void describeTo(Description description) {
                description.appendText("is disable");
            }

            @Override
            public boolean matchesSafely(View view) {
                return !view.isEnabled();
            }
        };
    }
      ...
 }
   ```
~~在android的断言中，一般只有正面的断言，比如控件可点击，控件可见，为了让测试用例通过，我们需要模仿官方的正面断言方法，
自己添加反面的断言判断即可；~~

 **对于上面的逻辑，在使用过程中我们发现更好的实现方式，hamcrest的反向选择,如下所示**

    mRightBtn.check(matches(not(isEnabled())));


下面我就以登陆的测试为例，进行简单的说明：

首先，创建一个测试类,继承自AcitivityTest类:
```
public class LoginActivityTest extends AcitivityTest {
}
```

接下来我们添加启动activity的方法，当然可以在rule中添加activity的bundle:
```
    @Rule
    public ActivityTestRule<LoginActivity> mActivityRule = new ActivityTestRule(LoginActivity.class);
```

下面，我们对Activity进行初始化:
```
    private ViewInteraction etPhone, etPass, btnLogin, tvErrorTip;
    private LoginClient mLoginClient;
    @Before
    public void initActivity() {
        mLoginClient = AppApplication.AppComponentHolder.getAppComponent().serviceManager().getLoginClient();
        etPhone = findViewById(R.id.et_login_phone);
        etPass = findViewById(R.id.et_login_password);
        btnLogin = findViewById(R.id.bt_login_login);
        tvErrorTip = findViewById(R.id.tv_error_tip);
    }
```
- 在这里，虽然我们的登陆是写在LoginFragment中，但在测试中依然能够通过activity获取到这些控件；<br>
- 这里的findViewById()方法是我在AcitivityTest中自定义的方法；<br>
- 这里的LoginClient，是Retrofit创建的服务器请求接口，这里我通过AppApplication中的Dagger2获取到这个对象，
在下面的代码中进行网络请求；

接下来，我们就可以进行具体的测试：
```
    /**
     * summary    不输入密码，只输入手机号能否点击登录按钮
     * steps      1.清空输入框  2.输入手机号
     * expected   登录按钮无法点击
     */
    @Test
    public void clickableWhenNoPassword() throws Exception {
        clearEditText(etPhone, etPass);
        etPhone.perform(replaceText("15928856596"), closeSoftKeyboard());
        btnLogin.check(matches(isUnClickable()));
    }
```

上面的方法中，我们进行了 清空输入框 --> 输入手机号，并关闭软件盘 -->验证按钮是否可点击 的步骤；

下面的方法,是对登陆进行网络请求，这里我调用了rap的接口进行测试，在action的回调中进行断言
```
    /**
     * summary    因为某些原因导致登录失败，比如密码错误
     * steps        1.输入正确的手机号  2.输入错误的密码 3.点击登陆按钮
     * expected   errorTip显示登陆失败的原因
     */
    @Test
    public void loginFailure() throws Exception {
        mLoginClient.login("failure", "12344", "dsafdsa","fdsadfs")
                .subscribeOn(Schedulers.io())
                .observeOn(AndroidSchedulers.mainThread())
                .subscribe(new TestAction<BaseJson<AuthBean>>() {
                    @Override
                    void testCall(BaseJson<AuthBean> integerBaseJson) {
                        LogUtils.d("haha",integerBaseJson.toString());
                        if (integerBaseJson.isStatus()) {
                            // 登陆成功跳转:当前不可能发生
                           assertFalse(true);
                        } else {
                            // 登陆失败
                            assertFalse(false);
                        }
                    }
                }, new TestAction<Throwable>() {
                    @Override
                    void testCall(Throwable e) {
                        LogUtils.e(e,"exception");
                        //assertFalse(true);
                    }
                });
    }
```

- **但要注意的是，在上面的网络测试中，因为是异步线程，测试方法运行完后会关闭程序，而此时还在运行的异步线程会关闭，
我们无法进行断言，可以通过RxJava提供的RxJavaHooks进行线程改变，将异步，变成同步；**
- **在使用过程中，发现RxJava含有内建的、测试友好的解决方案，可以处理观察结果**
还有另外一种网络请求测试方法
```
    /**
     * summary    输入正确的手机号，密码登陆成功
     * steps      1.输入正确的手机号 2.输入正确的密码 3.点击登陆按钮 4.主线成沉睡1s等待网络请求结果
     * expected   errorTip显示登陆成功的内容
     */
    @Test
    public void loginSuccess() throws Exception {
        clearEditText(etPhone, etPass);
        etPhone.perform(replaceText("15928856596"), closeSoftKeyboard());
        etPass.perform(replaceText("123456"), closeSoftKeyboard());
        btnLogin.perform(click());
        Thread.sleep(1000);
        tvErrorTip.check(matches(withText("登陆失败")));
    }
```
在上面的方法中，直接调用了界面的登陆点击事件，主线程沉睡1s后（使用的rap，内部网络一般都在几百ms内），直接通过某些变化条件（比如提示信息内容）进行断言

## javaTest 直接在jvm上运行
   在app目录下的test中，我们会进行一些工具类的测试，比如字符串，手机号。。。的格式验证


如果对单元测试想要更全面的了解，推荐一篇博客:[关于安卓单元测试，你需要知道的一切](http://www.jianshu.com/p/dc30338a3e84)

## [测试报告](TESTREPPORT.md)


