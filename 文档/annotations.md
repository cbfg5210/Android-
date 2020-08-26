###### 资源类型注解：
    AnimatorRes ：animator资源类型
    AnimRes：anim资源类型
    AnyRes：任意资源类型
    ArrayRes：array资源类型
    AttrRes：attr资源类型
    BoolRes：boolean资源类型
    ColorRes：color资源类型
    DimenRes：dimen资源类型。
    DrawableRes：drawable资源类型。
    FractionRes：fraction资源类型
    IdRes：id资源类型
    IntegerRes：integer资源类型
    InterpolatorRes：interpolator资源类型
    LayoutRes：layout资源类型
    MenuRes：menu资源类型
    PluralsRes：plurals资源类型
    RawRes：raw资源类型
    StringRes：string资源类型
    StyleableRes：styleable资源类型
    StyleRes：style资源类型
    TransitionRes：transition资源类型
    XmlRes：xml资源类型
    
###### Threading注解：
    @UiThread UI线程
    @MainThread 主线程
    @WorkerThread 子线程
    @BinderThread  绑定线程

###### Typedef注解：IntDef/StringDef
     整型除了可以作为资源的引用之外，也可以用作“枚举”类型使用。
     @IntDef和”typedef”作用非常类似，你可以创建另外一个注解，然后用@IntDef指定一个你期望的整型常量值列表，最后你就可以用这个定义好的注解修饰你的API了。
    
     例如：需要自定义网络类型:
        private final static int GET=0;
        private final static int POST=1;
        private final static int DELETE=2;
        private final static int PUT=3;
        @IntDef({GET, POST, DELETE,PUT})
        @Retention(RetentionPolicy.SOURCE)
        public @interface ReqType{}
        
     需要在调用时只能传入指定类型，如果传入类型不对，编译不通过:
     void testIndef(@ReqType int type){...}
     
###### Value Constraints注解
    在实际开发过程中，我们有时可能需要设置一个取值范围，这时我们可以使用取值范围注解来约束。
     @Size:
     栗子1：集合不能为空: @Size(min=1)
     栗子2：字符串最大只能有23个字符: @Size(max=23)
     栗子3：数组只能有2个元素: @Size(2)
     栗子4：数组的大小必须是2的倍数 (例如图形API中获取位置的x/y坐标数组: @Size(multiple=2)
     
     @IntRange:
     栗子：取值范围为0-100：@IntRange(from=0,to=100)
      
     @FloatRange
     
###### Permissions 注解: @RequiresPermission
    有时我们的方法调用需要调用者拥有指定的权限，这时我们可以使用@RequiresPermission注解。
    
    栗子1：需要单个权限：
    @RequiresPermission(Manifest.permission.SET_WALLPAPER)
    public abstract void setWallpaper(Bitmap bitmap) throws IOException;
    
    栗子2：至少需要权限集合中的一个：
    @RequiresPermission(anyOf = {
        Manifest.permission.ACCESS_COARSE_LOCATION,
        Manifest.permission.ACCESS_FINE_LOCATION})
    public abstract Location getLastKnownLocation(String provider);
      
     栗子3：同时需要多个权限：
    @RequiresPermission(allOf = {
        Manifest.permission.READ_HISTORY_BOOKMARKS, 
        Manifest.permission.WRITE_HISTORY_BOOKMARKS})
    public static final void updateVisitedHistory(ContentResolver cr, String url, boolean real) ;
      
     栗子4：对于intents的权限，可以直接在定义的intent常量字符串字段上标注权限需求(他们通常都已经被@SdkConstant注解标注过了)
    @RequiresPermission(android.Manifest.permission.BLUETOOTH)
    public static final String ACTION_REQUEST_DISCOVERABLE =
                "android.bluetooth.adapter.action.REQUEST_DISCOVERABLE";
    
    栗子5：对于content providers的权限，你可能需要单独的标注读和写的权限访问，所以可以用@Read或者@Write标注每一个权限需求
    @RequiresPermission.Read(@RequiresPermission(READ_HISTORY_BOOKMARKS))
    @RequiresPermission.Write(@RequiresPermission(WRITE_HISTORY_BOOKMARKS))
    public static final Uri BOOKMARKS_URI = Uri.parse("content://browser/bookmarks");
    
###### Overriding Methods 注解: @CallSuper
    如果你的API允许使用者重写你的方法，但是呢，你又需要你自己的方法(父方法)在重写的时候也被调用，这时候你可以使用@CallSuper标注
    
    栗子：Activity的onCreate函数：
    @CallSuper
    protected void onCreate(@Nullable Bundle savedInstanceState) 
    用了这个后，当重写的方法没有调用父方法时，工具就会给予标记提示
    
###### Return Values注解: @CheckResult
     如果你的方法返回一个值，你期望调用者用这个值做些事情，那么你可以使用@CheckResult注解标注这个方法。
     这个在具体使用中用的比较少，除非特殊情况，比如在项目中对一个数据进行处理，这个处理比较耗时，我们希望调用该函数的调用者在不需要处理结果的时候，提示没有使用，酌情删除调用。
    
###### 其他注解
    Keep：指出一个方法在被混淆的时候应该被保留。
    VisibleForTesting：可注解一个类，方法，或变量，表示有更宽松的可见性，这样它能够有更宽泛的可见性，使代码可以被测试。
    
    
    
### 什么是注解？
      对于很多初次接触的开发者来说应该都有这个疑问？Annotation是Java5开始引入的新特征，中文名称叫注解。它提供了一种安全的类似注释的机制，
      用来将任何的信息或元数据（metadata）与程序元素（类、方法、成员变量等）进行关联。为程序的元素（类、方法、成员变量）加上更直观更明了的说明，
      这些说明信息是与程序的业务逻辑无关，并且供指定的工具或框架使用。Annontation像一种修饰符一样，应用于包、类型、构造方法、方法、成员变量、
      参数及本地变量的声明语句中。

### 注解的用处：
      1、生成文档。这是最常见的，也是java 最早提供的注解。常用的有@param @return 等

      2、跟踪代码依赖性，实现替代配置文件功能。比如Dagger 2依赖注入，未来java开发，将大量注解配置，具有很大用处;

      3、在编译时进行格式检查。如@override 放在方法前，如果你这个方法并不是覆盖了超类方法，则编译时就能检查出。

#### 元注解：
    java.lang.annotation提供了四种元注解，专门注解其他的注解：

    @Documented –注解是否将包含在JavaDoc中
    @Retention –什么时候使用该注解
    @Target –注解用于什么地方
    @Inherited – 是否允许子类继承该注解

###### 1.）@Retention– 定义该注解的生命周期
    RetentionPolicy.SOURCE : 在编译阶段丢弃。这些注解在编译结束之后就不再有任何意义，所以它们不会写入字节码。@Override, @SuppressWarnings都属于这类注解。
    RetentionPolicy.CLASS : 在类加载的时候丢弃。在字节码文件的处理中有用。注解默认使用这种方式
    RetentionPolicy.RUNTIME : 始终不会丢弃，运行期也保留该注解，因此可以使用反射机制读取该注解的信息。我们自定义的注解通常使用这种方式。
    举例：bufferKnife 8.0 中@BindView 生命周期为CLASS

    @Retention(CLASS) @Target(FIELD)
        public @interface BindView {
        /** View ID to which the field will be bound. */
        @IdRes int value();
    }
    
###### 2.）Target – 表示该注解用于什么地方。默认值为任何元素，表示该注解用于什么地方。可用的ElementType参数包括:
    ElementType.CONSTRUCTOR:用于描述构造器
    ElementType.FIELD:成员变量、对象、属性（包括enum实例）
    ElementType.LOCAL_VARIABLE:用于描述局部变量
    ElementType.METHOD:用于描述方法
    ElementType.PACKAGE:用于描述包
    ElementType.PARAMETER:用于描述参数
    ElementType.TYPE:用于描述类、接口(包括注解类型) 或enum声明
    举例Retrofit 2 中@Field 作用域为参数

    @Documented
    @Target(PARAMETER)
    @Retention(RUNTIME)
    public @interface Field {
      String value();
    
      /** Specifies whether the {@linkplain #value() name} and value are already URL encoded. */
      boolean encoded() default false;
    }
     
###### 3.)@Documented–一个简单的Annotations标记注解，表示是否将注解信息添加在java文档中。
     
###### 4.)@Inherited – 定义该注释和子类的关系
     @Inherited 元注解是一个标记注解，@Inherited阐述了某个被标注的类型是被继承的。如果一个使用了@Inherited修饰的annotation类型被用于一个class，
     则这个annotation将被用于该class的子类。

     常见标准的Annotation：
      1.）Override
          java.lang.Override是一个标记类型注解，它被用作标注方法。它说明了被标注的方法重载了父类的方法，起到了断言的作用。如果我们使用了这种注解在一个
          没有覆盖父类方法的方法时，java编译器将以一个编译错误来警示。
    
      2.）Deprecated
         Deprecated也是一种标记类型注解。当一个类型或者类型成员使用@Deprecated修饰的话，编译器将不鼓励使用这个被标注的程序元素。
         所以使用这种修饰具有一定的“延续性”：如果我们在代码中通过继承或者覆盖的方式使用了这个过时的类型或者成员，
         虽然继承或者覆盖后的类型或者成员并不是被声明为@Deprecated，但编译器仍然要报警。
    
     3.）SuppressWarnings
         SuppressWarning不是一个标记类型注解。它有一个类型为String[]的成员，这个成员的值为被禁止的警告名。对于javac编译器来讲，
         被-Xlint选项有效的警告名也同样对@SuppressWarings有效，同时编译器忽略掉无法识别的警告名：@SuppressWarnings("unchecked") 
    
    
    自定义注解：
    这里模拟一个满足网络请求接口，以及如何获取接口的注解函数，参数执行请求。
    
    1.）定义注解：
    
    @ReqType 请求类型：
    
    @Documented
    @Target(METHOD)
    @Retention(RUNTIME)
    public @interface ReqType {
        /**
         * 请求方式枚举
         *
         */
        enum ReqTypeEnum{ GET,POST,DELETE,PUT};
    
        /**
         * 请求方式
         * @return
         */
        ReqTypeEnum reqType() default ReqTypeEnum.POST;
    }
    
    @ReqUrl 请求地址：
    
    @Documented
    @Target(METHOD)
    @Retention(RUNTIME)
    public @interface ReqUrl {
        String reqUrl() default "";
    }
    
    @ReqParam 请求参数：
    
    @Documented
    @Target(PARAMETER)
    @Retention(RUNTIME)
    public @interface ReqParam {
        String value() default "";
    }
   
    从上面可以看出注解参数的可支持数据类型有如下：
    1.所有基本数据类型（int,float,boolean,byte,double,char,long,short)
    2.String类型
    3.Class类型
    4.enum类型
    5.Annotation类型
    6.以上所有类型的数组
    
     而且不难发现@interface用来声明一个注解，其中的每一个方法实际上是声明了一个配置参数。方法的名称就是参数的名称，返回值类型就是参数的类型
     （返回值类型只能是基本类型、Class、String、enum）。可以通过default来声明参数的默认值。
    
    2.）如何使用自定义注解
    public interface IReqApi {
        @ReqType(reqType = ReqType.ReqTypeEnum.POST)//声明采用post请求
        @ReqUrl(reqUrl = "www.xxx.com/openApi/login")//请求Url地址
        String login(@ReqParam("userId") String userId, @ReqParam("pwd") String pwd);//参数用户名 密码
    }
    
    3.）如何获取注解参数
     这里强调一下，Annotation是被动的元数据，永远不会有主动行为，但凡Annotation起作用的场合都是有一个执行机制/调用者通过反射获得了这个元数据然后根据它采取行动。
    
    通过反射机制获取函数注解信息：
          Method[] declaredMethods = IReqApi.class.getDeclaredMethods();
            for (Method method : declaredMethods) {
                Annotation[]  methodAnnotations = method.getAnnotations();
                Annotation[][] parameterAnnotationsArray = method.getParameterAnnotations();
            }
            
    也可以获取指定的注解：
    ReqType reqType =method.getAnnotation(ReqType.class);
    
     4.）具体实现注解接口调用
    这里采用Java动态代理机制来实现，将定义接口与实现分离开，这个后期有时间再做总结。
    
    private void testApi() {
        IReqApi api = create(IReqApi.class);
        api.login("whoislcj", "123456");
    }
    
    public  T create(final Class service) {
        return (T) Proxy.newProxyInstance(service.getClassLoader(), new Class[]{service},
            new InvocationHandler() {
                @Override
                public Object invoke(Object proxy, Method method, Object... args)
                        throws Throwable {// Annotation[]  methodAnnotations = method.getAnnotations();//拿到函数注解数组
                    ReqType reqType = method.getAnnotation(ReqType.class);
                    Log.e(TAG, "IReqApi---reqType->" + (reqType.reqType() == ReqType.ReqTypeEnum.POST ? "POST" : "OTHER"));
                    ReqUrl reqUrl = method.getAnnotation(ReqUrl.class);
                    Log.e(TAG, "IReqApi---reqUrl->" + reqUrl.reqUrl());
                    Type[] parameterTypes = method.getGenericParameterTypes();
                    Annotation[][] parameterAnnotationsArray = method.getParameterAnnotations();//拿到参数注解
                    for (int i = 0; i < parameterAnnotationsArray.length; i++) {
                        Annotation[] annotations = parameterAnnotationsArray[i];
                        if (annotations != null) {
                            ReqParam reqParam = (ReqParam) annotations[0];
                            Log.e(TAG, "reqParam---reqParam->" + reqParam.value() + "==" + args[i]);
                        }
                    }
                    //下面就可以执行相应的网络请求获取结果 返回结果
                    String result = "";//这里模拟一个结果

                    return result;
                }
            });
    }