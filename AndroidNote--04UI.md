# 4 UI开发

## 4.1 该如何编写程序界面

写XML的好处是,不仅能够了解界面背后的实现原理,而且编写出来的界面还可以具备很好的屏幕适配性。
全新的界面布局:ConstraintLayout。ConstraintLayout不是非常适合通过编写XML的方式来开发界面,而是更加适合在可视化编辑器中使用拖放控件的方式来进行操作,Android Studio中也提供了非常完备的可视化编辑器。

## 4.2 常用控件的使用方法

### 4.2.1 TextView

activity_main.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <TextView
        android:id="@+id/textView"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:gravity="center"
        android:textSize="24sp"
        android:textColor="#00ff00"
        android:text="This is TextView"/>
</LinearLayout>
```

``android:id`` 给当前控件定义了一个唯一标识符。

``android:layout_width``和``android:layout_height``指定了控件的宽度和高度。Android中所有的控件都具有这两个属性,可选值有3种:match_parent、wrap_content和固定值。

match_parent表示让当前控件的大小和父布局的大小一样,也就是由父布局来决定当前控件的大小。wrap_content表示让当前控件的大小能够刚好包含住里面的内容,也就是由控件内容决定当前控件的大小。          固定值表示表示给控件指定一个固定的尺寸,单位一般用dp,这是一种屏幕密度无关的尺寸单位,可以保证在不同分辨率的手机上显示效果尽可能地一致,如50 dp就是一个有效的固定值。

``android:gravity``来指定文字的对齐方式,可选值有top、bottom、start、end、center等,可以用“|”来同时指定多个值,"center"效果等同于"center_vertical|center_horizontal",表示文字在垂直和水平方向都居中对齐。

``android:textColor``属性可以指定文字的颜色

``android:textSize``属性可以指定文字的大小。文字大小要使用sp作为单位,这样当用户在系统中修改了文字显示尺寸时,应用程序中的文字大小也会跟着变化。

### 4.2.2 Button

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    ...
    <Button
        android:id="@+id/button"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="button"
        android:textAllCaps="false" />
</LinearLayout>
```

``android:textAllCaps="false"``关闭默认将按钮上的英文字母转换成大写

在MainActivity中给button的点击事件注册一个监听器

```java
public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        Button btn = findViewById(R.id.button);
        btn.setOnClickListener(new View.OnClickListener(){
            @Override
            public void onClick(View view) {
                //点击按钮之后的操作
            }
        });
    }
}
```

第二种方式

```java
public class MainActivity extends AppCompatActivity implements View.OnClickListener{
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        Button btn = findViewById(R.id.button);
        btn.setOnClickListener(this);
    }

    @Override
    public void onClick(View view) {
        switch (view.getId()) {
            case R.id.button:
                // 点击按钮之后的操作
                break;
            default:
        }
    }
}
```

### 4.2.3 EditText

EditText是程序用于和用户进行交互的另一个重要控件,它允许用户在控件里输入和编辑内容,并可以在程序中对这些内容进行处理。

activity_main.xml  加入

```xml
<!--android:hint属性指定了一段提示性的文本  没输入文字之前显示的内容-->
<!--android:maxLines属性制定EditText最多显示两行-->
<EditText
        android:id="@+id/editText"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="写点什么吧"
        android:maxLines="2"/>
```

通过点击button获取到输入框中的内容

```Java
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);
    Button btn = findViewById(R.id.button);
    EditText editText = findViewById(R.id.editText);
    ImageView imageView = findViewById(R.id.imageView);
    btn.setOnClickListener(new View.OnClickListener() {
        @Override
        public void onClick(View view) {
            String inputText = editText.getText() + "";
            Toast.makeText(MainActivity.this, inputText, Toast.LENGTH_SHORT).show();
        }
    });
}
```

### 4.2.4 ImageView

ImageView是用于在界面上展示图片的一个控件。图片通常是放在以drawable开头的目录下的,并且要带上具体的分辨率。现在最主流的手机屏幕分辨率大多是xxhdpi的,在res目录下新建一个drawable-xxhdpi目录,然后将事先准备好的两张图片img_1.png和img_2.png复制到该目录当中。

activity_main.xml

```xml
<ImageView
        android:id="@+id/imageView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:src="@drawable/img02"/>
```

将ImageView的宽和高都设定为wrap_content,这样就保证了不管图片的尺寸是多少,都可以完整地展示出来。

点击按钮切换图片

```java
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);
    Button btn2 = findViewById(R.id.button2);
    ImageView imageView = findViewById(R.id.imageView);
    btn2.setOnClickListener(new View.OnClickListener() {
        @Override
        public void onClick(View view) {
            imageView.setImageResource(R.drawable.img01);
        }
    });
}
```

### 4.2.5 ProgressBar

ProgressBar用于在界面上显示一个进度条,表示我们的程序正在加载一些数据。

activity_main.xml中加入

```xml
<ProgressBar
    android:id="@+id/progressBar"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    />
```

Android控件的可见属性。所有的Android控件都具有这个属性,可以通过android:visibility进行指定,可选值有3种:visible、invisible和gone。

visible表示控件是可见的,这个值是默认值,不指定android:visibility时,控件都是可见的。
invisible表示控件不可见,但是它仍然占据着原来的位置和大小,可以理解成控件变成透明状态了。
gone则表示控件不仅不可见,而且不再占用任何屏幕空间。

通过代码来设置控件的可见性,使用的是setVisibility()方法,允许传入View.VISIBLE、View.INVISIBLE和View.GONE这3种值。

点击一下按钮让进度条消失,再点击一下按钮让进度条出现。

```java
btn3.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View view) {
        //通过getVisibility()方法来判断ProgressBar是否可见
        //可见就将ProgressBar隐藏
        if (progressBar.getVisibility() == View.GONE){
            progressBar.setVisibility(View.VISIBLE);
        } else {
        //如果不可见就将ProgressBar显示出来    
            progressBar.setVisibility(View.GONE);
        }
    }
});
```

给ProgressBar指定不同的样式，指定成水平进度条

修改activity_main.xml，在ProgressBar中添加

``style="?android:attr/progressBarStyleHorizontal"``
``android:max="100"``

``android:max``属性给进度条设置一个最大值,然后在代码中可以动态地更改进度条的进度

```Java
btn3.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View view) {
        int progress = progressBar.getProgress();
        progress = progress + 10;
        progressBar.setProgress(progress);
    }
});
```

### 4.2.6 AlertDialog

``AlertDialog``可以在当前界面弹出一个对话框,这个对话框是置顶于所有界面元素之上的,能够屏蔽其他控件的交互能力,因此``AlertDialog``一般用于提示一些非常重要的内容或者警告信息。

比如为了防止用户误删重要内容,在删除前弹出一个确认对话框。

```java
btn4.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View view) {
        AlertDialog.Builder dialog = new AlertDialog.Builder(MainActivity.this);
        dialog.setTitle("this is dialog");
        dialog.setMessage("something important");
        dialog.setCancelable(false);
        dialog.setPositiveButton("OK", new DialogInterface.OnClickListener() {
            @Override
            public void onClick(DialogInterface dialog, int which) {
                Toast.makeText(MainActivity.this,"okok",Toast.LENGTH_SHORT).show();
            }
        });
        dialog.setNegativeButton("cancle", new DialogInterface.OnClickListener() {
            @Override
            public void onClick(DialogInterface dialog, int which) {
                Toast.makeText(MainActivity.this,"cancle",Toast.LENGTH_SHORT).show();
            }
        });
        dialog.show();
    }
});
```

首先通过AlertDialog.Builder创建一个AlertDialog的实例，然后可以为这个对话框设置标题、内容、可否取消等属性，接下来调用setPositiveButton()方法为对话框设置确定按钮的点击事件，调用setNegativeButton()方法设置取消按钮的点击事件，最后调用show()方法将对话框显示出来。

### 4.2.7 ProgressDialog

ProgressDialog和AlertDialog有点类似，都可以在界面上弹出一个对话框，都能够屏蔽掉其他控件的交互能力。不同的是，ProgressDialog会在对话框中显示一个进度条，一般用于表示当前操作比较耗时，让用户耐心地等待。它的用法和AlertDialog也比较相似，修改MainActivity中的代码

```Java
btn4.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View view) {
        ProgressDialog progressDialog = new ProgressDialog(MainActivity.this);
        progressDialog.setTitle("this is progressDialog");
        progressDialog.setMessage("loading");
        //可以通过back键取消
        progressDialog.setCancelable(true);
        progressDialog.show();
    }
});
```

```
progressDialog.dismiss();关闭对话框
```

## 4.3 详解4种基本布局

一个丰富的界面是由很多个控件组成的,需要借助布局来实现。布局是一种可用于放置很多控件的容器,它可以按照一定的规律调整内部控件的位置,从而编写出精美的界面。布局的内部除了放置控件外,也可以放置布局,通过多层布局的嵌套,就能够完成一些比较复杂的界面实现。

![布局和控件的关系](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220413172851247.png)



新建UILayoutTest项目。

### 4.3.1 线性布局



## 4.4 自定义控件



## 4.5 ListView

## 4.6 RecycleView

## 4.7 编写界面的最佳实践