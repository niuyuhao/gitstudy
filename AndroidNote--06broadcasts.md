# 广播机制

## 6.1 广播机制简介

为什么Android中的广播机制更加灵活呢?

Android中的每个应用程序都可以对自己感兴趣的广播进行注册,这样该程序就只会收到自己所关心的广播内容,这些广播可能是来自于系统的,也可能是来自于其他应用程序的。Android提供了一套完整的API,允许应用程序自由地发送和接收广播。发送广播的方法其实之前稍微提到过,如果你记性好的话,可能还会有印象,就是借助我们第3章学过的Intent。而接收广播的方法则需要引入一个新的概念——BroadcastReceiver。BroadcastReceiver的具体用法将会在下一节介绍,这里我们先来了解一下广播的类型。

Android中的广播主要可以分为两种类型:标准广播和有序广播。

 - 标准广播(normal broadcasts)是一种完全异步执行的广播,在广播发出之后,所有的BroadcastReceiver几乎会在同一时刻收到这条广播消息,因此它们之间没有任何先后顺序可言。这种广播的效率会比较高,但同时也意味着它是无法被截断的。
   - <img src="https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220506174929122.png" alt="标准广播工作示意图" style="zoom: 25%;" />
 - 有序广播(ordered broadcasts)则是一种同步执行的广播,在广播发出之后,同一时刻只会有一个BroadcastReceiver能够收到这条广播消息,当这个BroadcastReceiver中的逻辑执行完毕后,广播才会继续传递。所以此时的BroadcastReceiver是有先后顺序的,优先级高的BroadcastReceiver就可以先收到广播消息,并且前面的BroadcastReceiver还可以截断正在传递的广播,这样后面的BroadcastReceiver就无法收到广播消息了。
   - <img src="https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220506175052549.png" alt="有序广播工作示意图" style="zoom:25%;" />

## 6.2 接收系统广播

Android内置了很多系统级别的广播,可以在应用程序中通过监听这些广播来得到各种系统的状态信息。比如手机开机完成后会发出一条广播,电池的电量发生变化会发出一条广播,系统时间发生改变也会发出一条广播,等等。如果想要接收这些广播,就需要使用BroadcastReceiver,下面来看一下它的具体用法。

### 6.2.1 动态注册监听网络变化

广播接收器可以自由地对根据自己感兴趣的广播进行注册,这样当有相应的广播发出时,相应的BroadcastReceiver就能够收到该广播,并可以在内部进行逻辑处理。注册BroadcastReceiver（广播接收器）的方式一般有两种:在代码中注册和在AndroidManifest.xml中注册。前者也被称为动态注册,后者也被称为静态注册。

如何创建一个BroadcastReceiver?新建一个类,让它继承自BroadcastReceiver,并重写父类的onReceive()方法。这样当有广播到来时,onReceive()方法就会得到执行,具体的逻辑就可以在这个方法中处理。下面先通过动态注册的方式编写一个能够监听网络变化的程序,借此学习一下BroadcastReceiver的基本用法。新建一个BroadcastTest项目,然后修改MainActivity中的代码,如下所示:

```Java
public class MainActivity extends AppCompatActivity {

    private IntentFilter intentFilter;
    private NetworkChangeReceiver networkChangeReceiver;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        intentFilter = new IntentFilter();
        intentFilter.addAction("android.net.conn.CONNECTIVITY_CHANGE");
        networkChangeReceiver = new NetworkChangeReceiver();
        registerReceiver(networkChangeReceiver,intentFilter);
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        unregisterReceiver(networkChangeReceiver);
    }

    class NetworkChangeReceiver extends BroadcastReceiver {

        @Override
        public void onReceive(Context context, Intent intent) {
            ConnectivityManager connectionManager = (ConnectivityManager) getSystemService(Context.CONNECTIVITY_SERVICE);
            NetworkInfo networkInfo = connectionManager.getActiveNetworkInfo();
            if (networkInfo != null && networkInfo.isAvailable()){
                Toast.makeText(context,"network is available",Toast.LENGTH_SHORT).show();
            }else {
                Toast.makeText(context,"network is unavailable",Toast.LENGTH_SHORT).show();
            }

        }
    }
}
```

在MainActivity中定义了一个内部类NetworkChangeReceiver,这个类是继承自BroadcastReceiver的，并重写了父类的onReceive()方法。每当网络状态发生变化时，onReceive()方法就会得到执行，这里只是简单地使用Toast提示了一段文本信息。

onCreate()方法，首先创建了个IntentFilter的实例，并给它添加了一个值为android.net.conn.CONNECTIVITY CHANGE的action,为什么要添加这个值呢？因为当网络状态发生变化时，系统发出的正是一条值为android.net.conn.CONNECTIVITY CHANGE的广播，也就是广播接收器想要监听什么广播，就在这里添加相应的action.。接下来创建了一个NetworkChangeReceiver的实例，然后调用registerReceiver()方法进行注册，将NetworkChangeReceiver的实例和IntentFilter的实例都传了进去，这样NetworkChange-Receiver就会收到所有值为android.net.conn.CONNECTIVITY_CHANGE的广播，也就实现了监听网络变化的功能。

最后要记得，动态注册的广播接收器一定都要取消注册才行，这里我们是在onDestroy()方法中通过调用unregisterReceiver()方法来实现的。运行一下程序能准确地告诉用户当前是有网络还是没有网络。

在onReceive()方法中，首先通过getSystemService()方法得到了ConnectivityManager的实例，这是一个**系统服务类**，专门用于管理网络连接的。然后调用它的getActiveNetwork-Info()方法可以得到NetworkInfo的实例，接着调用NetworkInfo的isAvailable()方法就可以判断出当前是否有网络了，最后我们还是通过Tost的方式对用户进行提示。

另外，这里有非常重要的一点需要说明，Android系统为了保护用户设备的安全和隐私，做了严格的规定：如果程序需要进行一些对用户来说比较敏感的操作，就必须在配置文件中声明权限才可以，否则程序将会直接崩溃。比如这里访问系统的网络状态就是需要声明权限的。打开AndroidManifest..xml文件，在里面加入如下权限就可以访问系统网络状态了：

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.nyh.broadcasttest">
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    ...
</manifest>
```

### 6.2.2 静态注册实现开机启动

动态注册的广播接收器可以自由地控制注册与注销，在灵活性方面有很大的优势，但是它也存在着一个缺点，即必须要在程序启动之后才能接收到广播，因为注册的逻辑是写在onCreate()方法中的。那么有没有什么办法可以让程序在未启动的情况下就能接收到广播呢？这就需要使用静态注册的方式了。
这里我们准备让程序接收一条开机广播，当收到这条广播时就可以在onReceive()方法里执行相应的逻辑，从而实现开机启动的功能。可以使用Android Studio提供的快捷方式来创建一个广播接收器，右击com.example.broadcasttest包→New→Other→Broadcast Receiver,弹出如图窗口

<img src="https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220506184224577.png" alt="image-20220506184224577" style="zoom: 33%;" />

将广播接收器命名为BootCompleteReceiver,Exported属性表示是否允许这个广播接收器接收本程序以外的广播，Enabled属性表示是否启用这个广播接收器。勾选这两个属性，点击Finish完成创建。
然后修改BootCompleteReceiver中的代码，如下所示：

```Java
public class BootCompleteReceiver extends BroadcastReceiver {

    @Override
    public void onReceive(Context context, Intent intent) {
        // TODO: This method is called when the BroadcastReceiver is receiving
        // an Intent broadcast.
        Toast.makeText(context,"Boot Complete",Toast.LENGTH_LONG).show();
    }
}
```

静态的广播接收器一定要在AndroidManifest..xml文件中注册才可以使用，不过由于是使用Android Studio的快捷方式创建的广播接收器，因此注册这一步已经被自动完成了。打开AndroidManifest..xml文件，代码如下所示：

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.nyh.broadcasttest">

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.BroadcastTest">
        ...
        <receiver
            android:name=".BootCompleteReceiver"
            android:enabled="true"
            android:exported="true"></receiver>
    </application>

    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />

</manifest>
```

可以看到，<application>:标签内出现了一个新的标签<receiver>,所有静态的广播接收器都是在这里进行注册的。它的用法其实和<activity>标签非常相似，也是通过android:name来指定具体注册哪一个广播接收器，而enabled和exported属性则是根据我们刚才勾选的状态自动生成的。
不过目前BootCompleteReceiver还是不能接收到开机广播的，还需要对AndroidManifest.xml文件进行修改才行，如下所示：

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.nyh.broadcasttest">

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.BroadcastTest">
      ...
        <receiver
            android:name=".BootCompleteReceiver"
            android:enabled="true"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED" />
            </intent-filter>
        </receiver>
    </application>

    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
</manifest>
```

由于Android系统启动完成后会发出一条值为android.intent.action.B00TC0 MPLETED的广播，因此在<intent-filter>标签里添加了相应的action。另外，监听系统开机广播也是需要声明权限的，可以看到，我们使用<uses-permission>标签又加人了一条android.permission.RECEIVE B00TC0 MPLETED权限。

重新运行程序。将模拟器关闭并重新启动。



到目前为止，我们在广播接收器的onReceive()方法中都只是简单地使用Toast提示了一段文本信息，当你真正在项目中使用到它的时候，就可以在里面编写自己的逻辑。需要注意的是不要在onReceive()方法中添加过多的逻辑或者进行任何的耗时操作，因为在广播接收器中是不允许开启线程的，当onReceive()方法运行了较长时间而没有结束时，程序就会报错。因此广播接收器更多的是扮演一种打开程序其他组件的角色，比如创建一条状态栏通知，或者启动一个服务等，这几个概念我们会在后面的章节中学到。

## 6.3 发送自定义广播

## 6.4 广播的最佳实践

