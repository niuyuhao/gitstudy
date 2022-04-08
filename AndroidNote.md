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

   - Activity是所有Android应用程序的门面,凡是在应用中你看得到的东西,都是放在Activity中的。
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
   这个目录主要包含了一些在编译时自动生成的文件,你也不需要过多关心。
4. gradle
   这个目录下包含了gradle wrapper的配置文件,使用gradle wrapper的方式不需要提前将gradle下载好,而是会自动根据本地的缓存情况决定是否需要联网下载gradle。
   Android Studio默认就是启用gradle wrapper方式的,如果需要更改成离线模式,可以点击Android Studio导航栏→File→Settings→Build, Execution,Deployment→Gradle,进行配置更改。
5. .gitignore
   这个文件是用来将指定的目录或文件排除在版本控制之外的。关于版本控制,我们将在第6章中开始正式的学习。
6. build.gradle
   这是项目全局的gradle构建脚本,通常这个文件中的内容是不需要修改的。稍后我们将会
   详细分析gradle构建脚本中的具体内容。
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
毫无疑问,**java目录是放置我们所有Java代码的地方**(Kotlin代码也放在这里),展开该目录,你将看到系统帮我们自动生成了一个MainActivity文件。
05. res
这个目录下的内容就有点多了。简单点说,就是你**在项目中使用到的所有图片、布局、字符串等资源都要存放在这个目录下**。当然这个目录下还有很多子目录,图片放在drawable目录下,布局放在layout目录下,字符串放在values目录下,所以你不用担心会把整个res目录弄得乱糟糟的。
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

当把Logcat中的级别选中为Info、Warn或者Error时,在onCreate()方法中打印的语句会显示,因为打印日志时使用的是Log.d()方法。
