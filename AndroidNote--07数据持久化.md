## 7.1 持久化技术简介 

数据持久化就是指将那些内存中的瞬时数据保存到存储设备中，保证即使在手机或计算机关机的情况下，这些数据仍然不会丢失。保存在内存中的数据是处于瞬时状态的，而保存在存储设备中的数据是处于持久状态的。持久化技术提供了一种机制，可以让数据在瞬时状态和持久状态之间进行转换。 

Android中的数据持久化技术。

Android系统中主要提供了3种方式用于简单地实现数据持久化功能：

文件存储、 SharedPreferences存储以及数据库存储。 

## 7.2 文件存储 

文件存储是Android中最基本的数据存储方式，它不对存储的内容进行任何格式化处理，所有数据都是原封不动地保存到文件当中的，因而它比较适合存储一些简单的文本数据或二进制数据。如果想使用文件存储的方式来保存一些较为复杂的结构化数据，就需要定义一套自己的格式规范，方便之后将数据从文件中重新解析出来。 

首先看一下，Android中是如何通过文件来保存数据的。

### 7.2.1 将数据存储到文件中

**Context**类中提供了一个openFileOutput()方法，可以用于将数据存储到指定的文件中。

这个方法接收两个参数：

- 第一个参数是文件名，在文件创建的时候使用，注意这里指定的文件名不可以包含路径，因为所有的文件都默认存储到/data/data//files/目录 下；
- 第二个参数是文件的操作模式，主要有MODE_PRIVATE和MODE_APPEND两种模式可选，
  - 默认是MODE_PRIVATE，表示当指定相同文件名的时候，所写入的内容将会覆盖原文件中的内容
  - 而MODE_APPEND则表示如果该文件已存在，就往文件里面追加内容，不存在就创建新文件。

*其实文件的操作模式本来还有另外两种：MODE_WORLD_READABLE和 MODE_WORLD_WRITEABLE。这两种模式表示允许其他应用程序对我们程序中的文件进行读写 操作，不过由于这两种模式过于危险，很容易引起应用的安全漏洞，已在Android 4.2版本中被废弃。* 

openFileOutput()方法返回的是一个FileOutputStream对象，得到这个对象之后就可以 使用Java流的方式将数据写入文件中了。以下是一段简单的代码示例，展示了如何将一段文本 内容保存到文件中：

```java
public void save(){
        String data =  "Data to save";
        FileOutputStream out = null;
        BufferedWriter writer = null;
        try {
            out = openFileOutput("data", Context.MODE_PRIVATE);
            writer = new BufferedWriter(new OutputStreamWriter(out));
            writer.write(data);
        } catch (IOException e){
            e.printStackTrace();
        } finally {
            try {
                if (writer != null){
                    writer.close();
                }
            } catch (IOException e){
                e.printStackTrace();
            }
        }
    }
```

通过openFileOutput()方法能够得到一个FileOutputStream对象，然后借助它构建出一个OutputStreamWriter对 象，接着再使用OutputStreamWriter构建出一个BufferedWriter对象，这样你就可以通过BufferedWriter将文本内容写入文件中了。

创建FilePersistenceTest项目，修改activity_main.xml中的代码

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent" >
    <EditText
        android:id="@+id/editText"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Type something here"
        />
</LinearLayout>
```

只加入了一个EditText用于输入文本内容。此时输入内容之后再按下Back键，此时输入的内容消失，因为他只是瞬时数据，在活动被销毁后就会被回收。修改MainActivity中的代码

```java
package com.nyh.filepersistencetest;

import androidx.appcompat.app.AppCompatActivity;

import android.content.Context;
import android.os.Bundle;
import android.widget.EditText;

import java.io.BufferedWriter;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.OutputStreamWriter;

public class MainActivity extends AppCompatActivity {

    private EditText editText;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        editText = findViewById(R.id.editText);
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        String inputText = editText.getText().toString();
        save(inputText);
    }

    private void save(String inputText) {
        FileOutputStream out = null;
        BufferedWriter writer = null;
        try {
            out = openFileOutput("data", Context.MODE_PRIVATE);
            writer = new BufferedWriter(new OutputStreamWriter(out));
            writer.write(inputText);
        } catch (IOException e){
            e.printStackTrace();
        } finally {
            try {
                if (writer != null){
                    writer.close();
                }
            } catch (IOException e){
                e.printStackTrace();
            }
        }
    }
}
```

在onCreate方法中获取EditText的实例；然后重写onDestroy方法，在onCreate方法中获取EditText中输入的内容，调用save方法把内容存储到文件中，文件名为data。

点击AS右下角Android Device Monitor 打开工具，在/data/data/com.nyh.xxx/files/目录，可以看到生成的data文件，双击打开可以看到EditText中输入的内容。

![image-20220516181034183](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220516181034183.png)

### 7.2.2 从文件中读取数据

类似于将数据存储到文件中，Context类中还提供了一个openFileInput()方法，用于从文件中读取数据。

这个方法只接收一个参数，即要读取的文件名，然后系统会自动到/data/data/package name/files/目录下加载这个文件，并返回一个FileInputStream对象，得到这个对象之后，再通过流的方式就可以将数据读取出来了。

```java
public String load(){
    FileInputStream in = null;
    BufferedReader reader = null;
    StringBuilder content = new StringBuilder();
    try {
        in = openFileInput("data");
        reader = new BufferedReader(new InputStreamReader(in));
        String line = "";
        while ((line = reader.readLine()) != null ){
            content.append(line);
        }
    } catch (IOException e){
        e.printStackTrace();
    } finally {
        if (reader != null) {
            try {
                reader.close();
            } catch (IOException e){
                e.printStackTrace();
            }
        }
    }
    return content.toString();
}
```

首先通过openFileInput()方法获取到了一个FileInputStream对象，然后借助它又构建出了一个InputStreamReader对象，接着再使用InputStreamReader构建出一个BufferedReader对象，这样我们就可以通过BufferedReader进行一行行地读取，把文件中所有的文本内容全部读取出来，并存放在一个StringBuilder对象中，最后将读取到的内容返回就可以了。

通过该方法使得重新启动程序时EditText中能够保留我们上次输入的内容。

修改MainActivity中的代码，如下所示：

```java
public class MainActivity extends AppCompatActivity {

    private EditText editText;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        editText = findViewById(R.id.editText);
        String inputText = load();
        if (!TextUtils.isEmpty(inputText)){
            editText.setText(inputText);
            editText.setSelection(inputText.length());
            Toast.makeText(this,"Restoring succeed",Toast.LENGTH_LONG).show();
        }

    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        String inputText = editText.getText().toString();
        save(inputText);
    }

    private void save(String inputText) {
        FileOutputStream out = null;
        BufferedWriter writer = null;
        try {
            out = openFileOutput("data", Context.MODE_PRIVATE);
            writer = new BufferedWriter(new OutputStreamWriter(out));
            writer.write(inputText);
        } catch (IOException e){
            e.printStackTrace();
        } finally {
            try {
                if (writer != null){
                    writer.close();
                }
            } catch (IOException e){
                e.printStackTrace();
            }
        }
    }
    public String load(){
        FileInputStream in = null;
        BufferedReader reader = null;
        StringBuilder content = new StringBuilder();
        try {
            in = openFileInput("data");
            reader = new BufferedReader(new InputStreamReader(in));
            String line = "";
            while ((line = reader.readLine()) != null ){
                content.append(line);
            }
        } catch (IOException e){
            e.printStackTrace();
        } finally {
            if (reader != null) {
                try {
                    reader.close();
                } catch (IOException e){
                    e.printStackTrace();
                }
            }
        }
        return content.toString();
    }
}
```

在onCreate()方法中调用load()方法来读取文件中存储的文本内容，如果读到的内容不为null,就调用EditText的setText()方法将内容填充到EditText里，并调用setSelection()方法将输入光标移动到文本的末尾位置以便于继续输入，然后弹出一句还原成功的提示。

上述代码在对字符串进行非空判断的时候使用了TextUtils.isEmpty()方法，这个方法可以一次性进行两种空值的判断。当传入的字符串等于null或者等于空字符串的时候，这个方法都会返回true,从而使得我们不需要先单独判断这两种空值再使用逻辑运算符连接起来了。

重新启动程序刚才保存的字符串就会被填充到EditText中。

文件存储的方式并不适合用于保存一些较为复杂的文本数据。

下面是Android中另一种数据持久化的方式，它比文件存储更加简单易用，而且可以很方便地对某一指定的数据进行读写操作。

## 7.3 SharedPreferences存储 

不同于文件的存储方式，SharedPreferences是使用键值对的方式来存储数据的。

也就是说， 当保存一条数据的时候，需要给这条数据提供一个对应的键，这样在读取数据的时候就可以通过这个键把相应的值取出来。而且SharedPreferences还支持多种不同的数据类型存储，如果存储的数据类型是整型，那么读取出来的数据也是整型的；如果存储的数据是一个字符串，那么读取出来的数据仍然是字符串。 使用SharedPreferences进行数据持久化要比使用文件方便很多

### 7.3.1 将数据存储到SharedPreferences中

1. Context类中的getSharedPreferences()方法 
   - 此方法接收两个参数：
     - 第一个参数用于指定SharedPreferences文件的名称，如果指定的文件不存在则会创建一个，SharedPreferences文件都是存放在/data/data/packagename/shared_prefs/目录下的；
     - 第二个参数用于指定操作模式，目前只有默认的 MODE_PRIVATE这一种模式可选，它和直接传入0的效果是相同的，表示只有当前的应用程序才可以对这个SharedPreferences文件进行读写。
       - *其他几种操作模式均已被废弃， MODE_WORLD_READABLE和MODE_WORLD_WRITEABLE这两种模式是在Android 4.2版本 中被废弃的，MODE_MULTI_PROCESS模式是在Android 6.0版本中被废弃的*。 
2. Activity类中的getPreferences()方法 
   - 这个方法和Context中的getSharedPreferences()方法很相似，不过它只接收一个操作模式参数，因为使用这个方法时会自动将当前Activity的类名作为 SharedPreferences的文件名。 
   - 得到了SharedPreferences对象之后，就可以开始向SharedPreferences文件中存储数据了，主要可以分为3步实现。
     1. 调用SharedPreferences对象的edit()方法获取一个 SharedPreferences.Editor对象。
     2. 向SharedPreferences.Editor对象中添加数据，比如添加一个布尔型数据就使用 putBoolean()方法，添加一个字符串则使用putString()方法，以此类推。
     3. 调用apply()方法将添加的数据提交，从而完成数据存储操作。 

新建一个SharedPreferencesTest项目，然后修改activity_main.xml

加一个按钮

```java
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        Button btn = findViewById(R.id.saveButton);
        btn.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                //通过getSharedPreferences()方法指定SharedPreferences的文件名为data
                SharedPreferences.Editor editor = getSharedPreferences("data",
                        MODE_PRIVATE).edit();
                //添加了3条不同类型的数据
                editor.putString("name","jack");
                editor.putInt("age",28);
                editor.putBoolean("married",false);
                //提交
                editor.apply();
            }
        });
    }
}
```

完成了数据存储的操作。

![image-20220516190935486](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220516190935486.png)

可以看到 SharedPreferences文件是使用XML格式来对数据进行管理的。

### 7.3.2 从SharedPreferences中读取数据

使用SharedPreferences存储数据是非常简单的，从SharedPreferences文件中读取数据会更加简单。SharedPreferences对象中提供了一系列的get方法，用于读取存储的数据，每种get方法都对应了 SharedPreferences.Editor中的一种put方法，比如读取一个布尔型数据就使用 getBoolean()方法，读取一个字符串就使用getString()方法。

这些get方法都接收两个参数：

第一个参数是键，传入存储数据时使用的键就可以得到相应的值了；

第二个参数是默认值，即表示当传入的键找不到对应的值时会以什么样的默认值进行返回。 

仍然是在SharedPreferencesTest项目的基础上继续开 发，修改activity_main.xml中的代码，如下所示：

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical" >
    <Button
        android:id="@+id/saveButton"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Save Data"
        />
    <Button
        android:id="@+id/restoreButton"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Restore Data"
        />
</LinearLayout>
```

修改MainActivity中的代码

```java
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        Button btn = findViewById(R.id.saveButton);
        btn.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                SharedPreferences.Editor editor = getSharedPreferences("data",
                        MODE_PRIVATE).edit();
                editor.putString("name","jack");
                editor.putInt("age",28);
                editor.putBoolean("married",false);
                editor.apply();
            }
        });

        Button btn2 = findViewById(R.id.restoreButton);
        btn2.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                //通过getSharedPreferences()得到SharedPreferences对象，分别调用它的getxxx方法
                SharedPreferences pref = getSharedPreferences("data",MODE_PRIVATE);
                String name = pref.getString("name", "");
                int age = pref.getInt("age",0);
                boolean married = pref.getBoolean("married", false);
                Log.d("999555",name);
                Log.d("999555",age + "");
                Log.d("999555",married + "");
            }
        });
    }
}
```

### 7.3.3 实现记住密码功能

先打开 BroadcastBestPractice项目，编辑一下登录界面的布局。修改activity_login.xml中的代码

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
 android:orientation="vertical"
 android:layout_width="match_parent"
 android:layout_height="match_parent">
 ...
 <LinearLayout
 android:orientation="horizontal"
 android:layout_width="match_parent"
 android:layout_height="wrap_content">
 <CheckBox
 android:id="@+id/rememberPass"
 android:layout_width="wrap_content"
 android:layout_height="wrap_content" />
 <TextView
 android:layout_width="wrap_content"
 android:layout_height="wrap_content"
 android:textSize="18sp"
 android:text="Remember password" />
 </LinearLayout>
 <Button
 android:id="@+id/login"
 android:layout_width="match_parent"
 android:layout_height="60dp"
 android:text="Login" />
</LinearLayout>
```

这里使用了一个新控件：CheckBox。这是一个复选框控件，用户可以通过点击的方式进行选中和取消，使用这个控件来表示用户是否需要记住密码。

然后修改LoginActivity中的代码

```java
public class LoginActivity extends BaseActivity {
    private SharedPreferences pref;
    private SharedPreferences.Editor editor;

    private EditText accountEdit;
    private EditText passwordEdit;
    private Button login;

    private CheckBox rememberPass;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_login);
        //获取到SharedPreferences对象
        pref = PreferenceManager.getDefaultSharedPreferences(this);

        accountEdit = findViewById(R.id.accountEdit);
        passwordEdit = findViewById(R.id.passwordEdit);

        rememberPass = findViewById(R.id.rememberPass);

        login = findViewById(R.id.login);
        //获取 remember_password 对应的boolean值  一开始默认值为false
        boolean isRemember = pref.getBoolean("remember_password",false);
        if (isRemember) {
            // 将账号和密码都设置到文本中
            String account = pref.getString("account", "");
            String password = pref.getString("password", "");
            accountEdit.setText(account);
            passwordEdit.setText(password);
            rememberPass.setChecked(true);
        }

        login.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                String account = accountEdit.getText().toString();
                String password = passwordEdit.getText().toString();
                if (account.equals("root") && password.equals("123456")){
                    //登录成功之后
                    editor = pref.edit();  //获取到editor用来putxxx
                    if (rememberPass.isChecked()) { //检查复选框是否被选中
                        editor.putBoolean("remember_password",true);
                        //把account 和 password都存入到SharedPreferences文件当中
                        editor.putString("account",account);
                        editor.putString("password",password);
                    } else {
                      //没有选中 就把SharedPreferences文件当中的数据全部清除掉
                      editor.clear();
                    }
                    //提交
                    editor.apply();

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

当用户选中了记住密码复选框，并成功登录一次之后，remember_password键对应的值就是true了，这个时候如果重新启动登录界面，就会从SharedPreferences文件中将保存的账号和密码都读取出来，并填充到文本输入框中，然后把记住密码复选框选中，这样就完成记住密码的功能了。

## 7.4 SQLite数据库存储

Android系统是内置了数据库。SQLite是一款轻量级的关系型数据库，它的运算速度非常快，占用资源很少，通常只需要几百KB的内存就足够了，因而特别适合在移动设备上使用。

SQLite不仅支持标准的SQL语法，还遵循了数据库的ACID事务，所以只要你以前使用过其他的关系型数据库，就可以很快地上手SQLite。而SQLite又比一般的数据库要简单得多，它甚至不用设置用户名和密码就可以使用。

Android正是把这个功能极为强大的数据库嵌入到了系统当中，使得本地持久化的功能有了一次质的飞跃。 前面所使用的文件存储和SharedPreferences存储只适用于保存一些简单的数据和键值对，当需要存储大量复杂的关系型数据的时候，以上两种存储方式很难应付得了。比如手机的短信程序中可能会有很多个会话，每个会话中又包含了很多条信息内容， 并且大部分会话还可能各自对应了通讯录中的某个联系人。很难想象如何用文件或者 SharedPreferences来存储这些数据量大、结构性复杂的数据？但是使用数据库就可以做得到。

Android中的SQLite数据库到底是如何使用的。