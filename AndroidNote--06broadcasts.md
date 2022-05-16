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

**广播接收器可以自由地对根据自己感兴趣的广播进行注册,这样当有相应的广播发出时,相应的BroadcastReceiver就能够收到该广播,并可以在内部进行逻辑处理。**注册BroadcastReceiver（广播接收器）的方式一般有两种:

在代码中注册和在AndroidManifest.xml中注册。前者也被称为动态注册,后者也被称为静态注册。

如何创建一个BroadcastReceiver?

新建一个类,让它继承自BroadcastReceiver,并重写父类的onReceive()方法。这样当有广播到来时,onReceive()方法就会得到执行,具体的逻辑就可以在这个方法中处理。下面先通过动态注册的方式编写一个能够监听网络变化的程序,借此学习一下BroadcastReceiver的基本用法。新建一个BroadcastTest项目,然后修改MainActivity中的代码,如下所示:

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

onCreate()方法，首先创建了个IntentFilter的实例，并给它添加了一个值为android.net.conn.CONNECTIVITY CHANGE的action,为什么要添加这个值呢？

因为当网络状态发生变化时，系统发出的正是一条值为android.net.conn.CONNECTIVITY CHANGE的广播，也就是广播接收器想要监听什么广播，就在这里添加相应的action.。接下来创建了一个NetworkChangeReceiver的实例，然后调用registerReceiver()方法进行注册，将NetworkChangeReceiver的实例和IntentFilter的实例都传了进去，这样NetworkChange-Receiver就会收到所有值为android.net.conn.CONNECTIVITY_CHANGE的广播，也就实现了监听网络变化的功能。

最后要记得，动态注册的广播接收器一定都要取消注册才行，这里我们是在onDestroy()方法中通过调用unregisterReceiver()方法来实现的。运行一下程序能准确地告诉用户当前是有网络还是没有网络。

在onReceive()方法中，首先通过getSystemService()方法得到了ConnectivityManager的实例，这是一个**系统服务类**，专门用于管理网络连接的。然后调用它的getActiveNetwork-Info()方法可以得到NetworkInfo的实例，接着调用NetworkInfo的isAvailable()方法就可以判断出当前是否有网络了，最后我们还是通过Tost的方式对用户进行提示。

另外，这里有非常重要的一点需要说明，Android系统为了保护用户设备的安全和隐私，做了严格的规定：如果程序需要进行一些对用户来说比较敏感的操作，就必须在配置文件中声明权限才可以，否则程序将会直接崩溃。比如这里访问系统的网络状态就是需要声明权限的。打开AndroidManifest.xml文件，在里面加入如下权限就可以访问系统网络状态了：

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.nyh.broadcasttest">
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    ...
</manifest>
```

### 6.2.2 静态注册实现开机启动

动态注册的广播接收器可以自由地控制注册与注销，在灵活性方面有很大的优势，但是它也存在着一个缺点，即**必须要在程序启动之后才能接收到广播**，因为注册的逻辑是写在onCreate()方法中的。

**让程序在未启动的情况下就能接收到广播需要使用静态注册的方式。**
这里我们准备让程序接收一条开机广播，当收到这条广播时就可以在onReceive()方法里执行相应的逻辑，从而实现开机启动的功能。可以使用Android Studio提供的快捷方式来创建一个广播接收器，右击com.example.broadcasttest包→New→Other→Broadcast Receiver,弹出如图窗口

<img src="https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220506184224577.png" alt="image-20220506184224577" style="zoom: 33%;" />

将广播接收器命名为BootCompleteReceiver

Exported属性表示是否允许这个广播接收器接收本程序以外的广播

Enabled属性表示是否启用这个广播接收器。勾选这两个属性，点击Finish完成创建。
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

<application>:标签内出现了一个新的标签<receiver>,**所有静态的广播接收器都是在这里进行注册的**。它的用法其实和<activity>标签非常相似，也是通过android:name来指定具体注册哪一个广播接收器，而enabled和exported属性则是根据我们刚才勾选的状态自动生成的。
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

由于Android系统启动完成后会发出一条值为android.intent.action.BOOT_COMPLETED的广播，因此在<intent-filter>标签里添加了相应的action。另外，监听系统开机广播也是需要声明权限的，可以看到，我们使用<uses-permission>标签又加人了一条android.permission.RECEIVE B00TC0 MPLETED权限。

重新运行程序。将模拟器关闭并重新启动。

到目前为止在广播接收器的onReceive()方法中都只是简单地使用Toast提示了一段文本信息，使用的时候可以在里面编写自己的逻辑。需要注意的是**不要在onReceive()方法中添加过多的逻辑或者进行任何的耗时操作，因为在广播接收器中是不允许开启线程的，当onReceive()方法运行了较长时间而没有结束时，程序就会报错**。因此广播接收器更多的是扮演一种打开程序其他组件的角色，比如创建一条状态栏通知，或者启动一个服务等，这几个概念我们会在后面的章节中学到。

## 6.3 发送自定义广播

如何在应用程序中发送自定义的广播。广播主要分为两种类型：标准广播和有序广播，在本节中通过实践的方式来看一下这两种广播具体的区别。

### 6.3.1 发送标准广播

在发送广播之前，需要先定义一个广播接收器来准备接收此广播才行。新建一个MyBroadcastReceiver

```Java
public class MyBroadcastReceiver extends BroadcastReceiver {

    @Override
    public void onReceive(Context context, Intent intent) {
        Toast.makeText(context,"received in MyBroadcastReceiver",Toast.LENGTH_SHORT).show();
    }
}
```

当MyBroadcastReceiver收到自定义的广播时,就会弹出“received in MyBroadcastReceiver”的提示。
然后在AndroidManifest.xml中对这个BroadcastReceiver进行修改

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.nyh.broadcasttest">

    <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.BroadcastTest">
	...
        <receiver
            android:name=".MyBroadcastReceiver"
            android:enabled="true"
            android:exported="true">
            <intent-filter>
                <action android:name="com.nyh.broadcasttest.MY_BROADCAST" />
            </intent-filter>
        </receiver>
    </application>
</manifest>
```

MyBroadcastReceiver接收一条值为com.example.broadcasttest.MY_BROADCAST的广播,因此待会儿在发送广播的时候,就需要发出这样的一条广播。
接下来修改activity_main.xml中的代码,如下所示:

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

        Button btn = findViewById(R.id.btn);
        btn.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent("com.nyh.broadcasttest.MY_BROADCAST");
                intent.setPackage("com.nyh.broadcasttest");
                sendBroadcast(intent);
                Log.d("test","--------1--------");
            }
        });

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

在按钮的点击事件里面加入了发送自定义广播的逻辑。

首先构建了一个Intent对象,并把要发送的广播的值传入。然后调用Intent的setPackage()方法,并传入当前应用程序的包名。最后调用sendBroadcast()方法将广播发送出去,这样所有监听com.example.broadcasttest.MY_BROADCAST这条广播的BroadcastReceiver就会收到消息了。此时发出去的广播就是一条标准广播。

对第2步调用的setPackage()方法进行更详细的说明。前面已经说过,在Android8.0系统之后,静态注册的BroadcastReceiver是无法接收隐式广播的,而默认情况下我们发出的自定义广播恰恰都是隐式广播。因此这里一定要调用setPackage()方法,指定这条广播是发送给哪个应用程序的,从而让它变成一条显式广播,否则静态注册的BroadcastReceiver将无法接收到这条广播。

### 6.3.2 发送有序广播

和标准广播不同,有序广播是一种同步执行的广播,并且是可以被截断的。为了验证这一点,再创建一个新的BroadcastReceiver。新建AnotherBroadcastReceiver,代码如下所示:

```Java
public class AnotherBroadcastReceiver extends BroadcastReceiver {
    @Override
    public void onReceive(Context context, Intent intent) {
        Toast.makeText(context,"received in AnotherBroadcastReceiver",Toast.LENGTH_SHORT).show();
    }
}
```

然后在AndroidManifest.xml中对这个BroadcastReceiver的配置进行修改

```xml
<receiver
android:name=".AnotherBroadcastReceiver"
android:enabled="true"
android:exported="true">
<intent-filter>
<action android:name="com.example.broadcasttest.MY_BROADCAST" />
</intent-filter>
</receiver>
```

AnotherBroadcastReceiver同样接收的是com.example.broadcasttest.MY_BROADCAST这条广播。现在重新运行程序,并点击“Send Broadcast”按钮,就会分别弹出两次提示信息

不过,到目前为止,程序发出的都是标准广播,现在尝试一下发送有序广播。重新回到BroadcastTest项目,然后修改MainActivity中的代码,如下所示:

```Java
btn.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent("com.nyh.broadcasttest.MY_BROADCAST");
                intent.setPackage("com.nyh.broadcasttest");
//                发送标准广播
                //                sendBroadcast(intent);
//                发送有序广播
                sendOrderedBroadcast(intent,null);
                
            }
        });
```

发送有序广播只需要改动一行代码,即将sendBroadcast()方法改成sendOrderedBroadcast()方法。sendOrderedBroadcast()方法接收两个参数:

第一个参数仍然是Intent;

第二个参数是一个与权限相关的字符串,这里传入null就行了。

现在重新运行程序,并点击“Send Broadcast”按钮,两个BroadcastReceiver仍然都可以收到这条广播。
看上去好像和标准广播并没有什么区别。不过这个时候的BroadcastReceiver是有先后顺序的,而且前面的BroadcastReceiver还可以将广播截断,以阻止其继续传播。

在注册的时候设定BroadcastReceiver的先后顺序，修改AndroidManifest.xml中的代码,如下所示

```xml
<receiver
android:name=".MyBroadcastReceiver"
android:enabled="true"
android:exported="true">
<intent-filter android:priority="100">
<action android:name="com.example.broadcasttest.MY_BROADCAST"/>
</intent-filter>
</receiver>
```

通过android:priority属性给BroadcastReceiver设置了优先级,优先级比较高的BroadcastReceiver就可以先收到广播。这里将MyBroadcastReceiver的优先级设成了100,以保证它一定会在AnotherBroadcastReceiver之前收到广播。既然已经获得了接收广播的优先权,那么MyBroadcastReceiver就可以选择是否允许广播继续传递了。修改MyBroadcastReceiver中的代码,如下所示:

```Java
public class MyBroadcastReceiver extends BroadcastReceiver {

    @Override
    public void onReceive(Context context, Intent intent) {
        Toast.makeText(context,"received in MyBroadcastReceiver",Toast.LENGTH_SHORT).show();
        //如果在onReceive()方法中调用了abortBroadcast()方法,就表示将这条广播截断,后面的BroadcastReceiver将无法再接收到这条广播。
        abortBroadcast();
    }
}
```



## 6.4 广播的最佳实践

强制下线应该算是一个比较常见的功能,比如如果你的QQ号在别处登录了,就会将你强制挤下线。实现强制下线功能的思路:**在界面上弹出一个对话框,让用户无法进行任何其他操作,必须点击对话框中的“确定”按钮,然后回到登录界面**。可是这样就会存在一个问题:当用户被通知需要强制下线时,可能正处于任何一个界面,难道要在每个界面上都编写一个弹出对话框的逻辑?如果你真的这么想,那思路就偏远了。我们完全可以借助本章所学的广播知识,非常轻松地实现这一功能。新建一个BroadcastBestPractice项目,然后开始动手吧。

强制下线功能需要先关闭所有的Activity,然后回到登录界面。在第3章的最佳实践部分已经实现过关闭所有Activity的功能了,因此这里使用同样的方案即可。先创建一个ActivityCollector类用于管理所有的Activity,代码如下所示:

```java
public class ActivityController {
    public static List<Activity> activities = new ArrayList<>();

    public static void addActivity(Activity activity){
        activities.add(activity);
    }
    public static void removeActivity(Activity activity){
        activities.remove(activity);
    }
    public static void finishAll() {
        for (Activity activity : activities) {
            if (!activity.isFinishing()){
                activity.finish();
            }
        }
    }
}
```

然后创建BaseActivity类作为所有Activity的父类,代码如下所示

```java
public class BaseActivity extends AppCompatActivity {

    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        ActivityController.addActivity(this);
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        ActivityController.removeActivity(this);
    }
}
```

首先需要创建一个LoginActivity来作为登录界面,并让Android Studio帮我们自动生成相应的布局文件。然后编辑布局文件activity_login.xml,代码如下所示

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
android:orientation="vertical"
android:layout_width="match_parent"
android:layout_height="match_parent">
    <LinearLayout
    android:orientation="horizontal"
    android:layout_width="match_parent"
    android:layout_height="60dp">
    <TextView
    android:layout_width="90dp"
    android:layout_height="wrap_content"
    android:layout_gravity="center_vertical"
    android:textSize="18sp"
    android:text="Account:" />
    <EditText
    android:id="@+id/accountEdit"
    android:layout_width="0dp"
    android:layout_height="wrap_content"
    android:layout_weight="1"
    android:layout_gravity="center_vertical" />
    </LinearLayout>
    <LinearLayout
    android:orientation="horizontal"
    android:layout_width="match_parent"
    android:layout_height="60dp">
    <TextView
    android:layout_width="90dp"
    android:layout_height="wrap_content"
    android:layout_gravity="center_vertical"
    android:textSize="18sp"
    android:text="Password:" />
    <EditText
    android:id="@+id/passwordEdit"
    android:layout_width="0dp"
    android:layout_height="wrap_content"
    android:layout_weight="1"
    android:layout_gravity="center_vertical"
    android:inputType="textPassword" />
    </LinearLayout>
    <Button
    android:id="@+id/login"
    android:layout_width="200dp"
    android:layout_height="60dp"
    android:layout_gravity="center_horizontal"
    android:text="Login" />
</LinearLayout>
```

这里我们使用LinearLayout编写了一个登录布局,最外层是一个纵向的LinearLayout,里面包含了3行直接子元素。第一行是一个横向的LinearLayout,用于输入账号信息;第二行也是一个横向的LinearLayout,用于输入密码信息;第三行是一个登录按钮。

接下来修改LoginActivity中的代码,如下所示

```java
public class LoginActivity extends BaseActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_login);
        accountEdit = findViewById(R.id.accountEdit);
        passwordEdit = findViewById(R.id.passwordEdit);
        login = findViewById(R.id.login);
        login.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                String account = accountEdit.getText().toString();
                String password = passwordEdit.getText().toString();
                if (account.equals("root") && password.equals("123456")){
                    Intent intent = new Intent(LoginActivity.this,MainActivity.class);
                    startActivity(intent);
                    finish();
                } else {
                    Toast.makeText(LoginActivity.this,"account or password is invalid",Toast.LENGTH_SHORT).show();
                }
            }
        });
    }
}
```

这里模拟了一个非常简单的登录功能。首先将LoginActivity的继承结构改成继承自BaseActivity,然后在登录按钮的点击事件里对输入的账号和密码进行判断:如果登录成功就跳转到MainActivity,否则就提示用户账号或密码错误。
将MainActivity理解成是登录成功后进入的程序主界面,这里在主界面只加入强制下线功能。

修改activity_main.xml中的代码,如下所示:

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
android:orientation="vertical"
android:layout_width="match_parent"
android:layout_height="match_parent" >
    <Button
    android:id="@+id/forceOffline"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:text="Send force offline broadcast" />
</LinearLayout>
```

只有一个按钮用于触发强制下线功能。然后修改MainActivity中的代码,如下所示

```Java
public class MainActivity extends BaseActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        Button btn = findViewById(R.id.force_offline);
        btn.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent("com.nyh.broadcastbestpractice.FORCE_OFFLINE");
                sendBroadcast(intent);
            }
        });
    }
}
```

在按钮的点击事件里发送了一条广播,广播的值为com.example.broadcastbestpractice.FORCE_OFFLINE,这条广播就是用于通知程序强制用户下线的。也就是说,**强制用户下线的逻辑并不是写在MainActivity里的,而是应该写在接收这条广播的BroadcastReceiver里。**这样强制下线的功能就不会依附于任何界面了,不管是在程序的任何地方,只要发出这样一条广播,就可以完成强制下线的操作了。
那么接下来就创建一个BroadcastReceiver来接收这条强制下线广播。

问题,应该在哪里创建呢?

由于BroadcastReceiver中需要弹出一个对话框来阻塞用户的正常操作,但如果创建的是一个静态注册的BroadcastReceiver,是没有办法在onReceive()方法里弹出对话框这样的UI控件的,也不可能在每个Activity中都注册一个动态的BroadcastReceiver。

所以需要在BaseActivity中动态注册一个BroadcastReceiver就可以了,因为所有的Activity都继承自BaseActivity。
修改BaseActivity中的代码,如下所示:

```Java
public class BaseActivity extends AppCompatActivity {

    private ForceOfflineReceiver receiver;

    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        ActivityController.addActivity(this);
    }

    @Override
    protected void onResume() {
        super.onResume();
        IntentFilter intentFilter = new IntentFilter();
        intentFilter.addAction("com.nyh.broadcastbestpractice.FORCE_OFFLINE");
        receiver = new ForceOfflineReceiver();
        registerReceiver(receiver,intentFilter);
    }

    @Override
    protected void onPause() {
        super.onPause();
        if (receiver != null){
            unregisterReceiver(receiver);
            receiver = null;
        }
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        ActivityController.removeActivity(this);
    }
    class ForceOfflineReceiver extends BroadcastReceiver {

        @Override
        public void onReceive(Context context, Intent intent) {
            AlertDialog.Builder builder = new AlertDialog.Builder(context);
            builder.setTitle("Warning");
            builder.setMessage("you are forced to be offline. Please try to login again.");
            builder.setCancelable(false);
            builder.setPositiveButton("OK", new DialogInterface.OnClickListener() {
                @Override
                public void onClick(DialogInterface dialog, int which) {
                    ActivityController.finishAll(); //销毁所有Activity
                    Intent intent = new Intent(context, LoginActivity.class);
                    context.startActivity(intent);  //重新启动 LoginActivity
                }
            });
            builder.show();
        }
    }
}
```

先来看一下ForceOfflineReceiver中的代码,这次onReceive()方法里可不再是仅仅弹出一个Toast了,而是加入了较多的代码。

首先是使用AlertDialog.Builder构建一个对话框。注意,这里一定要调用setCancelable()方法将对话框设为不可取消,否则用户按一下Back键就可以关闭对话框继续使用程序了。然后使用setPositiveButton()方法给对话框注册确定按钮,当用户点击了“OK”按钮时,就调用ActivityCollector的finishAll()方法销毁所有Activity,并重新启动LoginActivity。

再来看一下是怎么注册ForceOfflineReceiver这个BroadcastReceiver的。可以看到,这里重写了onResume()和onPause()这两个生命周期方法,然后分别在这两个方法里注册和取消注册了ForceOfflineReceiver。

为什么要这样写?之前不都是在onCreate()和onDestroy()方法里注册和取消注册BroadcastReceiver的?

这是因为始终需要保证只有处于栈顶的Activity才能接收到这条强制下线广播,非栈顶的Activity不应该也没必要接收这条广播,所以写在onResume()和onPause()方法里就可以很好地解决这个问题,当一个Activity失去栈顶位置时就会自动取消BroadcastReceiver的注册。
这样的话,所有强制下线的逻辑就已经完成了,接下来我们还需要对AndroidManifest.xml文件进行修改,代码如下所示:

```xml
<activity
          android:name=".LoginActivity"
          android:exported="true" >
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>
</activity>
```

这里只需要对一处代码进行修改,就是将主Activity设置为LoginActivity,而不再是MainActivity。
