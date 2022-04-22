# 5 探究Fragment

## 5.1 Fragment是什么

*Fragment是Android3.0后引入的一个新的API，他出现的初衷是为了适应大屏幕的平板电脑， 当然现在他仍然是平板APP UI设计的宠儿，而且我们普通手机开发也会加入这个Fragment， 我们可以把他看成一个小型的Activity，又称Activity片段！如果一个很大的界面，就一个布局，写起界面会麻烦，而且如果组件多的话是管理起来也很麻烦！而使用Fragment 我们可以把屏幕划分成几块，然后进行分组，进行一个模块化的管理！从而可以更加方便的在 运行过程中动态地更新Activity的用户界面！另外Fragment并不能单独使用，他需要嵌套在Activity 中使用，尽管他拥有自己的生命周期，但是还是会受到宿主Activity的生命周期的影响，比如Activity 被destory销毁了，他也会跟着销毁！*[菜鸟教程](https://www.runoob.com/w3cnote/android-tutorial-fragment-base.html)

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

   - 

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
     5. 提交事物，调用commit()方法实现

### 5.2.3 在Fragment中实现返回栈

通过按下back返回到上一个Fragment

修改MainActivity中的代码

![image-20220421110849736](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220421110849736.png)

在提交事务之前，调用了FragmentTransaction的addToBackStack()方法，它可以接收一个名字用于描述返回栈的状态，一般传入null即可。

### 5.2.4 Fragment和Activity之间进行交互

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

## 5.4 动态加载布局的技巧

## 5.5 Fragment的最佳实践