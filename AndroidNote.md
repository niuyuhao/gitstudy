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
