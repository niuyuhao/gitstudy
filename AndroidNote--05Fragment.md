# 5 探究Fragment

## 5.1 Fragment是什么

*Fragment是Android3.0后引入的一个新的API，他出现的初衷是**为了适应大屏幕的平板电脑**， 当然现在他仍然是平板APP UI设计的宠儿，而且我们普通手机开发也会加入这个Fragment， 我们可以把他**看成一个小型的Activity，又称Activity片段**！如果一个很大的界面，就一个布局，写起界面会麻烦，而且如果组件多的话是管理起来也很麻烦！而使用Fragment 我们可以把屏幕划分成几块，然后进行分组，进行一个模块化的管理！从而可以更加方便的在 运行过程中动态地更新Activity的用户界面！另外Fragment并不能单独使用，他需要嵌套在Activity 中使用，尽管他拥有自己的生命周期，但是还是会受到宿主Activity的生命周期的影响，比如Activity 被destory销毁了，他也会跟着销毁！*

[菜鸟教程](https://www.runoob.com/w3cnote/android-tutorial-fragment-base.html)

[官方文档](https://developer.android.com/guide/components/fragments.html)

![Fragment分别对应手机与平板间不同情况的处理图](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/41442282.jpg)

## 5.2 Fragment的使用方式

新建一个FragmentTest

### 5.2.1 Fragment的简单用法

在一个活动中添加两个碎片，并让这两个碎片平分活动空间

1. 新建左侧碎片布局 left_fragment.xml

   - ```xml
     <?xml version="1.0" encoding="utf-8"?>
     <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
         android:orientation="vertical"
         android:layout_width="match_parent"
         android:layout_height="match_parent">
         <Button
             android:id="@+id/button"
             android:layout_width="wrap_content"
             android:layout_height="wrap_content"
             android:layout_gravity="center_horizontal"
             android:text="button" />
     </LinearLayout>
     ```

2. 新建右侧碎片布局 right_fragment.xml

   - ```xml
     <?xml version="1.0" encoding="utf-8"?>
     <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
         android:orientation="vertical"
         android:background="#00ff00"
         android:layout_width="match_parent"
         android:layout_height="match_parent">
     
         <TextView
             android:layout_width="wrap_content"
             android:layout_height="wrap_content"
             android:layout_gravity="center_horizontal"
             android:textSize="20sp"
             android:text="这是右Fragment" />
     </LinearLayout>
     ```

3. 新建一个LeftFragment类 继承 Fragment

   - ``这里可能会有两个不同包下的Fragment可以选择:一个是系统内置的android.app.Fragment,一个是AndroidX库中的androidx.fragment.app.Fragment。这里使用AndroidX库中的Fragment,因为它可以让Fragment的特性在所有Android系统版本中保持一致,而系统内置的Fragment在Android 9.0版本中已被废弃。使用AndroidX库中的Fragment并不需要在build.gradle文件中添加额外的依赖,只要你在创建新项目时勾选了Use androidx.* artifacts选项,AndroidStudio会自动帮你导入必要的AndroidX库。``

   - ```Java
     public class LeftFragment extends Fragment {
         @Nullable
         @Override
         public View onCreateView(@NonNull LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
             //通过inflate方法将刚才定义的left_fragment.xml布局动态加载进来
             View view = inflater.inflate(R.layout.left_fragment, container, false);
             return view;
         }
     }
     ```

4. 同样新建一个RightFragment  使用inflate加载right_fragment

5. 修改activity_main.xml

   - ```xml
     <?xml version="1.0" encoding="utf-8"?>
     <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
         xmlns:app="http://schemas.android.com/apk/res-auto"
         xmlns:tools="http://schemas.android.com/tools"
         android:layout_width="match_parent"
         android:layout_height="match_parent"
         tools:context=".MainActivity">
         <!--需要通过android:name 属性来显式指明要添加的碎片类名  需要将类的包名也加上-->
         <fragment
             android:id="@+id/left_fragment"
             android:name="com.nyh.fragmenttest.LeftFragment"
             android:layout_width="0dp"
             android:layout_height="match_parent"
             android:layout_weight="1" />
         <fragment
             android:id="@+id/right_fragment"
             android:name="com.nyh.fragmenttest.RightFragment"
             android:layout_width="0dp"
             android:layout_height="match_parent"
             android:layout_weight="1" />
     </LinearLayout>
     ```


总结

1. 定义Fragment的布局，就是fragment显示内容的
2. 自定义一个Fragment类,需要继承Fragment或者他的子类,重写onCreateView()方法 在该方法中调用:inflater.inflate()方法加载Fragment的布局文件,接着返回加载的view对象
3. 在需要加载Fragment的Activity对应的布局文件中添加fragment的标签，name属性是全限定类名（包含Fragment的包名）
4. Activity在onCreate( )方法中调用setContentView()加载布局文件

### 5.2.2 动态添加Fragment

在程序运行时动态地添加到活动当中。

<img src="https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/29546434.jpg" alt="img" style="zoom: 80%;" />

1. 新建another_right_fragment.xml

   - ```xml
     <?xml version="1.0" encoding="utf-8"?>
     <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
         android:orientation="vertical"
         android:background="#ffff00"
         android:layout_width="match_parent"
         android:layout_height="match_parent">
         <TextView
             android:layout_width="wrap_content"
             android:layout_height="wrap_content"
             android:layout_gravity="center_horizontal"
             android:textSize="20sp"
             android:text="这是另一个 right Fragment" />
     </LinearLayout>
     ```

2. 新建AnotherRightFragment继承Fragment  动态加载布局新建好的布局

3. 修改activity_main.xml

   - ```xml
     <?xml version="1.0" encoding="utf-8"?>
     <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
         xmlns:app="http://schemas.android.com/apk/res-auto"
         xmlns:tools="http://schemas.android.com/tools"
         android:layout_width="match_parent"
         android:layout_height="match_parent"
         tools:context=".MainActivity">
         <fragment
             android:id="@+id/left_fragment"
             android:name="com.nyh.fragmenttest.LeftFragment"
             android:layout_width="0dp"
             android:layout_height="match_parent"
             android:layout_weight="1" />
         <FrameLayout
             android:id="@+id/right_layout"
             android:layout_width="0dp"
             android:layout_height="match_parent"
             android:layout_weight="1">
     
         </FrameLayout>
     </LinearLayout>
     ```

   - 将右侧碎片替换成了一个FrameLayout布局

4. 修改MainActivity中的代码  实现动态添加碎片的功能

   - ```java
     public class MainActivity extends AppCompatActivity implements View.OnClickListener{
     
         @Override
         protected void onCreate(Bundle savedInstanceState) {
             super.onCreate(savedInstanceState);
             setContentView(R.layout.activity_main);
             Button button = findViewById(R.id.button);
             button.setOnClickListener(this);
             
             replaceFragment(new RightFragment());
         }
     
         @Override
         public void onClick(View view) {
             switch (view.getId()){
                 case R.id.button:
                     replaceFragment(new AnotherRightFragment());
                     break;
             }
         }
         private void replaceFragment(Fragment fragment){
             FragmentManager fragmentManager = getSupportFragmentManager();
             FragmentTransaction transaction = fragmentManager.beginTransaction();
             transaction.replace(R.id.right_layout,fragment);
             transaction.commit();
         }
     }
     ```

   - 首先给左侧碎片中的按钮注册了一个点击事件，然后调用replaceFragment()方法动态添加了RightFragment这个碎片。当点击左侧碎片中的按钮时，又会调用replaceFragment()方法将右侧碎片替换成AnotherRightFragment。结合replaceFragment()方法中的代码可以看出，动态添加碎片主要分为5步。

     1. 创建待添加的碎片实例
     2. 获取FragmentManager，在活动中可以直接通过调用getSupportFragmentManager()方法得到
     3. 开启一个事务，通过调用beginTransaction()方法开启
     4. 向容器内添加或替换碎片，一般使用replace()方法实现，需要传入容器的id和待添加的碎片实例
     5. 提交事务，调用commit()方法实现

### 5.2.3 在Fragment中实现返回栈

通过按下back返回到上一个Fragment

修改MainActivity中的代码

![image-20220421110849736](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220421110849736.png)

在提交事务之前，调用了FragmentTransaction的addToBackStack()方法，它可以接收一个名字用于描述返回栈的状态，一般传入null即可。

### 5.2.4 Fragment和Activity之间进行交互（有疑问）

![img](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/45971686.jpg)

1. 组件获取

   - Fragment获得Activity中的组件: **getActivity().findViewById(R.id.list)；**
   - Activity获得Fragment中的组件(根据id和tag都可以)：**getFragmentManager.findFragmentByid(R.id.fragment1);**

2. 数据传递

   1. **Activit传递数据给Fragment:**

      > 在Activity中创建Bundle数据包,调用Fragment实例的setArguments(bundle) 从而将Bundle数据包传给Fragment,然后Fragment中调用getArguments获得 Bundle对象,然后进行解析就可以了

   2. **Fragment传递数据给Activity**

      > 在Fragment中定义一个内部回调接口,再让包含该Fragment的Activity实现该回调接口, Fragment就可以通过回调接口传数据了,回调

      - **Step 1:定义一个回调接口:(Fragment中)**

      ```
      /*接口*/  
      public interface CallBack{  
          /*定义一个获取信息的方法*/  
          public void getResult(String result);  
      }  
      ```

      - **Step 2：接口回调（Fragment中）**

      ```
      /*接口回调*/  
      public void getData(CallBack callBack){  
          /*获取文本框的信息,当然你也可以传其他类型的参数,看需求咯*/  
          String msg = editText.getText().toString();  
          callBack.getResult(msg);  
      }  
      ```

      - **Step 3:使用接口回调方法读数据(Activity中)**

      ```
      /* 使用接口回调的方法获取数据 */  
      leftFragment.getData(new CallBack() {  
       @Override  
             public void getResult(String result) {              /*打印信息*/  
                  Toast.makeText(MainActivity.this, "-->>" + result, 1).show();  
                  }
              }); 
      ```

      **总结** **->**在Fragment定义一个接口,接口中定义抽象方法,你要传什么类型的数据参数就设置为什么类型;
      **->**接着还有写一个调用接口中的抽象方法,把要传递的数据传过去
      **->**再接着就是Activity了,调用Fragment提供的那个方法,然后重写抽象方法的时候进行数据 的读取就可以了!!!

   3. **Fragment与Fragment之间的数据互传**

   > 其实这很简单,找到要接受数据的fragment对象,直接调用setArguments传数据进去就可以了 通常的话是replace时,即fragment跳转的时候传数据的,那么只需要在初始化要跳转的Fragment 后调用他的setArguments方法传入数据即可!
   > 如果是两个Fragment需要即时传数据,而非跳转的话,就需要先在Activity获得f1传过来的数据, 再传到f2了,就是以Activity为媒介~

   **示例代码如下：**

   ```
   FragmentManager fManager = getSupportFragmentManager( );
   FragmentTransaction fTransaction = fManager.beginTransaction();
   Fragmentthree t1 = new Fragmentthree();
   Fragmenttwo t2 = new Fragmenttwo();
   Bundle bundle = new Bundle();
   bundle.putString("key",id);
   t2.setArguments(bundle); 
   fTransaction.add(R.id.fragmentRoot, t2, "~~~");  
   fTransaction.addToBackStack(t1);  
   fTransaction.commit(); 
   ```

#### 5.2.4中的问题

​	getFragmentManager 和 getSupportFragmentManager 区别？

```
//报错
LeftFragment leftFragment = (LeftFragment)getFragmentManager().findFragmentById(R.id.left_fragment);
//正常
LeftFragment leftFragment = (LeftFragment) getSupportFragmentManager().findFragmentById(R.id.left_fragment);
```

## 5.3 Fragment的生命周期

### 5.3.1 Fragment的状态和回调

|   状态   |                             描述                             |
| :------: | :----------------------------------------------------------: |
| 运行状态 | 当一个Fragment所关联的Activity正处于运行状态时,该Fragment也处于运行状态。 |
| 暂停状态 | 当一个Activity进入暂停状态时(由于另一个未占满屏幕的Activity被添加到了栈顶),与它相关联的Fragment就会进入暂停状态。 |
| 停止状态 | 当一个Activity进入停止状态时,与它相关联的Fragment就会进入停止状态,或者通过调用FragmentTransaction的remove()、replace()方法将Fragment从Activity中移除,但在事务提交之前调用了addToBackStack()方法,这时的Fragment也会进入停止状态。总的来说,进入停止状态的Fragment对用户来说是完全不可见的,有可能会被系统回收。 |
| 销毁状态 | Fragment总是依附于Activity而存在,因此当Activity被销毁时,与它相关联的Fragment就会进入销毁状态。或者通过调用FragmentTransaction的remove()、replace()方法将Fragment从Activity中移除,但在事务提交之前并没有调用addToBackStack()方法,这时的Fragment也会进入销毁状态。 |

|       方法名        |                       描述                       |
| :-----------------: | :----------------------------------------------: |
|     onAttach()      |        当Fragment和Activity建立关联时调用        |
|   onCreateView()    |        为Fragment创建视图(加载布局)时调用        |
| onActivityCreated() | 确保与Fragment相关联的Activity已经创建完毕时调用 |
|   onDestroyView()   |        当与Fragment关联的视图被移除时调用        |
|     onDetach()      |        当Fragment和Activity解除关联时调用        |

<img src="https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220424134446264.png" alt="Fragment完整的生命周期" style="zoom:33%;" />



### 5.3.2 体验Fragment的生命周期

在FragmentTest中修改RightFragment中的代码

```Java
public class RightFragment extends Fragment {
    public static final String TAG = "RightFragment";

    @Override
    public void onAttach(@NonNull Context context) {
        super.onAttach(context);
        Log.d(TAG,"--------onAttach---------");
    }

    @Override
    public void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        Log.d(TAG,"--------onCreate---------");
    }

    @Nullable
    @Override
    public View onCreateView(@NonNull LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
        Log.d(TAG,"--------onCreateView---------");
        View view = inflater.inflate(R.layout.right_fragment, container, false);
        return view;
    }

    @Override
    public void onActivityCreated(@Nullable Bundle savedInstanceState) {
        super.onActivityCreated(savedInstanceState);
        Log.d(TAG,"--------onActivityCreated---------");
    }

    @Override
    public void onStart() {
        super.onStart();
        Log.d(TAG,"--------onStart---------");
    }

    @Override
    public void onResume() {
        super.onResume();
        Log.d(TAG,"--------onResume---------");
    }

    @Override
    public void onPause() {
        super.onPause();
        Log.d(TAG,"--------onPause---------");
    }

    @Override
    public void onStop() {
        super.onStop();
        Log.d(TAG,"--------onStop---------");
    }

    @Override
    public void onDestroyView() {
        super.onDestroyView();
        Log.d(TAG,"--------onDestroyView---------");
    }

    @Override
    public void onDestroy() {
        super.onDestroy();
        Log.d(TAG,"--------onDestroy---------");
    }
    @Override
    public void onDetach() {
        super.onDetach();
        Log.d(TAG,"--------onDetach---------");
    }
}
```

当RightFragment第一次被加载到屏幕上时,会依次执行onAttach()、onCreate()、onCreateView()、onActivityCreated()、onStart()和onResume()方法。

![启动程序时的打印日志](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220424140253062.png)

然后点击LeftFragment中的按钮，由于AnotherRightFragment替换了RightFragment,此时的RightFragment进入了停止状态,因此onPause()、onStop()和onDestroyView()方法会得到执行。

![替换成AnotherRightFragment时的打印日志](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220424140340752.png)

- 如果在替换的时候没有调用addToBackStack()方法,此时的RightFragment就会进入销毁状态,onDestroy()和onDetach()方法就会得到执行。
- ![image-20220424142157054](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220424142157054.png)

按下Back键,RightFragment会重新回到屏幕，由于RightFragment重新回到了运行状态,因此onCreateView()、
onActivityCreated()、onStart()和onResume()方法会得到执行。注意,此时onCreate()方法并不会执行,因为我们借助了addToBackStack()方法使得RightFragment并没有被销毁。

![返回RightFragment时的打印日志](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220424140436843.png)

再次按下Back键，依次执行onPause()、onStop()、onDestroyView()、onDestroy()和onDetach()方法,最终将Fragment销毁。

![退出程序时的打印日志](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220424140711821.png)



## 5.4 动态加载布局的技巧

程序根据设备的分辨率或屏幕大小,在运行时决定加载哪个布局

### 5.4.1 使用限定符

借助限定符(qualifier)实现在运行时判断程序应该是使用双页模式还是单页模式呢。

修改FragmentTest项目中的activity_main.xml文件  只留一个左侧Fragment，让其充满整个父布局。

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">
    <fragment
        android:id="@+id/left_fragment"
        android:name="com.nyh.fragmenttest.LeftFragment"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />
</LinearLayout>
```

在res目录下新建layout-large文件夹，新建一个activity_main.xml布局文件    包含两个碎片

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">
    <fragment
        android:id="@+id/left_fragment"
        android:name="com.nyh.fragmenttest.LeftFragment"
        android:layout_width="0dp"
        android:layout_height="match_parent"
        android:layout_weight="1"/>

    <fragment
        android:id="@+id/right_fragment"
        android:name="com.nyh.fragmenttest.RightFragment"
        android:layout_width="0dp"
        android:layout_height="match_parent"
        android:layout_weight="3" />

</LinearLayout>
```

然后将MainActivity中replaceFragment()方法里的代码注释掉，在平板上运行

<img src="https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220424145438678.png" alt="image-20220424145438678" style="zoom:50%;" />

在手机上运行

![image-20220424145522079](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220424145522079.png)

常见的限定符

| 屏幕特征 | 限定符 |                     描述                      |
| :------: | :----: | :-------------------------------------------: |
|   大小   | small  |            提供给小屏幕设备的资源             |
|          | normal |           提供给中等屏幕设备的资源            |
|          | large  |            提供给大屏幕设备的资源             |
|          | xlarge |           提供给超大屏幕设备的资源            |
|  分辨率  |  ldpi  |     提供给低分辨率设备的资源(120 dpi以下)     |
|          |  mdpi  |  提供给中等分辨率设备的资源(120 dpi~160 dpi)  |
|          |  hdpi  |   提供给高分辨率设备的资源(160 dpi~240 dpi)   |
|          | xhdpi  |  提供给超高分辨率设备的资源(240 dpi~320 dpi)  |
|          | xxhdpi | 提供给超超高分辨率设备的资源(320 dpi~480 dpi) |
|   方向   |  land  |             提供给横屏设备的资源              |
|          |  port  |             提供给竖屏设备的资源              |

### 5.4.2 使用最小宽度限定符

使用最小宽度限定符(smallest-width qualifier)，更加灵活地为不同设备加载布局,不管它们是不是被系统认定为large。最小宽度限定符允许**对屏幕的宽度指定一个最小值(以dp为单位),然后以这个最小值为临界点,屏幕宽度大于这个值的设备就加载一个布局,屏幕宽度小于这个值的设备就加载另一个布局**。

在res目录下新建layout-sw600dp文件夹,然后在这个文件夹下新建activity_main.xml布局

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="horizontal"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <fragment
        android:id="@+id/leftFrag"
        android:name="com.nyh.fragmenttest.LeftFragment"
        android:layout_width="0dp"
        android:layout_height="match_parent"
        android:layout_weight="1" />

    <fragment
        android:id="@+id/rightFrag"
        android:name="com.nyh.fragmenttest.RightFragment"
        android:layout_width="0dp"
        android:layout_height="match_parent"
        android:layout_weight="3" />

</LinearLayout>
```

这就意味着,当程序运行在屏幕宽度大于等于600 dp的设备上时,会加载layout-sw600dp/activity_main布局,当程序运行在屏幕宽度小于600 dp的设备上时,则仍然加载默认的layout/activity_main布局。

## 5.5 Fragment的最佳实践----简易版的新闻应用

1. 新建新闻实体类News

   - ```Java
     public class News {
         private String title;
         private String content;
     
         public String getTitle() {
             return title;
         }
     
         public void setTitle(String title) {
             this.title = title;
         }
     
         public String getContent() {
             return content;
         }
     
         public void setContent(String content) {
             this.content = content;
         }
     }
     ```

2. 新建布局文件news_content_frag.xml

   - ```xml
     <?xml version="1.0" encoding="utf-8"?>
     <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
         android:layout_width="match_parent"
         android:layout_height="match_parent">
     
         <LinearLayout
             android:id="@+id/visibility_layout"
             android:layout_width="match_parent"
             android:layout_height="match_parent"
             android:orientation="vertical"
             android:visibility="invisible" >
             <!--新闻标题-->
             <TextView
                 android:id="@+id/news_title"
                 android:layout_width="match_parent"
                 android:layout_height="wrap_content"
                 android:gravity="center"
                 android:padding="10dp"
                 android:textSize="20sp" />
             <!--使用一条细线分割-->
             <View
                 android:layout_width="match_parent"
                 android:layout_height="1dp"
                 android:background="#000" />
             <!--新闻内容-->
             <TextView
                 android:id="@+id/news_content"
                 android:layout_width="match_parent"
                 android:layout_height="0dp"
                 android:layout_weight="1"
                 android:padding="15dp"
                 android:textSize="18sp" />
     
         </LinearLayout>
         <View
             android:layout_width="1dp"
             android:layout_height="match_parent"
             android:layout_alignParentLeft="true"
             android:background="#000" />
     </RelativeLayout>
     ```

   - 新闻内容的布局主要可以分为两个部分，头部部分显示新闻标题，正文部分显示新闻内容，中间使用一条细线分隔开。这里的细线是利用View来实现的，将Vicw的宽或高设置为1dp，再通过background属性给细线设置一下颜色就可以了。这里设置成黑色。

3. 新建一个NewsContentFragment类，继承Fragment

   - ```Java
     public class NewsContentFragment extends Fragment {
         private View view;
         @Nullable
         @Override
         public View onCreateView(@NonNull LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
             view = inflater.inflate(R.layout.news_content_frag,container,false);
             return view;
         }
         public void refresh(String newsTitle,String newsContent){
             View visibilityLayout = view.findViewById(R.id.visibility_layout);
             visibilityLayout.setVisibility(View.VISIBLE);
             TextView newsTitleText = view.findViewById(R.id.news_title);
             TextView newsContentText = view.findViewById(R.id.news_content);
             newsTitleText.setText(newsTitle);    //刷新新闻标题
             newsContentText.setText(newsContent);//  刷新新闻内容
     
         }
     }
     ```

   - onCreateView方法里加载了上一步创建的news_content_frag这个布局。refresh方法，这个方法用于将新闻的标题和内容显示在页面上。

   - 到这就把新闻内容的碎片和布局都创建好了，这是在双页模式下使用的。

4. 在单页模式下使用，再创建一个活动  NewsContentActivity，将布局名制定成news_content，修改布局文件中的内容

   - ```xml
     <?xml version="1.0" encoding="utf-8"?>
     <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
         xmlns:app="http://schemas.android.com/apk/res-auto"
         xmlns:tools="http://schemas.android.com/tools"
         android:orientation="vertical"
         android:layout_width="match_parent"
         android:layout_height="match_parent"
         tools:context=".NewsContentActivity">
         <fragment
             android:id="@+id/news_content_fragment"
             android:name="com.nyh.fragmentbestpractice.NewsContentFragment"
             android:layout_width="match_parent"
             android:layout_height="match_parent" />
     </LinearLayout>
     ```

   - 直接在布局中引入了上一步创建的NewsContentFragment，相当于把news_content_frag布局的内容自动加了进来。

5. 修改 NewsContentActivity 中的代码

   - ```Java
     public class NewsContentActivity extends AppCompatActivity {
     
         public static void actionStart(Context context,String newsTitle,String newsContent){
             Intent intent = new Intent(context, NewsContentActivity.class);
             intent.putExtra("news_title",newsTitle);
             intent.putExtra("news_content",newsContent);
             context.startActivity(intent);
         }
     
         @Override
         protected void onCreate(Bundle savedInstanceState) {
             super.onCreate(savedInstanceState);
             setContentView(R.layout.news_content);
             //获取传入的新闻标题
             String newsTitle = getIntent().getStringExtra("news_title");
             //获取传入的新闻内容
             String newsContent = getIntent().getStringExtra("news_content");
             NewsContentFragment newsContentFragment = (NewsContentFragment) getSupportFragmentManager().findFragmentById(R.id.news_content_fragment);
             newsContentFragment.refresh(newsTitle,newsContent);  //刷新NewsContentFragment界面
         }
     }
     ```

   - 在onCreate方法中通过Intent获取到了传入的新闻标题和新闻内容；然后获取到NewsContentFragment的实例，然后调用它的refresh方法，将获取到的新闻标题和新闻内容传入。

     - actionStart方法   ---》 启动活动的最佳写法（Activity笔记中）

6. 再创建一个用于显示新闻列表的布局  news_title_frag.xml

   - ```xml
     <?xml version="1.0" encoding="utf-8"?>
     <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
         android:orientation="vertical"
         android:layout_width="match_parent"
         android:layout_height="match_parent">
         <androidx.recyclerview.widget.RecyclerView
             android:id="@+id/news_title_recycler_view"
             android:layout_width="match_parent"
             android:layout_height="match_parent" />
     </LinearLayout>
     ```

   - 里面只有一个用于显示新闻列表的RecyclerView。

7. 创建news_item.xml作为RecyclerView子项的布局

   - ```xml
     <?xml version="1.0" encoding="utf-8"?>
     <!--
         android:singleLine="true"    表示让这个TextView只能单行显示
         android:ellipsize="end"      用于设定当文本内容超出控件宽度时，文本的缩略方式，end表示在尾部进行缩略
         android:padding      表示给控件的周围加上补白，这样不至于让文本内容会紧靠在边缘上
     -->
     <TextView xmlns:android="http://schemas.android.com/apk/res/android"
         android:id="@+id/news_title"
         android:layout_width="match_parent"
         android:layout_height="wrap_content"
         android:singleLine="true"
         android:ellipsize="end"
         android:textSize="18sp"
         android:paddingLeft="10dp"
         android:paddingRight="10dp"
         android:paddingTop="15dp"
         android:paddingBottom="15dp">
     </TextView>
     ```

   - 属性

     - android:padding      表示给控件的周围加上补白，这样不至于让文本内容会紧靠在边缘上
     - android:singleLine="true"    表示让这个TextView只能单行显示
     - android:ellipsize="end"      用于设定当文本内容超出控件宽度时，文本的缩略方式，end表示在尾部进行缩略

8. 新建NewsTitleFragment 作为展示新闻列表的碎片

   - ```Java
     public class NewsTitleFragment extends Fragment {
         private boolean isTwopage;
     
         @Nullable
         @Override
         public View onCreateView(@NonNull LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
             View view = inflater.inflate(R.layout.news_title_frag, container, false);
             return view;
         }
     
         @Override
         public void onActivityCreated(@Nullable Bundle savedInstanceState) {
             super.onActivityCreated(savedInstanceState);
             if (getActivity().findViewById(R.id.news_content_layout)!= null){
                 isTwopage = true;  //找到news_content_fragment布局时，为双页模式
             } else {
                 isTwopage = false; //找不到为单页模式
             }
         }
     }
     
     ```

   - 在onCreateView方法中，加载news_title_frag布局。

   - 在onActivityCreated中通过能否找到一个id为news_content_layout的view来判断当前是双页模式还是单页模式

     1. 先修改activity_main.xml

        - ```xml
          <?xml version="1.0" encoding="utf-8"?>
          <FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
              xmlns:app="http://schemas.android.com/apk/res-auto"
              xmlns:tools="http://schemas.android.com/tools"
              android:id="@+id/news_title_layout"
              android:layout_width="match_parent"
              android:layout_height="match_parent"
              tools:context=".MainActivity">
          
              <fragment
                  android:id="@+id/news_title_fragment"
                  android:name="com.nyh.fragmentbestpractice.NewsTitleFragment"
                  android:layout_width="match_parent"
                  android:layout_height="match_parent" />
          </FrameLayout>
          ```

        - 表示单页模式下，只会加载一个新闻标题的碎片

     2. 新建layout-sw600dp文件夹，在这个文件夹下再新建一个activity_main.xml

        - ```xml
          <?xml version="1.0" encoding="utf-8"?>
          <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
              xmlns:app="http://schemas.android.com/apk/res-auto"
              xmlns:tools="http://schemas.android.com/tools"
              android:orientation="horizontal"
              android:layout_width="match_parent"
              android:layout_height="match_parent"
              tools:context=".MainActivity">
          
              <fragment
                  android:id="@+id/news_title_fragment"
                  android:name="com.nyh.fragmentbestpractice.NewsTitleFragment"
                  android:layout_width="0dp"
                  android:layout_height="match_parent"
                  android:layout_weight="1"/>
              <FrameLayout
                  android:id="@+id/news_content_layout"
                  android:layout_width="0dp"
                  android:layout_height="match_parent"
                  android:layout_weight="3" >
                  <fragment
                      android:id="@+id/news_content_fragment"
                      android:name="com.nyh.fragmentbestpractice.NewsContentFragment"
                      android:layout_width="match_parent"
                      android:layout_height="match_parent" />
              </FrameLayout>
          </LinearLayout>
          ```

        - 双页模式下引入了两个碎片，并将新闻内容的碎片放在FrameLayout布局下，布局id为news_content_layout。能够找到这个id的时候就是双页模式

9. 在NewsTitleFragment中通过RecyclerView将新闻列表展示出来

   1. 新建一个内部类NewsAdapter作为RecyclerView的适配器

      - ```java
        public class NewsTitleFragment extends Fragment {
            private boolean isTwopage;
            ...
            class NewsAdapter extends RecyclerView.Adapter<NewsAdapter.ViewHolder>{
                private List<News> mNewsList;
        
                class ViewHolder extends RecyclerView.ViewHolder{
                    TextView newsTitleText;
        
                    public ViewHolder(@NonNull View itemView) {
                        super(itemView);
                        newsTitleText = itemView.findViewById(R.id.news_title);
                    }
                }
        
                public NewsAdapter(List<News> newsList) {
                    mNewsList = newsList;
                }
        
                @NonNull
                @Override
                public ViewHolder onCreateViewHolder(@NonNull ViewGroup parent, int viewType) {
                    View view = LayoutInflater.from(parent.getContext()).inflate(R.layout.news_item,parent,false);
                    final ViewHolder holder = new ViewHolder(view);
                    view.setOnClickListener(new View.OnClickListener() {
                        @Override
                        public void onClick(View view) {
                            News news = mNewsList.get(holder.getAdapterPosition());
                            if (isTwopage) {
                                //如果是双页模式，则刷新NewsContentFragment中的内容
                                NewsContentFragment newsContentFragment = (NewsContentFragment) getFragmentManager().findFragmentById(R.id.news_content_fragment);
                                newsContentFragment.refresh(news.getTitle(),news.getContent());
                            } else {
                                //如果是单页模式，则直接启动NewsContentActivity
                                NewsContentActivity.actionStart(getActivity(),news.getTitle(),news.getContent());
                            }
                        }
                    });
                    return holder;
                }
        
                @Override
                public void onBindViewHolder(@NonNull ViewHolder holder, int position) {
                    News news = mNewsList.get(position);
                    holder.newsTitleText.setText(news.getTitle());
                }
        
                @Override
                public int getItemCount() {
                    return mNewsList.size();
                }
            }
        }
        
        ```

      - 这里写成内部类的好处就是可以直接访问NewsTitleFragment的变量,比如isTwoPane。onCreateViewHolder()方法中注册的点击事件,首先获取了点击项的News实例,然后通过isTwoPane变量判断当前是单页还是双页模式。如果是单页模式,就启动一个新的Activity去显示新闻内容;如果是双页模式,就更新NewsContentFragment里的数据。

10. 想RecyclerView中填充数据，修改NewsTitleFragment

    - ```java
      public class NewsTitleFragment extends Fragment {
          private boolean isTwopage;
      
          @Nullable
          @Override
          public View onCreateView(@NonNull LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
              View view = inflater.inflate(R.layout.news_title_frag, container, false);
              RecyclerView newsTitleRecyclerView = view.findViewById(R.id.news_title_recycler_view);
              LinearLayoutManager layoutManager = new LinearLayoutManager(getActivity());
              newsTitleRecyclerView.setLayoutManager(layoutManager);
              NewsAdapter adapter = new NewsAdapter(getNews());
              newsTitleRecyclerView.setAdapter(adapter);
              return view;
          }
      
          @Override
          public void onActivityCreated(@Nullable Bundle savedInstanceState) {
              super.onActivityCreated(savedInstanceState);
              if (getActivity().findViewById(R.id.news_content_layout)!= null){
                  isTwopage = true;  //找到news_content_fragment布局时，为双页模式
              } else {
                  isTwopage = false; //找不到为单页模式
              }
          }
      
          class NewsAdapter extends RecyclerView.Adapter<NewsAdapter.ViewHolder>{
              private List<News> mNewsList;
      
              class ViewHolder extends RecyclerView.ViewHolder{
                  TextView newsTitleText;
      
                  public ViewHolder(@NonNull View itemView) {
                      super(itemView);
                      newsTitleText = itemView.findViewById(R.id.news_title);
                  }
              }
      
              public NewsAdapter(List<News> newsList) {
                  mNewsList = newsList;
              }
      
              @NonNull
              @Override
              public ViewHolder onCreateViewHolder(@NonNull ViewGroup parent, int viewType) {
                  View view = LayoutInflater.from(parent.getContext()).inflate(R.layout.news_item,parent,false);
                  final ViewHolder holder = new ViewHolder(view);
                  view.setOnClickListener(new View.OnClickListener() {
                      @Override
                      public void onClick(View view) {
                          News news = mNewsList.get(holder.getAdapterPosition());
                          if (isTwopage) {
                              //如果是双页模式，则刷新NewsContentFragment中的内容
                              NewsContentFragment newsContentFragment = (NewsContentFragment) getFragmentManager().findFragmentById(R.id.news_content_fragment);
                              newsContentFragment.refresh(news.getTitle(),news.getContent());
                          } else {
                              //如果是单页模式，则直接启动NewsContentActivity
                              NewsContentActivity.actionStart(getActivity(),news.getTitle(),news.getContent());
                          }
                      }
                  });
                  return holder;
              }
      
              @Override
              public void onBindViewHolder(@NonNull ViewHolder holder, int position) {
                  News news = mNewsList.get(position);
                  holder.newsTitleText.setText(news.getTitle());
              }
      
              @Override
              public int getItemCount() {
                  return mNewsList.size();
              }
          }
      
      
          private List<News> getNews(){
              List<News> newsList = new ArrayList<>();
              for (int i = 0; i <= 50; i++) {
                  News news = new News();
                  news.setTitle("this is news title" + i);
                  news.setContent(getRandomLengthContent("this is news content" + i + "."));
                  newsList.add(news);
              }
              return newsList;
          }
      
          private String getRandomLengthContent(String content) {
              Random random = new Random();
              int length = random.nextInt(20) + 1;
              StringBuilder builder = new StringBuilder();
              for (int i = 0; i < length; i++) {
                  builder.append(content);
              }
              return builder.toString();
          }
      ```

## 熟练Fragment-01

![img](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/53574832.jpg)

1. 写下底部选项的一些资源文件

   - 图片Drawable资源：**tab_menu_channel.xml**

     - ```xml
       <?xml version="1.0" encoding="utf-8"?>
       <selector xmlns:android="http://schemas.android.com/apk/res/android">
           <item android:drawable="@mipmap/tab_channel_pressed" android:state_selected="true" />
           <item android:drawable="@mipmap/tab_channel_normal" />
       </selector>
       ```

   - 文字资源：**tab_menu_text.xml**

     - ```xml
       <?xml version="1.0" encoding="utf-8"?>
       <selector xmlns:android="http://schemas.android.com/apk/res/android">
           <item android:color="@color/text_yellow" android:state_selected="true" />
           <item android:color="@color/text_gray" />
       </selector>
       ```

   - 背景资源：**tab_menu_bg.xml**

     - ```xml
       <?xml version="1.0" encoding="utf-8"?>
       <selector xmlns:android="http://schemas.android.com/apk/res/android">
           <item android:state_selected="true">
               <shape>
                   <solid android:color="#FFC4C4C4" />
               </shape>
           </item>
           <item>
               <shape>
                   <solid android:color="@color/transparent" />
               </shape>
           </item>
       </selector>
       ```

2. 编写Activity布局

   - ```xml
     <?xml version="1.0" encoding="utf-8"?>
     <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
         xmlns:app="http://schemas.android.com/apk/res-auto"
         xmlns:tools="http://schemas.android.com/tools"
         android:layout_width="match_parent"
         android:layout_height="match_parent"
         tools:context=".MainActivity">
     
     <!--
         首先定义顶部标题栏的样式，48dp的LinearLayout中间加上一个TextView作为标题！
         接着定义一个大小为56dp的LinerLayout对齐底部，在这个里面加入四个TextView，比例1:1:1:1， 并且设置相关属性，接着在这个LinearLayout上加一条线段！
         最后以标题栏和底部导航栏为边界，写一个FrameLayout，宽高match_parent，用做Fragment的容器！
     -->
     
         <RelativeLayout
             android:id="@+id/ly_top_bar"
             android:layout_width="match_parent"
             android:layout_height="48dp"
             android:background="@color/bg_topbar">
     
             <TextView
                 android:id="@+id/txt_topbar"
                 android:layout_width="match_parent"
                 android:layout_height="match_parent"
                 android:layout_centerInParent="true"
                 android:gravity="center"
                 android:text="信息"
                 android:textColor="@color/text_topbar"
                 android:textSize="18sp" />
     
             <View
                 android:layout_width="match_parent"
                 android:layout_height="2px"
                 android:layout_alignParentBottom="true"
                 android:background="@color/div_white" />
         </RelativeLayout>
     
         <LinearLayout
             android:id="@+id/ly_tab_bar"
             android:layout_width="match_parent"
             android:layout_height="56dp"
             android:layout_alignParentBottom="true"
             android:background="@color/bg_white"
             android:orientation="horizontal">
     
             <TextView
                 android:id="@+id/txt_channel"
                 android:layout_width="0dp"
                 android:layout_height="match_parent"
                 android:layout_weight="1"
                 android:background="@drawable/tab_menu_bg"
                 android:drawablePadding="3dp"
                 android:drawableTop="@drawable/tab_menu_channel"
                 android:gravity="center"
                 android:padding="5dp"
                 android:text="@string/tab_menu_alert"
                 android:textColor="@drawable/tab_menu_text"
                 android:textSize="16sp" />
     
             <TextView
                 android:id="@+id/txt_message"
                 android:layout_width="0dp"
                 android:layout_height="match_parent"
                 android:layout_weight="1"
                 android:background="@drawable/tab_menu_bg"
                 android:drawablePadding="3dp"
                 android:drawableTop="@drawable/tab_menu_message"
                 android:gravity="center"
                 android:padding="5dp"
                 android:text="@string/tab_menu_profile"
                 android:textColor="@drawable/tab_menu_text"
                 android:textSize="16sp" />
     
             <TextView
                 android:id="@+id/txt_better"
                 android:layout_width="0dp"
                 android:layout_height="match_parent"
                 android:layout_weight="1"
                 android:background="@drawable/tab_menu_bg"
                 android:drawablePadding="3dp"
                 android:drawableTop="@drawable/tab_menu_better"
                 android:gravity="center"
                 android:padding="5dp"
                 android:text="@string/tab_menu_pay"
                 android:textColor="@drawable/tab_menu_text"
                 android:textSize="16sp" />
     
             <TextView
                 android:id="@+id/txt_setting"
                 android:layout_width="0dp"
                 android:layout_height="match_parent"
                 android:layout_weight="1"
                 android:background="@drawable/tab_menu_bg"
                 android:drawablePadding="3dp"
                 android:drawableTop="@drawable/tab_menu_setting"
                 android:gravity="center"
                 android:padding="5dp"
                 android:text="@string/tab_menu_setting"
                 android:textColor="@drawable/tab_menu_text"
                 android:textSize="16sp"/>
     
         </LinearLayout>
     
         <!-- 在底部导航栏上方 android:layout_above="@id/ly_tab_bar" -->
         <View
             android:id="@+id/div_tab_bar"
             android:layout_width="match_parent"
             android:layout_height="2px"
             android:background="@color/div_white"
             android:layout_above="@id/ly_tab_bar"/>
     
         <!-- 在顶部下方 android:layout_below="@id/ly_top_bar" -->
         <FrameLayout
             android:id="@+id/ly_content"
             android:layout_width="match_parent"
             android:layout_height="match_parent"
             android:layout_above="@id/div_tab_bar"
             android:layout_below="@id/ly_top_bar"></FrameLayout>
     </RelativeLayout>
     ```

3. 隐藏顶部导航栏

   1. 把 requestWindowFeature(Window.FEATURE_NO_TITLE);放在super.onCreate(savedInstanceState);
   2. 接着**AndroidManifest.xml**设置下theme属性 `android:theme="@style/Theme.AppCompat.NoActionBar"`

4. 创建一个Fragment的简单布局与类

   - fg_content.xml

     - ```xml
       <?xml version="1.0" encoding="utf-8"?>
       <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
           android:orientation="vertical"
           android:layout_width="match_parent"
           android:layout_height="match_parent"
           android:background="@color/bg_white">
           <TextView
               android:id="@+id/txt_content"
               android:layout_width="match_parent"
               android:layout_height="match_parent"
               android:gravity="center"
               android:text="哈哈"
               android:textColor="@color/text_yellow"
               android:textSize="20sp" />
       </LinearLayout>
       ```

   - MyFragment.java

     - ```java
       public class MyFragment extends Fragment {
           private String content;
           public MyFragment(String content) {
               this.content = content;
           }
       
           @Override
           public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {
               View view = inflater.inflate(R.layout.fg_content,container,false);
               TextView txt_content = (TextView) view.findViewById(R.id.txt_content);
               txt_content.setText(content);
               return view;
           }
       }
       ```

5. 编写MainActivity

   - 问题

     - Fragment什么时候初始化和add到容器中？什么时候hide和show？
     - 如何让TextView被选中？选中一个TextView后，要做一些什么操作？
     - 刚进入MainActivity怎么样让一个TextView处于Selected的状态？

   - 解决

     1. 选中TextView后对对应的Fragment进行判空，如果为空，初始化，并添加到容器中； 而hide的话，定义一个方法hide所有的Fragment，每次触发点击事件就先调用这个hideAll方法，将所有Fragment隐藏起来，然后如果TextView对应的Fragment不为空，我们就将这个Fragment显示出来；
     2. 通过点击事件来实现，点击TextView后先重置所有TextView的选中状态为false，然后设置点击的 TextView的选中状态为true；
     3. 是通过点击事件来设置选中的，在onCreate()方法里加个触发点击事件的方法(模拟一次点击)  txt_channel.performClick();

   - ```Java
     public class MainActivity extends AppCompatActivity implements View.OnClickListener{
     
         //UI Object
         private TextView txt_topbar;
         private TextView txt_channel;
         private TextView txt_message;
         private TextView txt_better;
         private TextView txt_setting;
         private FrameLayout ly_content;
     
         //Fragment Object
         private MyFragment fg1,fg2,fg3,fg4;
         private FragmentManager fManager;
     
         @Override
         protected void onCreate(Bundle savedInstanceState) {
             super.onCreate(savedInstanceState);
             requestWindowFeature(Window.FEATURE_NO_TITLE);
             setContentView(R.layout.activity_main);
             fManager = getSupportFragmentManager();
             bindViews();
             txt_channel.performClick();   //模拟一次点击，既进去后选择第一项
         }
     
         //UI组件初始化与事件绑定
         private void bindViews() {
             txt_topbar = (TextView) findViewById(R.id.txt_topbar);
             txt_channel = (TextView) findViewById(R.id.txt_channel);
             txt_message = (TextView) findViewById(R.id.txt_message);
             txt_better = (TextView) findViewById(R.id.txt_better);
             txt_setting = (TextView) findViewById(R.id.txt_setting);
             ly_content = (FrameLayout) findViewById(R.id.ly_content);
     
             txt_channel.setOnClickListener(this);
             txt_message.setOnClickListener(this);
             txt_better.setOnClickListener(this);
             txt_setting.setOnClickListener(this);
         }
     
         //重置所有文本的选中状态
         private void setSelected(){
             txt_channel.setSelected(false);
             txt_message.setSelected(false);
             txt_better.setSelected(false);
             txt_setting.setSelected(false);
         }
     
         //隐藏所有Fragment
         private void hideAllFragment(FragmentTransaction fragmentTransaction){
             if(fg1 != null)fragmentTransaction.hide(fg1);
             if(fg2 != null)fragmentTransaction.hide(fg2);
             if(fg3 != null)fragmentTransaction.hide(fg3);
             if(fg4 != null)fragmentTransaction.hide(fg4);
         }
     
     
         @Override
         public void onClick(View v) {
             FragmentTransaction fTransaction = fManager.beginTransaction();
             hideAllFragment(fTransaction);
             switch (v.getId()){
                 case R.id.txt_channel:
                     setSelected();
                     txt_channel.setSelected(true);
                     if(fg1 == null){
                         fg1 = new MyFragment("第一个Fragment");
                         fTransaction.add(R.id.ly_content,fg1);
                     }else{
                         fTransaction.show(fg1);
                     }
                     break;
                 case R.id.txt_message:
                     setSelected();
                     txt_message.setSelected(true);
                     if(fg2 == null){
                         fg2 = new MyFragment("第二个Fragment");
                         fTransaction.add(R.id.ly_content,fg2);
                     }else{
                         fTransaction.show(fg2);
                     }
                     break;
                 case R.id.txt_better:
                     setSelected();
                     txt_better.setSelected(true);
                     if(fg3 == null){
                         fg3 = new MyFragment("第三个Fragment");
                         fTransaction.add(R.id.ly_content,fg3);
                     }else{
                         fTransaction.show(fg3);
                     }
                     break;
                 case R.id.txt_setting:
                     setSelected();
                     txt_setting.setSelected(true);
                     if(fg4 == null){
                         fg4 = new MyFragment("第四个Fragment");
                         fTransaction.add(R.id.ly_content,fg4);
                     }else{
                         fTransaction.show(fg4);
                     }
                     break;
             }
             fTransaction.commit();
         }
     }
     ```

   - 注意

     - FragmentTransaction只能使用一次，每次使用都要调用FragmentManager 的beginTransaction()方法获得FragmentTransaction事务对象

## 熟练Fragment-02(使用*RadioGroup + RadioButton来实现01*)

一个RadioGroup包着四个RadioButton，和前面一样用比例来划分:1:1:1:1；
只需重写RadioGroup的onCheckedChange，判断checkid即可知道点击的是哪个RadioButton	

实现流程

将drawable类的资源都是将selected 状态修改成checked

1. 底部选项的资源文件

   1. 图片

      - ```xml
        <?xml version="1.0" encoding="utf-8"?>
        <selector xmlns:android="http://schemas.android.com/apk/res/android">
            <item android:drawable="@mipmap/tab_channel_pressed" android:state_checked="true" />
            <item android:drawable="@mipmap/tab_channel_normal" />
        </selector>
        ```

      - 其他三个同样

   2. 文字

      - ```xml
        <?xml version="1.0" encoding="utf-8"?>
        <selector xmlns:android="http://schemas.android.com/apk/res/android">
            <item android:color="@color/text_yellow" android:state_checked="true" />
            <item android:color="@color/text_gray" />
        </selector>
        ```

   3. 背景

      - ```xml
        <?xml version="1.0" encoding="utf-8"?>
        <selector xmlns:android="http://schemas.android.com/apk/res/android">
            <item android:state_selected="true">
                <shape>
                    <solid android:color="#FFC4C4C4" />
                </shape>
            </item>
            <item>
                <shape>
                    <solid android:color="@color/transparent" />
                </shape>
            </item>
        </selector>
        ```

2. 编写Activity布局

   1. 取出一个RadioGroup标签

      - ```xml
        <RadioButton
                    android:id="@+id/rb_channel"
                    android:layout_width="0dp"
                    android:layout_height="match_parent"
                    android:layout_weight="1"
                    android:background="@drawable/tab_menu_bg"
                    android:button="@null"
                    android:drawableTop="@drawable/tab_menu_channel"
                    android:gravity="center"
                    android:paddingTop="3dp"
                    android:text="@string/tab_menu_alert"
                    android:textColor="@drawable/tab_menu_text"
                    android:textSize="18sp" />
        ```

   2. 把每个相同RadioButton都相同的属性抽取出来，写到styles.xml中

      - ```xml
        <style name="tab_menu_item">
                <item name="android:layout_width">0dp</item>
                <item name="android:layout_weight">1</item>
                <item name="android:layout_height">match_parent</item>
                <item name="android:background">@drawable/tab_menu_bg</item>
                <item name="android:button">@null</item>
                <item name="android:gravity">center</item>
                <item name="android:paddingTop">3dp</item>
                <item name="android:textColor">@drawable/tab_menu_text</item>
                <item name="android:textSize">18sp</item>
            </style>
        ```

      - 然后activity_main.xml中的RadioButton就不用每次都写相同的代码了， 只要在RadioButton中添上**style="@style/tab_menu_item"**就可以了

      - ```xml
        <?xml version="1.0" encoding="utf-8"?>
        <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
            xmlns:app="http://schemas.android.com/apk/res-auto"
            xmlns:tools="http://schemas.android.com/tools"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            tools:context=".MainActivity">
            <RelativeLayout
                android:id="@+id/ly_top_bar"
                android:layout_width="match_parent"
                android:layout_height="48dp"
                android:background="@color/bg_topbar">
        
                <TextView
                    android:id="@+id/txt_topbar"
                    android:layout_width="match_parent"
                    android:layout_height="match_parent"
                    android:layout_centerInParent="true"
                    android:gravity="center"
                    android:text="信息"
                    android:textColor="@color/text_topbar"
                    android:textSize="18sp" />
        
                <View
                    android:layout_width="match_parent"
                    android:layout_height="2px"
                    android:layout_alignParentBottom="true"
                    android:background="@color/div_white" />
        
            </RelativeLayout>
        
            <RadioGroup
                android:id="@+id/rg_tab_bar"
                android:layout_width="match_parent"
                android:layout_height="56dp"
                android:layout_alignParentBottom="true"
                android:background="@color/bg_white"
                android:orientation="horizontal">
        
                <RadioButton
                    android:id="@+id/rb_channel"
                    style="@style/tab_menu_item"
                    android:drawableTop="@drawable/tab_menu_channel"
                    android:text="@string/tab_menu_alert" />
        
                <RadioButton
                    android:id="@+id/rb_message"
                    style="@style/tab_menu_item"
                    android:drawableTop="@drawable/tab_menu_message"
                    android:text="@string/tab_menu_profile" />
        
                <RadioButton
                    android:id="@+id/rb_better"
                    style="@style/tab_menu_item"
                    android:drawableTop="@drawable/tab_menu_better"
                    android:text="@string/tab_menu_pay" />
        
                <RadioButton
                    android:id="@+id/rb_setting"
                    style="@style/tab_menu_item"
                    android:drawableTop="@drawable/tab_menu_setting"
                    android:text="@string/tab_menu_setting"/>
        
            </RadioGroup>
        
            <View
                android:id="@+id/div_tab_bar"
                android:layout_width="match_parent"
                android:layout_height="2px"
                android:layout_above="@id/rg_tab_bar"
                android:background="@color/div_white" />
        
            <FrameLayout
                android:id="@+id/ly_content"
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                android:layout_above="@id/div_tab_bar"
                android:layout_below="@id/ly_top_bar"></FrameLayout>
        </RelativeLayout>
        ```

3. 隐藏顶部导航栏

   - **AndroidManifest.xml设置下theme属性**

     ```xml
     android:theme="@style/Theme.AppCompat.NoActionBar"
     ```

4. 复制01中的Fragment的简单布局和类

5. 编写MainActivity

   - ```Java
     public class MainActivity extends AppCompatActivity implements RadioGroup.OnCheckedChangeListener{
     
         private RadioGroup rg_tab_bar;
         private RadioButton rb_channel;
     
         //Fragment Object
         private MyFragment fg1,fg2,fg3,fg4;
         private FragmentManager fManager;
     
         @Override
         protected void onCreate(Bundle savedInstanceState) {
             super.onCreate(savedInstanceState);
             setContentView(R.layout.activity_main);
             fManager = getSupportFragmentManager();
             rg_tab_bar = (RadioGroup) findViewById(R.id.rg_tab_bar);
             rg_tab_bar.setOnCheckedChangeListener(this);
             //获取第一个单选按钮，并设置其为选中状态
             rb_channel = (RadioButton) findViewById(R.id.rb_channel);
             rb_channel.setChecked(true);
         }
     
     
         @Override
         public void onCheckedChanged(RadioGroup group, int checkedId) {
             FragmentTransaction fTransaction = fManager.beginTransaction();
             hideAllFragment(fTransaction);
             switch (checkedId){
                 case R.id.rb_channel:
                     if(fg1 == null){
                         fg1 = new MyFragment("第一个Fragment");
                         fTransaction.add(R.id.ly_content,fg1);
                     }else{
                         fTransaction.show(fg1);
                     }
                     break;
                 case R.id.rb_message:
                     if(fg2 == null){
                         fg2 = new MyFragment("第二个Fragment");
                         fTransaction.add(R.id.ly_content,fg2);
                     }else{
                         fTransaction.show(fg2);
                     }
                     break;
                 case R.id.rb_better:
                     if(fg3 == null){
                         fg3 = new MyFragment("第三个Fragment");
                         fTransaction.add(R.id.ly_content,fg3);
                     }else{
                         fTransaction.show(fg3);
                     }
                     break;
                 case R.id.rb_setting:
                     if(fg4 == null){
                         fg4 = new MyFragment("第四个Fragment");
                         fTransaction.add(R.id.ly_content,fg4);
                     }else{
                         fTransaction.show(fg4);
                     }
                     break;
             }
             fTransaction.commit();
         }
     
         //隐藏所有Fragment
         private void hideAllFragment(FragmentTransaction fragmentTransaction){
             if(fg1 != null)fragmentTransaction.hide(fg1);
             if(fg2 != null)fragmentTransaction.hide(fg2);
             if(fg3 != null)fragmentTransaction.hide(fg3);
             if(fg4 != null)fragmentTransaction.hide(fg4);
         }
     
     }
     ```
