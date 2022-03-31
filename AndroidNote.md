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

<img src="/home/ts/snap/typora/58/.config/Typora/typora-user-images/image-20220331161352665.png" alt="image-20220331161352665" style="zoom: 67%;" />

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

<img src="/home/ts/snap/typora/58/.config/Typora/typora-user-images/image-20220331175442230.png" alt="image-20220331175442230" style="zoom:50%;" />

**Project模式的项目结构**

<img src="/home/ts/snap/typora/58/.config/Typora/typora-user-images/image-20220331175546815.png" alt="image-20220331175546815" style="zoom:50%;" />

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

<img src="/home/ts/snap/typora/58/.config/Typora/typora-user-images/image-20220331180354971.png" alt="image-20220331180354971" style="zoom: 67%;" />

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
