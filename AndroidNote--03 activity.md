# 第一章
## 1.1 简介
操作系统
### 1.1.1 Android系统架构
Android大致可以分为4层架构:Linux内核层、系统运行库层、应用框架层和应用层。

01. Linux内核层
Android系统是基于Linux内核的,这一层为Android设备的各种硬件提供了底层的驱动,
如显示驱动、音频驱动、照相机驱动、蓝牙驱动、Wi-Fi驱动、电源管理等。
02. 系统运行库层
这一层通过一些C/C++库为Android系统提供了主要的特性支持。
03. 应用框架层
这一层主要提供了构建应用程序时可能用到的各种API,Android自带的一些核心应用就是
使用这些API完成的。
04. 应用层
所有安装在手机上的应用程序都是属于这一层的,比如系统自带的联系人、短信等程序,
或者是你从Google Play上下载的小游戏,当然还包括你自己开发的程序。

<img src="https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220407094625300.png" alt="image-20220407094625300" style="zoom:67%;" />

### 1.1.2  Android已发布的版本

### 1.1.3  Android应用开发特色

1. 四大组件
   Android系统四大组件分别是Activity、Service、BroadcastReceiver和ContentProvider。

   - Activity是所有Android应用程序的门面,凡是在应用中看得到的东西,都是放在Activity中的。
   - Service在后台默默地运行,即使用户退出了应用,Service仍然是可以继续运行的。
   - BroadcastReceiver允许你的应用接收来自各处的广播消息,比如电话、短信等,你的应用也可以向外发出广播消息。
   - ContentProvider则为**应用程序之间共享数据**提供了可能,比如你想要读取系统通讯录中的联系人,就需要通过ContentProvider来实现。
2. 丰富的系统控件
   Android系统为开发者提供了丰富的系统控件,可以很轻松地编写出漂亮的界面。
   可以定制属于自己的控件。
3. SQLite数据库
   Android系统还自带了这种轻量级、运算速度极快的嵌入式关系型数据库。它不仅支持标准的SQL语法,还可以通过Android封装好的API进行操作,让存储和读取数据变得非常方便。
4. 强大的多媒体

   Android系统还提供了丰富的多媒体服务,如音乐、视频、录音、拍照等,这都可以在程序中通过代码进行控制。

## 1.2  搭建开发环境

### 1.2.1  需要的工具

- JDK
- Android SDK
- Android Studio

### 1.2.2  开发环境

https://developer.android.google.cn/studio 

http://www.android-studio.org/

## 1.3 第一个android项目

### 1.3.1 hello world项目

### 1.3.2 启动模拟器 运行helloworld

创建模拟器  并将helloworld项目运行到模拟器上

### 1.3.3 分析第一个android程序

任何一个新建的项目都会默认使用Android模式的项目结构,但这并不是项目真实的目录结构,
而是被Android Studio转换过的。



**Project模式的项目结构**

<img src="https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220407094821316.png" alt="image-20220407094821316" style="zoom:67%;" />

1. .gradle和.idea
   这两个目录下放置的都是Android Studio自动生成的一些文件,无须关心,也不要去手动编辑。
2. app
   **项目中的代码、资源等内容都是放置在这个目录下的,我们后面的开发工作也基本是在这个目录下进行的,待会儿还会对这个目录单独展开讲解。**
3. build
   这个目录主要包含了一些在编译时自动生成的文件,不需要过多关心。
4. gradle
   这个目录下包含了gradle wrapper的配置文件,使用gradle wrapper的方式不需要提前将gradle下载好,而是会自动根据本地的缓存情况决定是否需要联网下载gradle。
   Android Studio默认就是启用gradle wrapper方式的,如果需要更改成离线模式,可以点击Android Studio导航栏→File→Settings→Build, Execution,Deployment→Gradle,进行配置更改。
5. .gitignore
   这个文件是用来将指定的目录或文件排除在版本控制之外的。
6. build.gradle
   这是项目全局的gradle构建脚本,通常这个文件中的内容是不需要修改的。
   后面详细分析gradle构建脚本中的具体内容。
7. gradle.properties
   这个文件是全局的gradle配置文件,在这里配置的属性将会影响到项目中所有的gradle编译脚本。
8. gradlew和gradlew.bat
   这两个文件是用来在命令行界面中执行gradle命令的,其中gradlew是在Linux或Mac系统中使用的,gradlew.bat是在Windows系统中使用的。
9. HelloWorld.iml
   iml文件是所有IntelliJ IDEA项目都会自动生成的一个文件(Android Studio是基于IntelliJIDEA开发的),用于标识这是一个IntelliJ IDEA项目,我们不需要修改这个文件中的任何内容。
10. local.properties
    这个文件用于指定本机中的Android SDK路径,通常内容是自动生成的,我们并不需要修改。除非你本机中的Android SDK位置发生了变化,那么就将这个文件中的路径改成新的位置即可。
11. settings.gradle
    这个文件用于指定项目中所有引入的模块。由于HelloWorld项目中只有一个app模块,因此该文件中也就只引入了app这一个模块。通常情况下,模块的引入是自动完成的,需要我们手动修改这个文件的场景可能比较少

#### app目录结构

<img src="https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220407094900195.png" alt="image-20220407094900195" style="zoom:67%;" />

01. build
这个目录和外层的build目录类似,也包含了一些在编译时自动生成的文件,不过它里面的内容会更加复杂,我们不需要过多关心。
02. **libs**
如果你的项目中使用到了第三方jar包,**就需要把这些jar包都放在libs目录下**,放在这个目录下的jar包会被自动添加到项目的构建路径里。
03. androidTest
此处是用来编写Android Test测试用例的,可以对项目进行一些自动化测试。
04. **java**
**java目录是放置我们所有Java代码的地方**,展开该目录,系统帮我们自动生成了一个MainActivity文件。
05. res
这个目录下的内容,就是**在项目中使用到的所有图片、布局、字符串等资源都要存放在这个目录下**。这个目录下还有很多子目录,图片放在drawable目录下,布局放在layout目录下,字符串放在values目录下。
06. **AndroidManifest.xml**
这是整个Android项目的配置文件,你在程序中定义的所有四大组件都需要在这个文件里注册,另外还可以在这个文件中给应用程序添加权限声明。

07. test
此处是用来编写Unit Test测试用例的,是对项目进行自动化测试的另一种方式。
08. .gitignore
这个文件用于将app模块内指定的目录或文件排除在版本控制之外,作用和外层
的.gitignore文件类似。
09. app.iml
IntelliJ IDEA项目自动生成的文件,我们不需要关心或修改这个文件中的内容。
10. build.gradle
这是app模块的gradle构建脚本,这个文件中会指定很多项目构建相关的配置,我们稍后
将会详细分析gradle构建脚本中的具体内容。
11. proguard-rules.pro
这个文件用于指定项目代码的混淆规则,当代码开发完成后打包成安装包文件,如果不希望代码被别人破解,通常会将代码进行混淆,从而让破解者难以阅读。

```xml
<!--这段代码表示对MainActivity进行注册,没有在AndroidManifest.xml里注册的Activity-->
        <activity
            android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <!--表示MainActivity是这个项目的主Activity,在手机上点击应用图标,首先启动的就是这个Activity-->
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
```

```java
/*
    AppCompatActivity是AndroidX中提供的一种向下兼容的Activity,可以使Activity在不同系统版本中的功能保持一致性
    Activity类是Android系统提供的一个基类  项目中所有自定义的Activity都必须继承它或者它的子类才能拥有Activity的特性
    onCreate()这个方法是一个Activity被创建时必定要执行的方法
*/
public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        //这个方法给当前的Activity引入了一个activity_main布局,“Hello World!”就是在这里定义的
        //布局文件都是定义在res/layout目录下的
        setContentView(R.layout.activity_main);
    }
}
```

```xml
<!--通过android:text="Hello World!"这句代码定义的-->
<TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello World!"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toTopOf="parent" />
```

### 1.3.4 详解项目中的资源

<img src="https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220408102600570.png" alt="image-20220408102600570" style="zoom:67%;" />

所有以“drawable”开头的目录都是用来放图片的

所有以“layout”开头的目录都是用来放布局文件的。

所有以“mipmap”开头的目录都是用来放应用图标的

所有以“values”开头的目录都是用来放字符串、样式、颜色等配置的

```
有这么多“mipmap”开头的目录,主要是为了让程序能够更好地兼容各种设备。drawable目录也是相同的道理,虽然Android Studio没有帮我们自动生成,但是我们应该自己创建drawable-hdpi、drawable-xhdpi、drawable-xxhdpi等目录。在制作程序的时候,最好能够给同一张图片提供几个不同分辨率的版本,分别放在这些目录下,然后程序运行的时候,会自动根据当前运行设备分辨率的高低选择加载哪个目录下的图片。当然这只是理想情况,更多的时候美工只会提供给我们一份图片,这时你把所有图片都放在drawable-xxhdpi目录下就好了,因为这是最主流的设备分辨率目录。
```

res/values/strings.xml文件,内容

![](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220408104524715.png)

这里定义了一个应用程序名的字符串,可以用下面两种方式来引用它。

- 在代码中通过R.string.app_name可以获得该字符串的引用。
  - ![image-20220408145443259](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220408145443259.png)
- 在XML中通过@string/app_name可以获得该字符串的引用。
  - ![image-20220408145909206](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220408145909206.png)label 处字样为项目名称

基本的语法就是两种方式,其中string部分是可以替换的,引用图片资源就替换成R.drawable

引用应用图标可以替换成mipmap,引用布局文件就可以替换成layout,以此类推。

<img src="https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220408155354552.png" alt="image-20220408155354552" style="zoom: 67%;" />

应用图标就是通过android:icon属性指定的,应用的名称则是通过android:label属性指定的。

这里对资源引用的方式正是在XML中引用资源的语法。

### 1.3.5 详解build.gradle文件（没明白）
Android Studio是采用Gradle来构建项目的。Gradle是一个非常先进的项目构建工具,它使用了一种基于Groovy的领域特定语言(DSL)来进行项目设置,摒弃了传统基于XML(如Ant和Maven)的各种烦琐配置。
在1.3.3HelloWorld项目中有两个build.gradle文件,一个是在最外层目录下的,一个是在app目录下的。这两个文件对构建Android Studio项目都起到了重要的作用,下面是对这两个文件中的内容分析。

## 1.4 日志工具的使用

### 1.4.1 使用Android的日志工具Log
Android中的日志工具类是Log(android.util.Log),这个类中提供了如下5个方法来供我们打印日志。

- Log.v()。用于打印那些最为琐碎的、意义最小的日志信息。对应级别verbose,是Android日志里面级别最低的一种。
- Log.d()。用于打印一些调试信息,这些信息对你调试程序和分析问题应该是有帮助的。对应级别debug,比verbose高一级。
- Log.i()。用于打印一些比较重要的数据,这些数据应该是你非常想看到的、可以帮你分析用户行为的数据。对应级别info,比debug高一级。
- Log.w()。用于打印一些警告信息,提示程序在这个地方可能会有潜在的风险,最好去修复一下这些出现警告的地方。对应级别warn,比info高一级。
- Log.e()。用于打印程序中的错误信息,比如程序进入了catch语句中。当有错误信息打印出来的时候,一般代表你的程序出现严重问题了,必须尽快修复。对应级别error,比warn高一级。

一共就5个方法,每个方法还会有不同的重载。

![image-20220408165735810](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220408165735810.png)

Log.d()方法中传入了两个参数:第一个参数是tag,一般传入当前的类名就好,主要用于对打印信息进行过滤;第二个参数是msg,即想要打印的具体内容。
重新运行项目。等程序运行完毕,在Logcat中就可以看到打印信息了。

![image-20220408165944571](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220408165944571.png)

### 1.4.2 为什么使用Log而不使用println()

首先,Logcat中可以很轻松地添加过滤器,可以在图中看到我们目前所有的过滤器。

![image-20220408171004830](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220408171004830.png)

目前只有3个过滤器,Show only selected application表示只显示当前选中程序的日志;
Firebase是Google提供的一个开发者工具和基础架构平台,可以不用管它;

No Filters相当于没有过滤器,会把所有的日志都显示出来。

添加一个自定义过滤器

点击“Edit Filter Configuration”,会弹出一个过滤器配置界面。我们给过滤器起
名叫data,并且让它对名为data的tag进行过滤

<img src="https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220408171309092.png" alt="image-20220408171309092" style="zoom: 67%;" />

点击“OK”,多出一个data过滤器。当选中这个过滤器的时候,刚才在onCreate()方法里打印的日志就不见了,这是因为data这个过滤器只会显示tag名称为data的日志。在onCreate()方法中把打印日志的语句改成Log.d("data","==========onCreate execute============"),然后再次运行程序,就会在data过滤器下看到这行日志了。

<img src="https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220408171807542.png" alt="image-20220408171807542" style="zoom:67%;" />

Logcat中的日志级别控制。Logcat中主要有5个级别,分别对应上一小节介绍的5个方法。

![image-20220408171847493](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220408171847493.png)

当选中的级别是Verbose,也就是最低等级。这意味着不管使用哪一个方法打印日志,这条日志都一定会显示出来。而将级别选中为Debug,这时只有我们使用Debug及以上级别方法打印的日志才会显示出来,以此类推。

当把Logcat中的级别选中为Info、Warn或者Error时,在onCreate()方法中打印的语句不会显示,因为打印日志时使用的是Log.d()方法。

关键字过滤

![image-20220411093754667](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220411093754667.png)

# 3 Activity

## 3.1 Activity是什么

Activity：在Android中，Activity代表手机或者平板电脑中的一屏，提供和用户交互的可视化界面。

Activity是一种可以包含用户界面的组件（在一个Activity中可以添加很多组件，这些组件负责具体的功能）,主要用于和用户进行交互。一个应用程序中可以包含零个或多个Activity。

## 3.2 Activity的基本用法

### 3.2.1 手动创建Activity

1. 手动创建Activity
   - new 一个新的project FirstActivity  选择No Activity
2. 在app/src/main/java/com.example.activitytest目录下
   - New→Activity→Empty Activity
3. 勾选Generate Layout File表示会自动为FirstActivity创建一个对应的布局文件,
   勾选Launcher Activity表示会自动将FirstActivity设置为当前项目的主Activity。
   - ![image-20220411094745773](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220411094745773.png)

项目中的任何Activity都应该重写onCreate()方法

### 3.2.2 创建和加载布局

1. 创建和加载布局

   - 在app/src/main/res目录→New→Directory,先创建一个名为layout的目录。然后对着layout目录右键→New→Layout resource file,然后在新建布局资源文件的窗口,将这个布局文件命名为first_layout,根元素默认选择为LinearLayout

     - ![image-20220411100356845](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220411100356845.png)

   - 创建之后的布局编辑器

     - XML文件的方式   同时存在    可视化布局编辑器![image-20220411101012661](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220411101012661.png)

     - ![image-20220411100625860](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220411100625860.png)

     - ```xml
       <?xml version="1.0" encoding="utf-8"?>
       <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
           android:orientation="vertical"
           android:layout_width="match_parent"
           android:layout_height="match_parent">
           <!--添加了一个Button元素,并在Button元素的内部增加了几个属性。-->
           <Button
               android:id="@+id/button1"
               android:layout_width="match_parent"
               android:layout_height="match_parent"
               android:text="Button 1"
           />
       </LinearLayout>
       ```

     - android:id是给当前的元素定义一个唯一的标识符,之后可以在代码中对这个元素进行操作。

       - 如果你需要在XML中引用一个id,就使用@id/id_name这种语法
       - 如果你需要在XML中定义一个id,则要使用@+id/id_name这种语法。

     - 随后android:layout_width指定了当前元素的宽度,这里match_parent表示让当前元素和父元素一样宽。

     - android:layout_height指定了当前元素的高度,这里wrap_content表示当前元素的高度只要能刚好包含里面的内容就行。

     - android:text指定了元素中显示的文字内容。

2. 预览效果

   - ![image-20220411102225012](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220411102225012.png)

3. 回到FirstActivity,在onCreate()方法中加入如下代码

   - ```java
     @Override
         protected void onCreate(Bundle savedInstanceState) {
             super.onCreate(savedInstanceState);
             /*调用了setContentView()方法来给当前的Activity加载一个布局,而在setContentView()方法中,我们一般会传入一个布局文件的id。项目中添加的任何资源都会在R文件中生成一个相应的资源id,因此我们刚才创建的first_layout.xml布局的id现在已经添加到R文件中了。在代码中引用布局文件的方法,只需要调用R.layout.first_layout就可以得到first_layout.xml布局的id,然后将这个值传入setContentView()方法即可。*/
             setContentView(R.layout.first_layout);
         }
     
     ```

### 3.2.3 在AndroidManifest文件中注册

所有的Activity都要在AndroidManifest.xml中进行注册才能生效。实际上FirstActivity已经在AndroidManifest.xml中注册过了,打开app/src/main/AndroidManifest.xml文件

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.nyh.activitytest">
    <application
        ...
        <activity
            android:name=".FirstActivity"
            android:label="This is FirstActivity"
            android:exported="true" >
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>
</manifest>
```

Activity的注册声明要放在<application>标签内,这里是通过<activity>标签来对Activity进行注册的。Android Studio在当创建Activity或其他系统组件时，会自动在AndroidManifest.xml中进行注册。
在<activity>标签中,android:name来指定具体注册哪一个Activity,这里填入的.FirstActivity是com.example.activitytest.FirstActivity的缩写。由于在最外层的<manifest>标签中已经通过package属性指定了程序的包名是com.example.activitytest,因此在注册Activity时,这一部分可以直接使用.FirstActivity。
还需要为程序配置主Activity。也就是说,程序运行起来的时候,首先启动的Activity。配置主Activity的方法就是在<activity>标签的内部加入<intent-filter>标签,并在标签里添加

```xml
<action android:name="android.intent.action.MAIN"/>
<category android:name="android.intent.category.LAUNCHER" />
```

这两句声明即可。

使用android:label指定Activity中标题栏的内容,标题栏是显示在Activity最顶部的。需要注意的是,给主Activity指定的label不仅会成为标题栏中的内容,还会成为启动器(Launcher)中应用程序显示的名称。

在界面的最顶部标题栏,里面显示的是刚才在注册Activity时指定的内容。标题栏的下面就是在布局文件first_layout.xml中编写的界面,可以看到我们刚刚定义的按钮。

![image-20220411110328739](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220411110328739.png)

### 3.2.4 在Activity中使用Toast

Toast是Android系统提供的一种非常好的提醒方式,在程序中可以使用它将一些短小的信息通
知给用户,这些信息会在一段时间后自动消失,并且不会占用任何屏幕空间。
首先需要定义一个弹出Toast的触发点,正好界面上有个按钮,那我们就让这个按钮的点击事件
作为弹出Toast的触发点吧。在onCreate()方法中添加如下代码:

```java
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.first_layout);
    // -------------------------------------------
    Button button1 = findViewById(R.id.button1);
    button1.setOnClickListener(new View.OnClickListener() {
        @Override
        public void onClick(View view) {
            Toast.makeText(FirstActivity.this,"click button1",Toast.LENGTH_SHORT).show();
        }
    });
}
```

在Activity中,可以通过findViewById()方法获取在布局文件中定义的元素,传入R.id.button1来得到按钮的实例,这个值是刚才在first_layout.xml中通过android:id属性指定的。
findViewById()方法返回的是一个继承自View的泛型对象,因此需要向下转型转为Button类
型。得到按钮的实例之后,我们通过调用setOnClickListener()方法为按钮注册一个监听器,点击按钮时就会执行监听器中的onClick()方法。因此,弹出Toast的功能要在onClick()方法中编写了。
Toast的用法:通过静态方法makeText()创建出一个Toast对象,然后调用show()将Toast显示出来就可以了。这里需要注意的是,makeText()方法需要传入3个参数。
第一个参数是Context,也就是Toast要求的上下文,由于Activity本身就是一个Context对象,因此这里直接传入this即可。
第二个参数是Toast显示的文本内容。
第三个参数是Toast显示的时长,有两个内置常量可以选择:Toast.LENGTH_SHORT和Toast.LENGTH_LONG。

重新运行程序，点击按钮可以看到![image-20220411112201199](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220411112201199.png)

### 3.2.5 在Activity中使用Menu

首先在res目录下新建一个menu文件夹,右击res目录→New→Directory,输入文件夹
名“menu”,点击“OK”。接着在这个文件夹下新建一个名叫“main”的菜单文件,右击menu文件夹→New→Menu resource file。

在main.xml中添加如下代码
```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item
        android:id="@+id/add_item"
        android:title="Add" />
    <item
        android:id="@+id/remove_item"
        android:title="Remove" />
</menu>
```
这里我们创建了两个菜单项,其中<item>标签用来创建具体的某一个菜单项,然后通过android:id给这个菜单项指定一个唯一的标识符,通过android:title给这个菜单项指定一个名称。
接着回到FirstActivity中来重写onCreateOptionsMenu()方法,重写方法可以使用Ctrl + O快捷键

```java
	@Override
    public boolean onCreateOptionsMenu(Menu menu) {
        getMenuInflater().inflate(R.menu.main,menu);
        return true;
    }
```

通过getMenuInflater()方法得到MenuInflater对象，调用inflate()方法给当前活动创建菜单。inflate()方法接收两个参数：

第一个参数用于指定我们通过哪个资源文件来创建菜单，传入R.menu.main。

第二个参数用于指定我们的菜单项将添加到哪一个Menu对象当中，这里直接使用onCreateOptionsMenu()方法中传入的menu参数。

然后给这个方法返回true,表示允许创建的菜单显示出来，如果返回了false,创建的菜单将无法显示。

在FirstActivity中重写onOptionsItemSelected()方法：让菜单真正可用

```java
@Override
public boolean onOptionsItemSelected(@NonNull MenuItem item) {
    switch (item.getItemId()) {
        case R.id.add_item:
            Toast.makeText(FirstActivity.this,"click add",Toast.LENGTH_SHORT).show();
            break;
        case R.id.remove_item:
            Toast.makeText(FirstActivity.this,"click remove",Toast.LENGTH_SHORT).show();
            break;
        default:
    }
    return true;
}
```

在onOptionsItemSelected()方法中，通过调用item,getItemId()来判断我们点击的是哪一个菜单项，然后给每个菜单项加入自己的逻辑处理，这里弹出一个刚刚使用过的Toast。
重新运行程序，标题栏的右侧多了一个三点的符号，这个就是菜单按钮，如图

![image-20220411140202788](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220411140202788.png)

点击Add  Remove触发不同的Toast

### 3.2.6 销毁一个Activity

其实答案非常简单,只要按一下Back键就可以销毁当前的Activity了。不过,如果你不想通过按键的方式,而是希望在程序中通过代码来销毁Activity,当然也可以,Activity类提供了一个finish()方法,我们只需要调用一下这个方法就可以销毁当前的Activity了。
修改按钮监听器中的代码,如下所示:

```java
button1.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View view) {
        //Toast.makeText(FirstActivity.this,"click button1",Toast.LENGTH_SHORT).show();
        //效果等同于back
        finish();
    }
});
```

重新运行程序,这时点击一下按钮,当前的Activity就被成功销毁了,效果和按下Back键是一样的。

## 3.3 使用Intent在活动之间穿梭

不管创建多少个Activity,方法都和上一节中介绍的是一样的。唯一的问题在于,在启动器中点击应用的图标只会进入该应用的主Activity。**由主Activity跳转到别的Activity**

### 3.3.1 使用显式 Intent
再快速地创建一个Activity。右击com.example.activitytest→New→Activity→Empty Activity,命名为SecondActivity,并勾选Generate Layout File,给布局文件起名为second_layout,但不要勾选Launcher Activity选项

点击“Finish”完成创建,Android Studio会为我们自动生成SecondActivity.java和
second_layout.xml这两个文件。
这里我们还是使用比较熟悉的LinearLayout,编辑second_layout.xml,将里面的代码替换成如下内容:

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <Button
        android:id="@+id/button2"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Button 2"
    />
</LinearLayout>
```

SecondActivity中的代码已经自动生成了一部分

```java
public class SecondActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.second_layout);
    }
}
```

自动已经注册好了

```xml
<activity
      android:name=".SecondActivity"
      android:exported="false" />
```

由于SecondActivity不是主Activity,因此不需要配置<intent-filter>标签里的内容,。

**Intent是Android程序中各组件之间进行交互的一种重要方式**,它不仅可以指明当前组件想要执行的动作,还可以在不同组件之间传递数据。Intent一般可用于**启动Activity（本次做的）**、启动Service以及发送广播等场景。
Intent大致可以分为两种:显式Intent和隐式Intent。我们先来看一下显式Intent如何使用。

Intent有多个构造函数的重载,其中一个是Intent(Context packageContext, Class<?
cls)。这个构造函数接收两个参数:

第一个参数Context要求提供一个启动Activity的上下文;

第二个参数Class用于指定想要启动的目标Activity

通过这个构造函数创建出Intent。接下来使用这个Intent，Activity类中提供了一个startActivity()方法,专门用于启动Activity,它接收一个Intent参数,这里将构建好的Intent传入startActivity()方法就可以启动目标Activity了。

```java
@Override
            public void onClick(View view) {
                Intent intent = new Intent(FirstActivity.this, SecondActivity.class);
                startActivity(intent);
            }
```

`首先new了一个Intent，传入FirstActivity.this作为上下文，传入SecondActivity.class作为目标Activity，这样做的目的是在FirstActivity这个活动的基础上打开SecondActivity。然后通过startActivity（）方法执行这个Intent`

按下Back就可以销毁当前活动，返回到上一个活动。这种方式称之为显式Intent。

### 3.3.2 使用隐式Intent

相比于显式Intent,隐式Intent则含蓄了许多,它并不明确指出想要启动哪一个Activity,而是
指定了一系列更为抽象的action和category等信息,然后交由系统去分析这个Intent,并帮
我们找出合适的Activity去启动。
什么叫作合适的Activity?简单来说就是可以响应这个隐式Intent的Activity,目前
SecondActivity可以响应什么样的隐式Intent，通过在<activity>标签下配置<intent-filter>的内容,可以指定当前Activity能够响应的action和category,打开AndroidManifest.xml,添加如下代码:

```xml
<activity
    android:name=".SecondActivity"
    android:exported="false" >
    <intent-filter>
        <action android:name="com.nyh.activitytest.ACTION_START" />
        <category android:name="android.intent.category.DEFAULT" />
    </intent-filter>
</activity>
```

在<action>标签中我们指明了当前Activity可以响应
com.nyh.activitytest.ACTION_START这个action,而<category>标签则包含了
一些附加信息,更精确地指明了当前Activity能够响应的Intent中还可能带有的category。**只
有<action>和<category>中的内容同时匹配Intent中指定的action和category时,这个
Activity才能响应该Intent。**

修改FirstActivity中按钮的点击事件

```java
button1.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Intent intent = new Intent("com.nyh.activitytest.ACTION_START");
                startActivity(intent);
            }
        });
```

使用了Intent的另一个构造函数,直接将action的字符串传了进去,表明想要启动能够响应com.example.activitytest.ACTION_START这个action的Activity。
这里因为android.intent.category.DEFAULT是一种默认的category,在调用startActivity()方法的时候会自动将这个category添加到Intent中。重新运行程序,在FirstActivity的界面点击一下按钮,你同样成功启动SecondActivity了。不同的是这次使用隐式Intent的方式来启动的,说明在<activity>标签下配置的action和category的内容已经生效了!每个Intent中只能指定一个action,但能指定多个category。再来增加一个。
修改FirstActivity中按钮的点击事件,代码如下:

```java
button1.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Intent intent = new Intent("com.nyh.activitytest.ACTION_START");
                //可以调用Intent中的addCategory()方法来添加一个category
                //定了一个自定义的category,值为com.nyh.activitytest.MY_CATEGORY。
                intent.addCategory("com.nyh.activitytest.MY_CATEGORY");
                startActivity(intent);
            }
        });
```

现在重新运行程序,在FirstActivity的界面点击一下按钮,报错了。在Logcat界面查看错误日志,看到如图所示错误信息。

![image-20220411144715232](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220411144715232.png)

错误信息表明没有任何一个Activity可以响应定义的Intent。这是因为Intent中新增了一个category,而SecondActivity的<intent-filter>标签中并没有声明可以响应这个category,所以就出现了没有任何Activity可以响应该Intent的情况。现在在<intent-filter>中再添加一个category的声明,如下所示:

```xml
<activity android:name=".SecondActivity" >
	<intent-filter>
		<action android:name="com.example.activitytest.ACTION_START" />
		<category android:name="android.intent.category.DEFAULT" />
        <!--添加一个category响应定义Intent-->
		<category android:name="com.example.activitytest.MY_CATEGORY"/>
	</intent-filter>
</activity>
```

重新运行程序,一切正常。

### 3.3.3 更多隐式Intent的用法

使用隐式Intent,不仅可以启动自己程序内的Activity,还可以启动其他程序的Activity,这就使多个应用程序之间的功能共享成为了可能。比如应用程序中需要展示一个网页,这时只需要调用系统的浏览器来打开这个网页就行了。

修改FirstActivity中button1的onClick方法

```java
Intent intent = new Intent(Intent.ACTION_VIEW);
intent.setData(Uri.parse("https://www.baidu.com"));
startActivity(intent);
```

首先指定了Intent的action是Intent.ACTION_VIEW,这是一个Android系统内置的动作,其常量值为android.intent.action.VIEW。

然后通过Uri.parse()方法将一个网址字符串解析成一个Uri对象,再调用Intent的setData()方法将这个Uri对象传递进去。

重新运行程序在FirstActivity界面点击按钮就可以看到打开了系统浏览器。

setData()这个方法接收一个Uri对象,主要用于指定当前Intent正在操作的数据,而这些数据通常是以字符串形式传入Uri.parse()方法中解析产生的。
与此对应,我们还可以在<intent-filter>标签中再配置一个<data>标签,用于更精确地指定当前Activity能够响应的数据。<data>标签中主要可以配置以下内容。

- android:scheme。用于指定数据的协议部分,如上例中的https部分。
- android:host。用于指定数据的主机名部分,如上例中的www.baidu.com部分。
- android:port。用于指定数据的端口部分,一般紧随在主机名之后。
- android:path。用于指定主机名和端口之后的部分,如一段网址中跟在域名之后的内容。
- android:mimeType。用于指定可以处理的数据类型,允许使用通配符的方式进行指定。

只有当<data>标签中指定的内容和Intent中携带的Data完全一致时,当前Activity才能够响应该Intent。不过,在<data>标签中一般不会指定过多的内容。例如在上面的浏览器示例中,其实只需要指定android:scheme为https,就可以响应所有https协议的Intent了。

通过自己建立一个Activity,让它也能响应打开网页的Intent

右击com.nyh.activitytest包→New→Activity→Empty Activity,新建ThirdActivity,并勾选Generate Layout File,给布局文件起名为third_layout,点击“Finish”完成创建。然后编辑third_layout.xml,将里面的代码替换成如下内容:

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
	android:orientation="vertical"
	android:layout_width="match_parent"
	android:layout_height="match_parent">
	<Button
		android:id="@+id/button3"
		android:layout_width="match_parent"
		android:layout_height="wrap_content"
		android:text="Button 3"
	/>
</LinearLayout>
```

ThirdActivity中的代码保持不变即可,最后在AndroidManifest.xml中修改ThirdActivity的注册信息:

```xml
<activity
      android:name=".ThirdActivity"
      android:exported="true">
      <intent-filter tools:ignore="AppLinkUrlError">
           <action android:name="android.intent.action.VIEW" />
           <category android:name="android.intent.category.DEFAULT" />
           <data android:scheme="https" />
      </intent-filter>
</activity>
```

ThirdActivity的<intent-filter>中配置了当前Activity能够响应的action是Intent.ACTION_VIEW的常量值,而category指定了默认的category值,另外在<data>标签中,我们通过android:scheme指定了数据的协议必须是https协议,这样ThirdActivity应该就和浏览器一样,能够响应一个打开网页的Intent了。

``另外,由于Android Studio认为所有能够响应ACTION_VIEW的Activity都应该加上BROWSABLE的category,否则就会给出一段警告提醒。加上BROWSABLE的category是为了实现deep link功能,和我们目前学习的东西无关,所以这里直接在<intent-filter>标签上使用tools:ignore属性将警告忽略即可。``

<img src="https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220411154118405.png" alt="image-20220411154118405" style="zoom:50%;" />

系统自动弹出了一个列表,显示了目前能够响应这个Intent的所有程序。选择Chrome还会像之前一样打开浏览器,并显示百度的主页,而如果选择了ActivityTest,则会启动ThirdActivity。JUST ONCE表示只是这次使用选择的程序打开,ALWAYS则表示以后一直使用这次选择的程序打开。``需要注意的是,虽然我们声明了ThirdActivity是可以响应打开网页的Intent的,但实际上这个Activity并没有加载并显示网页的功能,所以在真正的项目中尽量不要出现这种有可能误导用户的行为,不然会让用户对我们的应用产生负面的印象。``

除了https协议外,我们还可以指定很多其他协议,比如geo表示显示地理位置、tel表示拨打电话。下面的代码展示了如何在程序中调用系统拨号界面。

```java
Intent intent = new Intent(Intent.ACTION_DIAL);
intent.setData(Uri.parse("tel:10086"));
startActivity(intent);
```

### 3.3.4 向下一个Activity传递数据

Intent在启动Activity的时候还可以传递数据

在FirstActivity中传递一个字符串到SecondActivity中去

```java
button1.setOnClickListener(new View.OnClickListener() {
        @Override
        public void onClick(View view) {
              String data = "Hello SecondActivity";
              //显式Intent方式启动SecondActivity
              Intent intent = new Intent(FirstActivity.this,SecondActivity.class);
              //通过putExtra()方法传递参数 第一个参数为key  第二个为要传递的数据
              intent.putExtra("extra_data",data);
              startActivity(intent);
        }
});
```

```java
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.second_layout);
    Intent intent = getIntent();
    String data = intent.getStringExtra("extra_data");
    Log.d("SecondActivity",data);
}
```

通过getIntent()方法,获取用于启动SecondActivity的Intent,然后调用intent的getStringExtra()方法并传入相应的key,就可以得到传递的数据了。这里由于传递的是字符串,所以使用getStringExtra()方法来获取传递的数据。如果传递的是整型数据,则使用getIntExtra()方法;如果传递的是布尔型数据,则使用getBooleanExtra()方法,以此类推。
重新运行程序,在FirstActivity的界面点击一下按钮会跳转到SecondActivity,查看Logcat打印信息。

![image-20220411160227847](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220411160227847.png)

### 3.3.5 返回数据给上一个Activity

返回上一个活动只需要按一下Back键并没有一个用于启动活动Intent来传递数据。

Activity中还有一个startActivityForResult()方法也是用于启动活动的，但这个方法期望在活动销毁的时候能够返回一个结果给上一个活动。
startActivityForResult()方法接收两个参数，第一个参数还是Intent,第二个参数是请
求码，用于在之后的回调中判断数据的来源。

修改FirstActivity中按钮的点击事件，代码如下所示：

 ```java
 	  @Override
       public void onClick(View view) {
           Intent intent = new Intent(FirstActivity.this,SecondActivity.class);
           //用于启动SecondActivity，请求码只要是一个唯一值就可以。
           startActivityForResult(intent,1);
 ```

在SecondActivity中给按钮注册点击事件，并在点击事件中添加返回数据的逻辑

```java
@Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.second_layout);
        Button button2 = (Button) findViewById(R.id.button2);
        button2.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                //仅用于传递数据
                Intent intent = new Intent();
                intent.putExtra("data_return","Hello FirstActivity");
                //setResult()方法专门用于向上一个活动返回数据的;
                //两个参数： 第一个参数用于向上一个活动返回处理结果，一般只使用RESULT_OK或RESULT_CANCELED这两个值
                //第二个参数则把带有数据的Intent传递回去
                setResult(RESULT_OK,intent);
                finish(); //销毁当前活动
            }
        });
    }  
```

由于是用startActivityForResult()方法启动的SecondActivity，在SecondActivity被销毁之后会回调上一个活动的onActivityResult()方法。重写该方法 来获取返回的数据

```Java
@Override
/*
	三个参数
	requestCode  启动活动时传入的请求码
	resultCode   返回数据时传入的处理结果
	data    携带着返回数据的Intent
*/
protected void onActivityResult(int requestCode, int resultCode, @Nullable Intent data) {
    super.onActivityResult(requestCode, resultCode, data);
    switch (requestCode) {
        case 1:
            if (resultCode == RESULT_OK) {
                String returnedData = data.getStringExtra("data_return");
                Log.d("FirstActivity", returnedData);
            }
            break;
        default:
    }
}
```

由于在一个活动中有可能调用startActivityForResult()方法去启动很多不同的活动，每一个活动返回的数据都会回调到onActivityResult()这个方法中，**因此首先要做的就是通过检查requestCode的值来判断数据来源（这个值是唯一的，通过这个值可以判断是哪个活动回调的）。确定数据是从SecondActivity返回的之后，再通过resultCode的值来判断处理结果是否成功，最后从data中取值并打印出来，这样就完成向上一个活动返回数据**

重新运行程序，在SecondActivity中点击Button2会回到FirstActivity

![image-20220411170123213](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220411170123213.png)

按下Back回到FIrstActivity通过重写onBackPressed()来解决。

```JAVA
@Override
    public void onBackPressed() {
        Intent intent = new Intent();
        intent.putExtra("data_return","Hello FirstActivity");
        setResult(RESULT_OK,intent);
        finish();
    }
```

## 3.4 Activity的生命周期

### 3.4.1 返回栈

Android中的Activity是可以层叠的。每启动一个新的Activity,就会覆盖在原Activity之上,然后点击Back键会销毁最上面的Activity,下面的一个Activity就会重新显示出来。
其实Android是使用任务(task)来管理Activity的,一个任务就是一组存放在栈里的Activity
的集合,这个栈也被称作返回栈(back stack)。栈是一种后进先出的数据结构,在默认情况
下,每当启动了一个新的Activity,它就会在返回栈中入栈,并处于栈顶的位置。而每当我
们按下Back键或调用finish()方法去销毁一个Activity时,处于栈顶的Activity就会出栈,前
一个入栈的Activity就会重新处于栈顶的位置。系统总是会显示处于栈顶的Activity给用户

<img src="https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220412093439527.png" alt="image-20220412093439527" style="zoom: 50%;" />

### 3.4.2 Activity状态

每个Activity在其生命周期中最多可能会有4种状态。


|   状态   | 描述                                                         |
| :------: | :----------------------------------------------------------- |
| 运行状态 | 当一个Activity位于返回栈的栈顶时,Activity就处于运行状态。系统最不愿意回收的就是处于运行状态的Activity,因为这会带来非常差的用户体验。 |
| 暂停状态 | **一个Activity不再处于栈顶位置,但仍然可见时**,Activity就进入了暂停状态。比如启动对话框形式的Activity只会占用屏幕中间的部分区域。处于暂停状态的Activity仍然是完全存活着的,系统也不愿意回收这种Activity(因为它还是可见的),只有在内存极低的情况下,系统才会去考虑回收这种Activity。 |
| 停止状态 | 当**当一个Activity不在处于栈顶位置,并且被其他Activity所覆盖处于完全不可见的时候,就进入了停止状态。**但是系统仍然会为这种Activity保存相应的状态和信息,但是这并不是完全可靠的,当其他地方需要内存时,处于停止状态的Activity有可能会被系统回收。 |
| 销毁状态 | 一个Activity从返回栈中移除后就变成了销毁状态。系统最倾向于回收处于这种状态的Activity,以保证手机的内存充足。 |



### 3.4.3 Activity的生存期

Activity类中定义了7个回调方法,覆盖了Activity生命周期的每一个环节,下面是对这7个方法的介绍。

|   方法名    |                             描述                             |
| :---------: | :----------------------------------------------------------: |
| onCreate()  | 在每个Activity中都重写了这个方法,它会在Activity第一次被创建的时候调用。应该在这个方法中完成**Activity的初始化操作,比如加载布局、绑定事件等。** |
|  onStart()  | 启动Activity时被回调，也就是Activity由**不可见变为可见**的时候调用。 |
| onResume()  | 这个方法在Activity**准备好和用户进行交互的时候调用**。此时的Activity一定位于返回栈的栈顶,并且处于运行状态。该方法总是在onPause()方法以后执行。 |
| onPause()。 | **这个方法在系统准备去启动或者恢复另一个Activity的时候调用（暂停Activity时被调用）**。通常会在这个方法中将一些消耗CPU的资源释放掉（？）,以及保存一些关键数据（？）,但这个方法的执行速度一定要快,不然会影响到新的栈顶Activity的使用。 |
|  onStop()   | 这个方法在Activity**完全不可见的时候调用（停止Activity时被调用）**。它和onPause()方法的主要区别在于,如果启动的新Activity是一个对话框式的Activity,那么onPause()方法会得到执行,而onStop()方法并不会执行。 |
| onDestroy() | 这个方法在Activity被销毁之前调用,之后Activity的状态将变为销毁状态。 |
| onRestart() | 这个方法在Activity由停止状态变为运行状态之前调用（重新启动Activity时被回调）。 |

以上7个方法中除了onRestart()方法,其他都是两两相对的,从而又可以将Activity分为以下3种生存期。

- **完整生存期**  Activity在onCreate()方法和onDestroy()方法之间所经历的就是完整生
存期。一般情况下,一个Activity会在onCreate()方法中完成各种初始化操作,而在
onDestroy()方法中完成释放内存的操作。
- **可见生存期 ** Activity在onStart()方法和onStop()方法之间所经历的就是可见生存
期。在可见生存期内,Activity对于用户总是可见的,即便有可能无法和用户进行交互。我们可以通过这两个方法合理地管理那些对用户可见的资源。比如在onStart()方法中对资源进行加载,而在onStop()方法中对资源进行释放,从而保证处于停止状态的Activity不会占用过多内存。
- **前台生存期**。Activity在onResume()方法和onPause()方法之间所经历的就是前台生存期。在前台生存期内,Activity总是处于运行状态,此时的Activity是可以和用户进行交互的,我们平时看到和接触最多的就是这个状态下的Activity。

![Activity的生命周期](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220412105914942.png)

### 3.4.4 体验Activity的生命周期

1. 新建一个ActivityLifeCycleTest项目,这次让AndroidStudio自动创建Activity和布局,创建的Activity名和布局名都使用默认值。

   - 勾选Generate Layout File自动为MainActivity创建一个对应的布局文件,
     勾选Launcher Activity表示会自动将MainActivity设置为当前项目的Activity。

2. 主Activity创建完成,再创建两个子Activity——NormalActivity和DialogActivity

   - 右击com.nyh.activitylifecycletest包→New→Activity→Empty Activity,新建
     NormalActivity,布局起名为normal_layout。然后使用同样的方式创建DialogActivity,布局起名为dialog_layout。

   - 编辑normal_layout.xml文件,将里面的代码替换成如下内容:

     ```xml
     <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
     android:orientation="vertical"
     android:layout_width="match_parent"
     android:layout_height="match_parent">
     <TextView
     android:layout_width="match_parent"
     android:layout_height="wrap_content"
     android:text="This is a normal activity"
     />
     </LinearLayout>
     ```

   - 编辑dialog_layout.xml文件,将里面的代码替换成如下内容:
     ```xml 
     <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
     android:orientation="vertical"
     android:layout_width="match_parent"
     android:layout_height="match_parent">
     <TextView
     android:layout_width="match_parent"
     android:layout_height="wrap_content"
     android:text="This is a dialog activity"
     />
     </LinearLayout> 
     ```

   - NormalActivity和DialogActivity中的代码我们保持默认就好,不需要改动。

3. 将DialogActivity设成对话框式

   - 修改AndroidManifest.xml的``<activity>``标签

   - ```xml
     <!--android:theme属性,用于给当前Activity指定主题,Android系统内置有很多主题可以选择@style/Theme.AppCompat.Dialog是让DialogActivity使用对话框式的主题。-->
     <activity android:name=".DialogActivity"
     android:theme="@style/Theme.AppCompat.Dialog">
     </activity>
     <activity android:name=".NormalActivity">
     </activity>
     ```

4. 修改activity_main.xml,重新定制主Activity的布局 -----加俩按钮用于启动Activity

   - ```xml
     <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
     android:orientation="vertical"
     android:layout_width="match_parent"
     android:layout_height="match_parent">
     <Button
     android:id="@+id/startNormalActivity"
     android:layout_width="match_parent"
     android:layout_height="wrap_content"
     android:text="Start NormalActivity" />
     <Button
     android:id="@+id/startDialogActivity"
     android:layout_width="match_parent"
     android:layout_height="wrap_content"
     android:text="Start DialogActivity" />
     </LinearLayout>
     ```

5. 修改MainActivity

   - ```java
     public class MainActivity extends AppCompatActivity {
         public static final String TAG = "MainActivity";
         @Override
         protected void onCreate(Bundle savedInstanceState) {
             super.onCreate(savedInstanceState);
             setContentView(R.layout.activity_main);
             Button startNormalButton = findViewById(R.id.startNormalActivity);
             Button startDialogButton = findViewById(R.id.startDialogActivity);
             /*
             为两个按钮注册点击事件,点击第一个按钮会启动NormalActivity,
             点击第二个按钮会启动DialogActivity。
             */
             startNormalButton.setOnClickListener(new View.OnClickListener() {
                 @Override
                 public void onClick(View view) {
                     Intent intent = new Intent(MainActivity.this, NormalActivity.class);
                     startActivity(intent);
                 }
             });
             startDialogButton.setOnClickListener(new View.OnClickListener() {
                 @Override
                 public void onClick(View view) {
                     Intent intent = new Intent(MainActivity.this, DialogActivity.class);
                     startActivity(intent);
                 }
             });
             Log.d(TAG,"---------onCreate----------");
         }
     
         @Override
         protected void onStart() {
             super.onStart();
             Log.d(TAG,"---------onstart----------");
         }
     
         @Override
         protected void onResume() {
             super.onResume();
             Log.d(TAG,"---------onResume----------");
         }
         @Override
         protected void onPause() {
             super.onPause();
             Log.d(TAG,"---------onPause----------");
         }
         @Override
         protected void onStop() {
             super.onStop();
             Log.d(TAG,"---------onStop----------");
         }
         @Override
         protected void onDestroy() {
             super.onDestroy();
             Log.d(TAG,"---------onDestroy----------");
         }@Override
         protected void onRestart() {
             super.onRestart();
             Log.d(TAG,"---------onRestart----------");
         }
     }
     ```

测试过程

1. 启动程序时的打印日志
   - ![启动程序时的打印日志](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220412104914857.png)

2. 点击第一个按钮,启动NormalActivity
   - ![打开NormalActivity](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220412105153136.png)
   - 由于NormalActivity已经把MainActivity完全遮挡住,因此onPause()和onStop()方法都会得到执行。
3. 然后按下Back键返回MainActivity
   - ![返回MainActivity时的打印日志](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220412105244418.png)
   - 由于之前MainActivity已经进入了停止状态,所以onRestart()方法会得到执行,之后会依次执行onStart()和onResume()方法。
   - ``此时onCreate()方法不会执行,因为MainActivity并没有重新创建。``
4. 然后点击第二个按钮,启动DialogActivity
   - ![打开DialogActivity时的打印日志](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220412105342837.png)
   - 只有onPause()方法得到了执行,onStop()方法并没有执行,因为DialogActivity并没有完全遮挡住MainActivity,此时MainActivity只是进入了暂停状态,并没有进入停止状态。
5. 按下Back键返回MainActivity也只有onResume()方法会得到执行。
   - ![再次返回MainActivity时的打印日志](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220412105418548.png)

6. 最后在MainActivity按下Back键退出程序，依次会执行onPause()、onStop()和onDestroy()方法,最终销毁MainActivity。
   - ![退出程序时的打印日志](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220412105446040.png)

完整的过程

![Activity完整的生命周期](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220412105831787.png)

### 3.4.5 Activity被回收了怎么办

当一个Activity进入了停止状态,是有可能被系统回收的。

场景:
``应用中有一个Activity A,用户在Activity A的基础上启动了Activity B,Activity A就进入了停止状态,这个时候由于系统内存不足,将Activity A回收掉了,然后用户按下Back键返回Activity A,会出现什么情况?
其实还是会正常显示Activity A的,只不过这时并不会执行onRestart()方法,而是会执行Activity A的onCreate()方法,因为Activity A在这种情况下会被重新创建一次。``
这样看上去好像一切正常,可是有一个重要问题:Activity A中是可能存在临时数据和状态的。比如,MainActivity中如果有一个文本输入框,现在输入了一段文字,然后启动NormalActivity,这时MainActivity由于系统内存不足被回收掉,过了一会又点击了Back键回到MainActivity,刚刚输入的文字都没了,因为MainActivity被重新创建了。

Activity中提供了一个onSaveInstanceState()回调方法,这个方法可以保证在**Activity被回收之前**一定会被调用,可以通过这个方法来解决问题。
onSaveInstanceState()方法会携带一个Bundle类型的参数,Bundle提供了一系列的方法
用于保存数据,比如可以使用putString()方法保存字符串,使用putInt()方法保存整型数据,以此类推。每个保存方法需要传入两个参数,第一个参数是键,用于后面从Bundle中取值,第二个参数是真正要保存的内容。

在MainActivity中添加如下代码讲临时数据进行保存

```java
@Override
protected void onSaveInstanceState(@Nullable Bundle outState) {
    super.onSaveInstanceState(outState);
    String tempData = "Something you just typed";
    outState.putString("data_key",tempData);
}
```

使用的onCreate()方法其实也有一个Bundle类型的参数。这个参数在一般情况下都是null,但是如果在Activity被系统回收之前,通过onSaveInstanceState()方法保存数据,这个参数就会带有之前保存的全部数据,通过相应的取值方法将数据取出即可。
修改MainActivity的onCreate()方法

```Java
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    Log.d(TAG,"---------onCreate----------");
    setContentView(R.layout.activity_main);
    if (savedInstanceState != null){
        String tempData = savedInstanceState.getString("data_key");
        Log.d(TAG,tempData);
    }
}
```

Intent还可以结合Bundle一起用于传递数据。首先把需要传递的数据都保存在Bundle对象中,然后再将Bundle对象存放在Intent里。到了目标Activity之后,先从Intent中取出Bundle,再从Bundle中一一取出数据。

#### 问题：

没有做出来Activity被回收，所以也没看着上面代码的具体效果

## 3.5 Activity的启动模式

在实际项目中应该根据特定的需求为每个Activity指定恰当的启动模式。启动模式一共有4种,分别是standard、singleTop、singleTask和singleInstance,可以在AndroidManifest.xml中通过给<activity>标签指定android:launchMode属性来选择启动模式。

### 3.5.1 standard

standard是Activity默认的启动模式,在不进行显式指定的情况下,所有Activity都会自动使用这种启动模式。到Android是使用返回栈来管理Activity的,在standard模式下,每当启动一个新的Activity,它就会在返回栈中入栈,并处于栈顶的位置。对于使用standard模式的Activity,系统不会在乎这个Activity是否已经在返回栈中存在,每次启动都会创建一个该Activity的新实例。
换到ActivityTest

修改FirstActivity中onCreate()方法的代码

```java
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    Log.d("FirstActivity","===============" + this.toString() + "===============");
    setContentView(R.layout.first_layout);
    Button button1 = (Button) findViewById(R.id.button1);
    button1.setOnClickListener(new View.OnClickListener() {
        @Override
        public void onClick(View view) {
            Intent intent = new Intent(FirstActivity.this,FirstActivity.class);
            startActivity(intent);
        }
    });
}
```

在FirstActivity界面连续点击按钮，打印信息如下

![image-20220412142124120](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220412142124120.png)

每一次点击都会创建一个新的FIrstActivity，此时返回栈中也会存在3个FirstActivity的实例，退出程序需要连续多按几次才行。

<img src="https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220412142628421.png" alt="standard模式示意图" style="zoom: 80%;" />

### 3.5.2  singleTop

singleTop模式。当Activity的启动模式指定为singleTop,在启动Activity时如果发现返回栈的栈顶已经是该Activity,则认为可以直接使用它,不会再创建新的Activity实例。
修改AndroidManifest.xml中FirstActivity的启动模式,如下所示:

```xml
<!--在activity标签中添加  android:launchMode="singleTop" -->
<activity
          android:name=".FirstActivity"
          android:exported="true"
          android:launchMode="singleTop"
          android:label="This is FirstActivity">
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>
</activity>
```

启动程序，此时不管点多少下按钮，都只会有一条打印信息。因为目前FirstActivity已经处于栈顶，每当要再启动一个FirstActivity时，都会直接使用栈顶的活动，因此FirstActivity只有一个实例。

当FirstActivity未处于栈顶位置时，此时再启动FirstActivity，还是会创建新的实例。

修改FirstActivity中onCreate（）方法,点击按钮启动SecondActivity。

```java
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    Log.d("FirstActivity","===============" + this.toString() + "===============");
    setContentView(R.layout.first_layout);
    Button button1 = (Button) findViewById(R.id.button1);
    button1.setOnClickListener(new View.OnClickListener() {
        @Override
        public void onClick(View view) {
            Intent intent = new Intent(FirstActivity.this,SecondActivity.class);
            startActivity(intent);
        }
    });
}
```

修改SecondActivity中的onCreate方法，点击按钮启动FirstActivity

```java
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    Log.d("SecondActivity","===============" + this.toString() + "===============");
    setContentView(R.layout.second_layout);
    Button button2 = (Button) findViewById(R.id.button2);
    button2.setOnClickListener(new View.OnClickListener() {
        @Override
        public void onClick(View view) {
            Intent intent = new Intent(SecondActivity.this, FirstActivity.class);
            startActivity(intent);
        }
    });
}
```

此时创建了两个不同的FirstActivity实例。

![image-20220412143635426](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220412143635426.png)

![singleTop模式原理图](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220412144604199.png)

### 3.5.3 singleTask

singleTask模式：让某个Activity在整个应用程序的上下文中只存在一个实例。

当Activity的启动模式指定为singleTask,每次启动该Activity时,系统首先会在返回栈中检查是否存在该Activity的实例,如果发现已经存在则直接使用该实例,并把在这个Activity之上的所有其他Activity统统出栈,如果没有发现就会创建一个新的Activity实例。

修改AndroidManifest.xml中FirstActivity的启动模式

```xml
android:launchMode="singleTask"
```

然后在FirstActivity中添加onRestart()方法,并打印日志

```java
@Override
protected void onRestart() {
    super.onRestart();
    Log.d("FirstActivity", "=========onRestart========");
}
```

最后在SecondActivity中添加onDestroy()方法,并打印日志

```java
@Override
protected void onDestroy() {
    super.onDestroy();
    Log.d("FirstActivity", "===========onDestroy==========");
}
```

现在重新运行程序,在FirstActivity界面点击按钮进入SecondActivity,然后在SecondActivity界面点击按钮,又会重新进入FirstActivity。

![singleTask模式下的打印日志](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220412150409056.png)

在SecondActivity中启动FirstActivity时,会发现返回栈中已经存在一个FirstActivity的实例,并且是在SecondActivity的下面,于是SecondActivity会从返回栈中出栈,而FirstActivity重新成为了栈顶Activity,因此FirstActivity的onRestart()方法和SecondActivity的onDestroy()方法会得到执行。现在返回栈中只剩下
一个FirstActivity的实例了,按一下Back键就可以退出程序。

![singleTask模式原理示意图](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220412150546383.png)

### 3.5.4 singleInstance

singleInstance模式的Activity会启用一个新的返回栈来管理这个Activity(其实如果singleTask模式指定了不同的taskAffinity,也会启动一个新的返回栈)。

场景：

假设我们的程序中有一个是允许其他程序调用的,如果想实现其他程序和我们的程序可以共享这个Activity的实
例,应该如何实现呢?使用前面3种启动模式肯定是做不到的,因为每个应用程序都会有自己的返回栈,同一个Activity在不同的返回栈中入栈时必然创建了新的实例。而使用singleInstance模式就可以解决这个问题,**在这种模式下,会有一个单独的返回栈来管理这个Activity**,不管是哪个应用程序来访问这个Activity,都共用同一个返回栈,也就解决了共享Activity实例的问题。

修改AndroidManifest.xml中SecondActivity的启动模式，将SecondActivity的启动模式指定为singleInstance

```xml
<activity
        android:name=".SecondActivity"
        android:launchMode="singleInstance"
        android:exported="false">
    <intent-filter>
    <action android:name="com.nyh.activitytest.ACTION_START" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="com.nyh.activitytest.MY_CATEGORY" />
    </intent-filter>
</activity>
```

修改FirstActivity中onCreate()方法的代码  

```java
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    //打印当前返回栈的id
    Log.d("FirstActivity","=====Task id is " + getTaskId() + "=====");
    setContentView(R.layout.first_layout);
    Button button1 = (Button) findViewById(R.id.button1);
    button1.setOnClickListener(new View.OnClickListener() {
        @Override
        public void onClick(View view) {
            Intent intent = new Intent(FirstActivity.this,SecondActivity.class);
            startActivity(intent);
        }
    });
}
```

修改SecondActivity中onCreate()方法的代码  

```Java
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    Log.d("SecondActivity","=====Task id is " + getTaskId() + "=====");
    setContentView(R.layout.second_layout);
    Button button2 = (Button) findViewById(R.id.button2);
    button2.setOnClickListener(new View.OnClickListener() {
        @Override
        public void onClick(View view) {
            Intent intent = new Intent(SecondActivity.this, ThirdActivity.class);
            startActivity(intent);
        }
    });
}
```

最后修改ThirdActivity中onCreate()方法的代码

```java
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    Log.d("ThirdActivity","=====Task id is " + getTaskId() + "=====");
    setContentView(R.layout.third_layout);
}
```

运行程序在FirstActivity点击按钮进入到SecondActivity，在SecondActivity中点击按钮进入到ThirdActivity

![singleInstance模式下的打印](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220412153532533.png)

说明SecondActivity是存放在一个单独的返回栈里。

按下Back键进行返回,ThirdActivity竟然直接返回到了FirstActivity,再按下Back键又会返回到SecondActivity,再按下Back键才会退出程序；原理很简单,由于FirstActivity和ThirdActivity是存放在同一个返回栈里的,当在ThirdActivity的界面按下Back键时,ThirdActivity会从返回栈中出栈,那么FirstActivity就成为了栈顶Activity显示在界面上,因此也就出现了从ThirdActivity直接返回到FirstActivity的情况。然后在FirstActivity界面再次按下Back键,这时当前的返回栈已经空了,于是就显示了另一个返回栈的栈顶Activity,即SecondActivity。最后再次按下Back键,这时所有返回栈都已经空了,退出了程序。

![singleInstance模式原理示意图](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220412154003670.png)

## 3.6 Activity的最佳实践

### 3.6.1 知道当前是在哪一个Activity

在ActivityTest项目的基础上修改，新建一个BaseActivity类。让其继承AppCompatActivity，并重写onCreate方法

```java
public class BaseActivity extends AppCompatActivity {
    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        //打印当前实例的类名
        Log.d("BaseActivity", "---->" + getClass().getSimpleName() + "<----");
    }
}
```

让BaseActivity成为ActivityTest项目中所有活动的父类。

重新运行程序，通过点击按钮分别进入到FirstActivity，SecondActivity，ThirdActivity的界面，打印信息：

![image-20220412160914258](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220412160914258.png)

每进入到一个活动页面，该活动的类名就会被打印出来。

### 3.6.2 随时随地退出程序

在ThirdActivity想退出程序是非常不方便的,需要连按3次Back键才行。按Home键只是把程序挂起,并没有退出程序。我们的程序需要注销或者退出的功能，要有一个随时随地都能退出程序的方案。

新建一个ActivityCollector类作为活动管理器

```Java
public class ActivityCollector {
    public static List<Activity> activities = new ArrayList<>();

    public static void addActivity(Activity activity) {
        activities.add(activity);
    }

    public static void removeActivity(Activity activity) {
        activities.remove(activity);
    }

    public static void finishAll() {
        for (Activity activity : activities) {
            if (!activity.isFinishing()) {
                activity.finish();
            }
        }
    }
}
```

通过一个List暂存活动，addActivity方法向list中添加一个活动，removeActivity方法移除活动，finishAll方法将List中存储的活动全部销毁。

修改BaseActivity

```java
public class BaseActivity extends AppCompatActivity {
    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        Log.d("BaseActivity", "---->" + getClass().getSimpleName() + "<----");
        // 活动启动时自动添加到集合
        ActivityCollector.addActivity(this);
    }

    //销毁时移除
    @Override
    protected void onDestroy() {
        super.onDestroy();
        ActivityCollector.removeActivity(this);
    }
}
```

修改ThirdActivity

```java
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    Log.d("ThirdActivity","=====Task id is " + getTaskId() + "=====");
    setContentView(R.layout.third_layout);
    Button btn3 = findViewById(R.id.button3);
    btn3.setOnClickListener(new View.OnClickListener() {
        @Override
        public void onClick(View view) {
            //集合里所有Activity执行一个finish
            ActivityCollector.finishAll();
        }
    });
}
```

```Java
android.os.Process.killProcess(android.os.Process.myPid());
```

killProcess()方法用于杀掉一个进程,它接收一个进程id参数,通过myPid()方法来获得当前程序的进程id。需要注意的是,killProcess()方法只能用于杀掉当前程序的进程,不能用于杀掉其他程序。

疑问：activity.isFinishing()这个方法是判断是否结束的？

### 3.6.3 启动活动的最佳写法

启动Activity的方法,首先通过Intent构建出当前的“意图”,然后调用startActivity()或startActivityForResult()方法将Activity启动起来,如果有数据需要在Activity之间传递,也可以借助Intent来完成。
假设SecondActivity中需要用到两个非常重要的字符串参数,在启动SecondActivity的时候必须传递过来,那么可以使用如下代码:

```java
Intent intent = new Intent(FirstActivity.this, SecondActivity.class);
intent.putExtra("param1", "data1");
intent.putExtra("param2", "data2");
startActivity(intent);
```

虽然这样写是完全正确的,但是在真正的项目开发中经常会出现对接的问题。比如SecondActivity不是由你开发的,但现在你负责开发的部分需要启动SecondActivity,而你却不清楚启动SecondActivity需要传递哪些数据。这时无非就有两个办法:一个是你自己去阅读SecondActivity中的代码,另一个是询问负责编写SecondActivity的同事。你会不会觉得很麻烦呢?其实只需要换一种写法,就可以轻松解决上面的窘境。
修改SecondActivity中的代码,如下所示:

```java
public class SecondActivity extends BaseActivity{
	public static void actionStart(Context context,String data1,String data2) {
		Intent intent = new Intent(context, SecondActivity.class);
		intent.putExtra("param1", data1);
		intent.putExtra("param2", data2);
		context.startActivity(intent)
	}
    ...
}
```

在SecondActivity中添加了一个actionStart()方法，在这个方法中完成了Intent的构建，另外所有SecondActivity中需要的数据都是通过actionStart()方法的参数传递过来的，然后把他们存储到Intent中，最后调用startActivity()方法启动SecondActivity。

SecondActivity所需要的数据在方法参数中全部体现出来了，可以非常清晰的知道启动SecondActivity需要传递哪些数据。另外，只需要一行代码就可以启动SecondActivity了。

```java
button1.setOnClickListener(new OnClickListener(){
    @override
    public void onClick(View view) {
		SecondActivity.actionStart(FirstActivity.this,"data1","data2");
    }
});
```











``两个可能会用到的网站``

[解决Android Studio报The emulator process for AVD Pixel_4a_Edited_API_28 has terminated.类似错误 - Tireless - 博客园 (cnblogs.com)](https://www.cnblogs.com/a001jingzhao/p/15211160.html)

[(57条消息) 对于Android虚拟机启动失败“The emulator process for AVD Nexus_5X_API_30 was killed”的解决方案_卉卉今天喝水了吗的博客-CSDN博客](https://blog.csdn.net/weixin_46523794/article/details/116156366)

