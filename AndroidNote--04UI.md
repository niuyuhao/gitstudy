# 4 UI开发

## 4.1 该如何编写程序界面

写XML的好处是,不仅能够了解界面背后的实现原理,而且编写出来的界面还可以具备很好的屏幕适配性。
*全新的界面布局:ConstraintLayout。ConstraintLayout不是非常适合通过编写XML的方式来开发界面,而是更加适合在可视化编辑器中使用拖放控件的方式来进行操作,Android Studio中也提供了非常完备的可视化编辑器。*

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
        if (progressBar.getVisibility() == View.GONE){
        	//如果不可见就将ProgressBar显示出来
            progressBar.setVisibility(View.VISIBLE);
        } else {
            //可见就将ProgressBar隐藏
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
        dialog.setCancelable(false); //可否取消
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

这个布局会将它所包含的控件在线性方向上依次排列。

通过``android:orientation``属性指定了排列方向是vertical  ----->  垂直排列；horizontal  -----> 水平排列

在activity_main.xml中 添加三个按钮，将长和宽都设置成wrap_content，指定vertical为排列方向。

改为horizontal 则为水平排列

注意,如果LinearLayout的排列方向是horizontal,内部的控件就绝对不能将宽度指定为match_parent,否则,单独一个控件就会将整个水平方向占满,其他的控件就没有可放置的位置。同样的道理,如果LinearLayout的排列方向是vertical,内部的控件就不能将高度指定为match_parent。
``android:layout_gravity``属性：用于指定控件在布局中的对齐方式。android:layout_gravity的可选值和android:gravity差不多,但是需要注意

当LinearLayout的排列方向是horizontal时,只有垂直方向上的对齐方式才会生效。因为此时水平方向上的长度是不固定的,每添加一个控件,水平方向上的长度都会改变,因而无法指定该方向上的对齐方式。

同样的道理,当LinearLayout的排列方向是vertical时,只有水平方向上的对齐方式才会生效。

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
android:orientation="horizontal"
android:layout_width="match_parent"
android:layout_height="match_parent">
    <Button
    android:id="@+id/button1"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:layout_gravity="top"
    android:text="Button 1" />
    <Button
    android:id="@+id/button2"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:layout_gravity="center_vertical"
    android:text="Button 2" />
    <Button
    android:id="@+id/button3"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:layout_gravity="bottom"
    android:text="Button 3" />
</LinearLayout>
```

指定垂直方向上的排列方向,将第一个Button的对齐方式指定为top,第二个Button的对齐方式指定为
center_vertical,第三个Button的对齐方式指定为bottom。

<img src="https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220414092813234.png" alt="指定layout_gravity的效果" style="zoom:50%;" />

``android:layout_weight``属性：允许我们使用比例的方式来指定控件的大小。作用在手机屏幕的适配性方面。

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:orientation="horizontal"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">
    <EditText
        android:id="@+id/input_message"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_weight="1"
        android:hint="写点什么吧" />
    <Button
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_weight="1"
        android:text="send" />
</LinearLayout>
```

由于使用了``android:layout_weight``属性,此时控件的宽度就不应该再由android:layout_width来决定了,这里指定成0dp是一种比较规范的写法。1代表这两个评分屏幕的宽度。
让EditText占据屏幕宽度的3/5,Button占据屏幕宽度的2/5,将EditText的layout_ weight改成3,Button的layout_weight改成2就可以了。

通过指定**部分控件的layout_weight值**来实现更好的效果。修改activity_main.xml中的代码

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:orientation="horizontal"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">
    <!--EditText则会占满屏幕所有的剩余空间-->
    <EditText
        android:id="@+id/input_message"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_weight="1"
        android:hint="写点什么吧" />
    <!--Button的宽度仍然按照wrap_content来计算-->
    <Button
        android:id="@+id/send"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="send" />
</LinearLayout>
```

### 4.3.2 相对布局

RelativeLayout通过相对定位的方式让控件出现在布局的任何位置。

1. 相对于父布局进行定位

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">
    <Button
        android:id="@+id/button1"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentLeft="true"
        android:layout_alignParentTop="true"
        android:text="button 1" />
    <Button
        android:id="@+id/button2"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentRight="true"
        android:layout_alignParentTop="true"
        android:text="button 2" />
    <Button
        android:id="@+id/button3"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_centerInParent="true"
        android:text="button 3" />
    <Button
        android:id="@+id/button4"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentBottom="true"
        android:layout_alignParentLeft="true"
        android:text="button 4" />
    <Button
        android:id="@+id/button5"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentBottom="true"
        android:layout_alignParentRight="true"
        android:text="button 5" />
</RelativeLayout>
```

2. 相对于控件进行定位

   - ```xml
     <?xml version="1.0" encoding="utf-8"?>
     <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
         xmlns:tools="http://schemas.android.com/tools"
         android:layout_width="match_parent"
         android:layout_height="match_parent"
         tools:context=".MainActivity">
         <Button
             android:id="@+id/button3"
             android:layout_width="wrap_content"
             android:layout_height="wrap_content"
             android:layout_centerInParent="true"
             android:text="button 3" />
         <Button
             android:id="@+id/button1"
             android:layout_width="wrap_content"
             android:layout_height="wrap_content"
             android:layout_above="@+id/button3"
             android:layout_toLeftOf="@id/button3"
             android:text="button 1" />
         <Button
             android:id="@+id/button2"
             android:layout_width="wrap_content"
             android:layout_height="wrap_content"
             android:layout_above="@id/button3"
             android:layout_toRightOf="@id/button3"
             android:text="button 2" />
     
         <Button
             android:id="@+id/button4"
             android:layout_width="wrap_content"
             android:layout_height="wrap_content"
             android:layout_below="@id/button3"
             android:layout_toLeftOf="@id/button3"
             android:text="button 4" />
         <Button
             android:id="@+id/button5"
             android:layout_width="wrap_content"
             android:layout_height="wrap_content"
             android:layout_below="@id/button3"
             android:layout_toRightOf="@id/button3"
             android:text="button 5" />
     </RelativeLayout>
     ```

   - <img src="https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220414102940889.png" alt="image-20220414102940889" style="zoom:50%;" />

   - 注意：当一个控件去引用另一个控件的id时,该控件一定要定义在引用控件的后面,不然会出现找不到id的情况。

3. RelativeLayout中还有另外一组相对于控件进行定位的属性,android:layout_alignLeft表示让一个控件的左边缘和另一个控件的左边缘对齐,android:layout_alignRight表示让一个控件的右边缘和另一个控件的右边缘对齐。android:layout_alignTop和android:layout_alignBottom同理

### 4.3.3 帧布局

这种布局所有的控件都会默认摆放在布局的左上角。

```xml
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">
    <TextView
        android:id="@+id/text_view"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="This is TextView"/>
    <ImageView
        android:id="@+id/image_view"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:src="@mipmap/ic_launcher"/>
</FrameLayout>
```

<img src="https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220414104736476.png" alt="FrameLayout运行效果" style="zoom: 50%;" />

使用``layout_gravity``指定对齐方式

<img src="https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220414104958321.png" alt="image-20220414104958321" style="zoom:50%;" />

### 4.3.4 百分比布局（build.gradle这个文件内容待学习）



## 4.4 自定义控件

**所有控件都是直接或间接继承自View,所用的所有布局都是直接或间接继承自ViewGroup的**。

View是Android中最基本的一种UI组件,它可以在屏幕上绘制一块矩形区域,并能响应这块区域的各种事件,我们使用的各种控件其实就是在View的基础上又添加了各自特有的功能。而ViewGroup则是一种特殊的View,它可以包含很多子View和子ViewGroup,是一个用于放置控件和布局的容器。

<img src="https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220414163546727.png" alt="常用控件的布局的继承结构"  />

可以利用上面的继承机构创建自定义控件。新建UICustomViews  project

### 4.4.1 引入布局

新建一个布局title.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:background="@color/black">
    <!--android:background用于为布局或控件指定一个背景,可以使用颜色或图片来进行填充-->
    <!--android:layout_margin属性可以指定控件在上下左右方向上的间距-->
    <!--android:layout_marginLeft或android:layout_marginTop单独指定控件在某个方向上的间距-->
    <Button
        android:id="@+id/titleBack"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center"
        android:layout_margin="5dp"
        android:text="Back"
        android:textColor="#fff" />
    <TextView
        android:id="@+id/titleText"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_gravity="center"
        android:layout_weight="1"
        android:gravity="center"
        android:text="Title Text"
        android:textColor="#fff"
        android:textSize="24sp" />
    <Button
        android:id="@+id/titleEdit"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center"
        android:layout_margin="5dp"
        android:text="Edit"
        android:textColor="#fff" />
</LinearLayout>
```

在activity_main.xml中的代码

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">
	<!--引入标题栏布局-->
    <include layout="@layout/title" />
</LinearLayout>
```

在MainActivity中将系统自带的标题栏隐藏掉

```java
public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        //获得ActionBar的实例
        ActionBar actionBar = getSupportActionBar();
        if (actionBar != null){
            //隐藏标题栏
            actionBar.hide();
        }
    }
}
```

### 4.4.2 创建自定义控件

通过自定义控件，解决：每一个活动中都需要重新注册一遍按钮的点击事件。

新建TitleLayout

```Java
//继承LinearLayout，成为自定义的标题栏控件
public class TitleLayout extends LinearLayout {
    public TitleLayout(Context context, @Nullable AttributeSet attrs) {
        super(context, attrs);
        //通过LayoutInflater.from()方法可以构建出一个LayoutInflater对象
        //调用inflate()方法动态加载一个布局文件
        //参数1  要加载的布局文件的id  参数2 给加载好的布局再添加一个父布局
        LayoutInflater.from(context).inflate(R.layout.title,this);
        Button titleBack = findViewById(R.id.titleBack);
        Button titleEdit = findViewById(R.id.titleEdit);
        titleBack.setOnClickListener(new OnClickListener() {
            @Override
            public void onClick(View view) {
                ((Activity)getContext()).finish();
            }
        });
        titleEdit.setOnClickListener(new OnClickListener() {
            @Override
            public void onClick(View view) {
                Toast.makeText(getContext(),"click edit btn",Toast.LENGTH_SHORT).show();
            }
        });
    }
}
```

创建好之后，在布局文件中添加这个自定义控件

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">
	<!--指明控件的完整类名，包名不可以省略-->
    <com.nyh.uicustomviews.TitleLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content" />
</LinearLayout>
```

注册点击事件

这样在一个布局中引入TitleLayout时，按钮的点击事件就已经实现好了。

## 4.5 ListView

ListView：在屏幕上显示大量数据。

### 4.5.1 ListView的简单用法

新建ListViewTest，创建好Activity，在activity_main.xml中加入ListView

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <ListView
        android:id="@+id/listView"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />
</LinearLayout>
```

MainActivity

```Java
public class MainActivity extends AppCompatActivity {
    //使用一个string数组，模拟一些数据
    private String[] data = {"宋家庄","石榴庄","大红门","角门东","角门西","草桥","牛街","积水潭","牡丹园", "美团单车","志新桥","志新西路","打卡"};
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        ArrayAdapter<String> adapter = new ArrayAdapter<String>(MainActivity.this, android.R.layout.simple_list_item_1,data);
        ListView listView = findViewById(R.id.listView);
        listView.setAdapter(adapter);
    }
}
```

集合中的数据是无法直接传递给ListView的,需要借助适配器来完成。Android中提供了很多适配器的实现类,本次使用ArrayAdapter。它可以通过泛型来指定要适配的数据类型,然后在构造函数中把要适配的数据传入。ArrayAdapter有多个构造函数的重载,根据实际情况选择最合适的一种。由于这里提供的数据都是字符串,因此将ArrayAdapter的泛型指定为String,然后在ArrayAdapter的构造函数中依次传入Activity的实例、ListView子项布局的id,以及数据源。
此处``android.R.layout.simple_list_item_1``作为ListView子项布局的id,这是一个Android内置的布局文件,里面只有一个TextView,可用于简单地显示一段文本。这样适配器对象就构建好了。
最后,调用ListView的setAdapter()方法,将构建好的适配器对象传递进去,ListView和数据之间的关联就建立完成了。

<img src="https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220415102837066.png" alt="image-20220415102837066" style="zoom:50%;" />

### 4.5.2 定制ListView的界面

让名称旁边显示一个图片。

1. 定义一个地铁类，作为ListView适配器的适配类型。

   - ```java
     public class Subwaystation {
         private String name;
         private int imageId;  //对应图片id
         public Subwaystation(String name,int imageId){
             this.name = name;
             this.imageId = imageId;
         }
     
         public String getName() {
             return name;
         }
     
         public int getImageId() {
             return imageId;
         }
     }
     ```

   - 

2. 给ListView的子项指定一个自定义的布局，在layout下新建station_item.xml

   - ```xml
     <?xml version="1.0" encoding="utf-8"?>
     <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
         android:layout_width="match_parent"
         android:layout_height="wrap_content">
         <!--显示图-->
         <ImageView
             android:id="@+id/stationImage"
             android:layout_width="wrap_content"
             android:layout_height="wrap_content" />
         <!--显示名称-->
         <TextView
             android:id="@+id/stationName"
             android:layout_width="wrap_content"
             android:layout_height="wrap_content"
             android:layout_gravity="center_vertical"
             android:layout_marginLeft="10dp"/>
     </LinearLayout>
     ```

3. 新建类StationAdapter，这个适配器继承自ArrayAdapter，将泛型指定为SubwayStation，创建一个自定义的适配器。

   - ```java
     public class StationAdapter extends ArrayAdapter<Subwaystation> {
         private int resourceId;
         //将context，ListView子项布局的id和数据传递进来
         public StationAdapter(@NonNull Context context, int textViewResourceId, @NonNull List<Subwaystation> objects) {
             super(context, textViewResourceId, objects);
             resourceId = textViewResourceId;
         }
     
         //该方法在每个子项被滚动到屏幕内的时候会被调用。
         @NonNull
         @Override
         public View getView(int position, @Nullable View convertView, @NonNull ViewGroup parent) {
             Subwaystation station = getItem(position); //得到SubwayStation的实例
             //inflate()方法动态加载一个布局文件
             //参数1  要加载的布局文件的id  参数2 给加载好的布局再添加一个父布局
             //参数3  false 表示只让父布局中声明的layout属性生效，但不为这个view添加父布局
             View view = LayoutInflater.from(getContext()).inflate(resourceId, parent, false); 
             //获取到 ImageView TextView的实例  将station的属性设置给布局文件
             ImageView stationImage = view.findViewById(R.id.stationImage);
             TextView stationName = view.findViewById(R.id.stationName);
             //设置图片和文字
             stationImage.setImageResource(station.getImageId());
             stationName.setText(station.getName());
             return view;
         }
     }
     ```

   - 完成自定义适配器

4. 修改MainActivity中的代码

   - ```Java
     public class MainActivity extends AppCompatActivity {
         private List<Subwaystation> subwaystationList = new ArrayList<>();
         @Override
         protected void onCreate(Bundle savedInstanceState) {
             super.onCreate(savedInstanceState);
             setContentView(R.layout.activity_main);
             initStation(); //初始化
             //创建StationAdapter对象
             StationAdapter adapter = new StationAdapter(MainActivity.this, R.layout.station_item,subwaystationList);
             ListView listView = findViewById(R.id.listView);
             //将StationAdapter传递给ListView
             listView.setAdapter(adapter);
         }
         
         private void initStation() {
             //添加两遍，为了数据多一点
             for (int i = 0; i < 2; i++) {
                 Subwaystation songJiaZhuang  = new Subwaystation("宋家庄",R.drawable.ic_launcher_background); //图片用的自带的
                 subwaystationList.add(songJiaZhuang);
                 Subwaystation shiLiuZhuang  = new Subwaystation("石榴庄",R.drawable.ic_launcher_background);
                 subwaystationList.add(shiLiuZhuang);
                 Subwaystation daHongMen  = new Subwaystation("大红门",R.drawable.ic_launcher_background);
                 subwaystationList.add(daHongMen);
                 Subwaystation jiaoMenDong  = new Subwaystation("角门东",R.drawable.ic_launcher_background);
                 subwaystationList.add(jiaoMenDong);
                 Subwaystation jiaoMenXi  = new Subwaystation("角门西",R.drawable.ic_launcher_background);
                 subwaystationList.add(jiaoMenXi);
                 Subwaystation caoQiao  = new Subwaystation("草桥",R.drawable.ic_launcher_background);
                 subwaystationList.add(caoQiao);
                 Subwaystation niuJie  = new Subwaystation("牛街",R.drawable.ic_launcher_background);
                 subwaystationList.add(niuJie);
                 Subwaystation jiShuiTan  = new Subwaystation("积水潭",R.drawable.ic_launcher_background);
                 subwaystationList.add(jiShuiTan);
                 Subwaystation muDanYuan  = new Subwaystation("牡丹园",R.drawable.ic_launcher_background);
                 subwaystationList.add(muDanYuan);
             }
         }
     }
     ```

5. 效果

   - <img src="https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220415114833161.png" alt="image-20220415114833161" style="zoom:50%;" />

### 问题：适配器？

### 4.5.3 提升ListView的运行效率

在StationAdapter的getView()方法中，每次都将布局重新加载了一遍，当ListView快速滚动的时候，可能会成为性能的瓶颈。

使用convertView参数，将之前加载好的布局进行缓存，以便之后进行重用。

```java
public class StationAdapter extends ArrayAdapter<Subwaystation> {
	...
    @NonNull
    @Override
    public View getView(int position, @Nullable View convertView, @NonNull ViewGroup parent) {
        Subwaystation station = getItem(position);
        View view;
 		//进行了一个判断，为空则加载布局；不为空则对convertView进行重用
        if (convertView == null){
            view = LayoutInflater.from(getContext()).inflate(resourceId, parent, false);
        } else {
            view = convertView;
        }
        ImageView stationImage = view.findViewById(R.id.stationImage);
        TextView stationName = view.findViewById(R.id.stationName);
        stationImage.setImageResource(station.getImageId());
        stationName.setText(station.getName());
        return view;
    }
}
```

每次getView时还是会调用View的findViewById()方法来获取一次控件的实例。

```java
public class StationAdapter extends ArrayAdapter<Subwaystation> {
	...
    @NonNull
    @Override
    public View getView(int position, @Nullable View convertView, @NonNull ViewGroup parent) {
        Subwaystation station = getItem(position);
        View view;
        ViewHolder viewHolder;
        if (convertView == null){
            view = LayoutInflater.from(getContext()).inflate(resourceId, parent, false);
            viewHolder = new ViewHolder();
            viewHolder.stationImage = view.findViewById(R.id.stationImage);
            viewHolder.stationName = view.findViewById(R.id.stationName);
            view.setTag(viewHolder);
        } else {
            view = convertView;
            viewHolder = (ViewHolder) view.getTag();
        }
        viewHolder.stationImage.setImageResource(station.getImageId());
        viewHolder.stationName.setText(station.getName());
        return view;
    }
    //新增一个内部类ViewHolder  作用是对控件的实例进行缓存
    class ViewHolder {
        ImageView stationImage;
        TextView stationName;
    }
}
```

当convertView为null的时候，创建一个ViewHolder对象，并将控件的实例都存放在ViewHolder里，然后调用View的setTag()方法，将ViewHolder对象存储在View中。

当convertView不为null的时候，调用View的getTag()方法，把ViewHolder重新取出。这样所有控件的实例都缓存在了ViewHolder里，就没有必要每次都通过findViewById()方法来获取控件实例了。

### 4.5.4 ListView的点击事件

```java
public class MainActivity extends AppCompatActivity {

    private List<Subwaystation> subwaystationList = new ArrayList<>();

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        initStation(); //初始化
        StationAdapter adapter = new StationAdapter(MainActivity.this, R.layout.station_item,subwaystationList);
        ListView listView = findViewById(R.id.listView);
        listView.setAdapter(adapter);
        //给ListView注册一个监听器
        listView.setOnItemClickListener(new AdapterView.OnItemClickListener() {
            //点击ListView中任何一个子项时，回调的方法
            @Override
            public void onItemClick(AdapterView<?> parent, View view, int position, long id) {
                //通过position判断点击的是哪一个子项，获取到相应的Subwaystation
                Subwaystation subwaystation = subwaystationList.get(position);
               //输出name
                Toast.makeText(MainActivity.this,subwaystation.getName(),Toast.LENGTH_SHORT).show();
            }
        });
    }
    private void initStation() {
        for (int i = 0; i < 2; i++) {
            ...
        }
    }
}
```



## 4.6 RecyclerView

增强版的ListView。

### 4.6.1 RecyclerView的基本用法

创建RecyclerViewTest

修改activity_main.xml中的代码

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    >
    <androidx.recyclerview.widget.RecyclerView
        android:id="@+id/recyclerView"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />
</LinearLayout>
```

创建一个Subwaystation类 和 station_item.xml

1. 为RecyclerView准备一个适配器  StationAdapter继承RecyclerView.Adapter将泛型制定为<StationAdapter.ViewHolder>。ViewHolder是在StationAdapter中自定义的一个内部类

   - ```java
     public class StationAdapter extends RecyclerView.Adapter<StationAdapter.ViewHolder> {
         private List<Subwaystation> mSubwaystations;
     
         //首先定义一个内部类，继承自RecyclerView.ViewHolder
         static class ViewHolder extends RecyclerView.ViewHolder{
             ImageView stationImage;
             TextView stationName;
             //构造方法传入的view参数  ----> 通常是RecyclerView子项的最外层布局
             public ViewHolder(@NonNull View itemView) {
                 super(itemView);
                 //可以通过 findViewById 来获取到布局中的 ImageView 和 TextView
                 stationImage = itemView.findViewById(R.id.stationImage);
                 stationName = itemView.findViewById(R.id.stationName);
             }
         }
         // StationAdapter这个构造方法  用于把要展示的数据源传进来，并赋值给一个全局变量mSubwaystations
         // 后续操作在这个数据源上进行
         public StationAdapter(List<Subwaystation> subwaystations) {
             mSubwaystations = subwaystations;
         }
     
         /**
          * 重写onCreateViewHolder()  用于创建ViewHolder实例
          *   parent – 新视图绑定到适配器位置后将添加到的视图组。 
          *   viewType – 新视图的视图类型。
          *   返回：一个新的 ViewHolder，它持有给定视图类型的视图。
          * onBindViewHolder()       用于对RecyclerView子项的数据进行赋值，会在每个子项被滚动到屏幕内的时候执行
          * holder - 应更新以表示数据集中给定位置的项目内容的 ViewHolder。 
          * position – 项目在适配器数据集中的位置。
          * getItemCount()           用于告诉RecyclerView一共有多少子项，直接返回数据源长度即可
          * */
         @NonNull
         @Override
         public ViewHolder onCreateViewHolder(@NonNull ViewGroup parent, int viewType) {
             //将station_item布局加载进来，然后创建一个ViewHolder实例，将加载出来的布局传入到构造方法中
             View view = LayoutInflater.from(parent.getContext()).inflate(R.layout.station_item,parent,false);
             ViewHolder holder = new ViewHolder(view);
             return holder;
         }
     
         @Override
         public void onBindViewHolder(@NonNull ViewHolder holder, int position) {
             //通过position参数得到当前项的 Subwaystation，再将数据设置到ViewHolder的ImageView和TextView
             Subwaystation subwaystation = mSubwaystations.get(position);
             holder.stationImage.setImageResource(subwaystation.getImageId());
             holder.stationName.setText(subwaystation.getName());
         }
     
         @Override
         public int getItemCount() {
             return mSubwaystations.size();
         }
     }
     ```

   - 首先定义了一个内部类ViewHolder,它要继承自RecyclerView.ViewHolder。然后ViewHolder的主构造函数中要传入一个View参数,这个参数通常就是RecyclerView子项的最外层布局,可以通过findViewById()方法来获取布局中ImageView和TextView的实例。

   - FruitAdapter中也有一个主构造函数,它用于把要展示的数据源传进来,后续的操作都
     将在这个数据源的基础上进行。
     FruitAdapter是继承自RecyclerView.Adapter的,必须重写onCreateViewHolder()、onBindViewHolder()和getItemCount()这3个方法。

     - onCreateViewHolder()方法是用于创建ViewHolder实例的这个方法中将fruit_item布局加载进来,然后创建一个ViewHolder实例,并把加载出来的布局传入构造函数当中,最后将ViewHolder的实例返回。
     - onBindViewHolder()方法用于对RecyclerView子项的数据进行赋值,会在每个子项被滚动到屏幕内的时候执行,通过position参数得到当前项的Fruit实例,然后再将数据设置到ViewHolder的ImageView和TextView中。
     - getItemCount()方法用于告诉RecyclerView一共有多少子项,直接返回数据源的长度。

2. 使用RecyclerView，修改MainActivity

   - ```java
     public class MainActivity extends AppCompatActivity {
         private List<Subwaystation> subwaystationList = new ArrayList<>();
         @Override
         protected void onCreate(Bundle savedInstanceState) {
             super.onCreate(savedInstanceState);
             setContentView(R.layout.activity_main);
             initStation(); //
             RecyclerView recyclerView = findViewById(R.id.recyclerView);
             //LayoutManager 用于指定RecyclerView的布局方式
             //LinearLayoutManager 线性布局
             LinearLayoutManager layoutManager = new LinearLayoutManager(this);
             //将LinearLayoutManager对象设置到RecyclerView中
             recyclerView.setLayoutManager(layoutManager);
             //将初始化的数据传入到stationAdapter
             StationAdapter adapter = new StationAdapter(subwaystationList);
             recyclerView.setAdapter(adapter); //完成适配器设置
         }
         //初始化所有车站
         private void initStation() {
             for (int i = 0; i < 2; i++) {
                 ...
             }
         }
     }
     ```

### 4.6.2 实现横向滚动和瀑布流布局

1. 实现横向滚动

   1. 先对station_item布局进行修改

      - ```xml
        <?xml version="1.0" encoding="utf-8"?>
        <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
            android:orientation="vertical"  改为垂直方向排列
            android:layout_width="100dp"    固定宽度 wrap导致长短不一 match导致占满屏幕
            android:layout_height="wrap_content">
            <ImageView
                android:id="@+id/stationImage"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_gravity="center_horizontal"/> 水平居中
            <TextView
                android:id="@+id/stationName"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_gravity="center_horizontal"  水平居中
                android:layout_marginTop="10dp"/>           图片和文字之间留点距离
        </LinearLayout>
        ```

   2. 对MainActivity修改

      - ```java
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
            initStation();
            RecyclerView recyclerView = findViewById(R.id.recyclerView);
        	//创建LinearLayoutManager 并设置为横向排列
            LinearLayoutManager layoutManager = new LinearLayoutManager(this);
            layoutManager.setOrientation(LinearLayoutManager.HORIZONTAL);
        
            recyclerView.setLayoutManager(layoutManager);
            StationAdapter adapter = new StationAdapter(subwaystationList);
            recyclerView.setAdapter(adapter);
        }
        ```

------

```Java
/*参数：
context – 当前上下文，将用于访问资源。
spanCount – 网格中的列数或行数
方向 - 布局方向。应该是HORIZONTAL或VERTICAL 。
*/
GridLayoutManager layoutManager1 = new GridLayoutManager(this,3);
layoutManager1.setOrientation(GridLayoutManager.HORIZONTAL);
```

-----

1. StaggeredGridLayoutManager实现瀑布流布局

   - 修改station_item.xml

   - ```xml
     <?xml version="1.0" encoding="utf-8"?>
     <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
         android:orientation="vertical"
         android:layout_width="match_parent"  根据布局的列数自动适配
         android:layout_height="wrap_content"
         android:layout_margin="5dp">         子项之间的距离
         <ImageView
             android:id="@+id/stationImage"
             android:layout_width="wrap_content"
             android:layout_height="wrap_content"
             android:layout_gravity="center_horizontal"/>
         <TextView
             android:id="@+id/stationName"
             android:layout_width="wrap_content"
             android:layout_height="wrap_content"
             android:layout_gravity="left"    左对齐 后面将文字的长度变长
             android:layout_marginTop="10dp"/>
     </LinearLayout>
     ```

2. 修改MainActivity

   - ```Java
     public class MainActivity extends AppCompatActivity {
         private List<Subwaystation> subwaystationList = new ArrayList<>();
         @Override
         protected void onCreate(Bundle savedInstanceState) {
             super.onCreate(savedInstanceState);
             setContentView(R.layout.activity_main);
             initStation();
             RecyclerView recyclerView = findViewById(R.id.recyclerView);
     		//参数1  指定布局的列数  参数2  指定布局的排列方向
             StaggeredGridLayoutManager layoutManager2 = new StaggeredGridLayoutManager(3,StaggeredGridLayoutManager.VERTICAL);
             
             recyclerView.setLayoutManager(layoutManager2);
             StationAdapter adapter = new StationAdapter(subwaystationList);
             recyclerView.setAdapter(adapter);
         }
     
         private void initStation() {
             for (int i = 0; i < 2; i++) {
                 Subwaystation songJiaZhuang  = new Subwaystation(getRandomLengthName("宋家庄"),R.drawable.ic_launcher_background);
                 ...
             }
         }
         //将名字长度设成不一致的  便于观察高度不一致
         private String getRandomLengthName(String name){
             Random random = new Random();
             int length = random.nextInt(20) + 1;
             StringBuilder builder = new StringBuilder();
             for (int i = 0; i < length; i++) {
                 builder.append(name);
             }
             return builder.toString();
         }
     }
     ```

   - <img src="https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/image-20220418141917475.png" alt="image-20220418141917475" style="zoom:50%;" />

### 4.6.3 RecyclerView的点击事件

RecyclerView并没有提供类似于setOnItemClickListener()这样的注册监听器方法,而是需要、给子项具体的View去注册点击事件。

```Java
public class StationAdapter extends RecyclerView.Adapter<StationAdapter.ViewHolder> {
    private List<Subwaystation> mSubwaystations;

    static class ViewHolder extends RecyclerView.ViewHolder{
        View stationView; //保存子项最外层布局的实例
        ImageView stationImage;
        TextView stationName;

        public ViewHolder(@NonNull View itemView) {
            super(itemView);
            stationView = itemView;
            stationImage = itemView.findViewById(R.id.stationImage);
            stationName = itemView.findViewById(R.id.stationName);
        }
    }
    public StationAdapter(List<Subwaystation> subwaystations) {
        mSubwaystations = subwaystations;
    }

    @NonNull
    @Override
    public ViewHolder onCreateViewHolder(@NonNull ViewGroup parent, int viewType) {

        View view = LayoutInflater.from(parent.getContext()).inflate(R.layout.station_item,parent,false);
        final ViewHolder holder = new ViewHolder(view);
        //给最外层布局注册点击事件
        holder.stationView.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                int position = holder.getAdapterPosition();
                Subwaystation subwaystation = mSubwaystations.get(position);
                Toast.makeText(view.getContext(),"you click view" + subwaystation.getName(),Toast.LENGTH_SHORT).show();
            }
        });
        //给图片注册点击事件
        holder.stationImage.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                int position = holder.getAdapterPosition();
                Subwaystation subwaystation = mSubwaystations.get(position);
                Toast.makeText(view.getContext(),"you click image" + subwaystation.getName(),Toast.LENGTH_SHORT).show();
            }
        });
        return holder;
    }

    @Override
    public void onBindViewHolder(@NonNull ViewHolder holder, int position) {

        Subwaystation subwaystation = mSubwaystations.get(position);
        holder.stationImage.setImageResource(subwaystation.getImageId());
        holder.stationName.setText(subwaystation.getName());
    }

    @Override
    public int getItemCount() {
        return mSubwaystations.size();
    }
}
```

## 4.7 编写界面的最佳实践

新创建一个UIBestPractice

1. 编写主界面

   - 修改activity_main.xml

   - ```xml
     <?xml version="1.0" encoding="utf-8"?>
     <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
         xmlns:app="http://schemas.android.com/apk/res-auto"
         xmlns:tools="http://schemas.android.com/tools"
         android:orientation="vertical"
         android:layout_width="match_parent"
         android:layout_height="match_parent"
         android:background="#d8e0e8"
         tools:context=".MainActivity">
         <androidx.recyclerview.widget.RecyclerView
             android:id="@+id/msgRecyclerView"
             android:layout_width="match_parent"
             android:layout_height="0dp"
             android:layout_weight="1" />
         <LinearLayout
             android:layout_width="match_parent"
             android:layout_height="wrap_content" >
             <EditText
                 android:id="@+id/inputText"
                 android:layout_width="0dp"
                 android:layout_height="wrap_content"
                 android:layout_weight="1"
                 android:hint="type something here"
                 android:maxLines="2" />
             <Button
                 android:id="@+id/send"
                 android:layout_width="wrap_content"
                 android:layout_height="wrap_content"
                 android:text="Send" />
         </LinearLayout>
     </LinearLayout>
     ```

   - RecyclerView 用于显示聊天的消息内容，EditText用于输入消息，Button用于发送消息

2. 定义消息的实体类

   - ```java
     public class Msg {
         public final static int TYPE_RECEIVED = 0;  //收到
         public final static int TYPE_SENT = 1;      //发送
         //表示消息的内容
         private String content;
         //表示消息的类型
         private int type;
     
         public Msg(String content, int type) {
             this.content = content;
             this.type = type;
         }
     
         public Msg() {
         }
     
         public String getContent() {
             return content;
         }
     
         public int getType() {
             return type;
         }
     }
     ```

3. 编写RecyclerView子项的布局  msg_item.xml

   - ```xml
     <?xml version="1.0" encoding="utf-8"?>
     <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
         android:orientation="vertical"
         android:layout_width="match_parent"
         android:layout_height="wrap_content"
         android:padding="10dp">
     
         <LinearLayout
             android:id="@+id/leftLayout"
             android:layout_width="wrap_content"
             android:layout_height="wrap_content"
             android:layout_gravity="left"
             android:background="@drawable/message_left" >
             <TextView
                 android:id="@+id/leftMsg"
                 android:layout_width="wrap_content"
                 android:layout_height="wrap_content"
                 android:layout_gravity="center"
                 android:layout_margin="10dp"
                 android:textColor="#fff" />
         </LinearLayout>
     
         <LinearLayout
             android:id="@+id/rightLayout"
             android:layout_width="wrap_content"
             android:layout_height="wrap_content"
             android:layout_gravity="right"
             android:background="@drawable/message_right" >
             <TextView
                 android:id="@+id/rightMsg"
                 android:layout_width="wrap_content"
                 android:layout_height="wrap_content"
                 android:layout_gravity="center"
                 android:layout_margin="10dp" />
         </LinearLayout>
     
     </LinearLayout>
     ```

   - 收到消息左对齐，发出的消息右对齐

4. 创建RecyclerView的适配器类  MsgAdapter

   - ```Java
     public class MsgAdapter extends RecyclerView.Adapter<MsgAdapter.ViewHolder> {
         private List<Msg> mMsgList;
     
         static class ViewHolder extends RecyclerView.ViewHolder{
             LinearLayout leftLayout;
             LinearLayout rightLayout;
             TextView leftMsg;
             TextView rightMsg;
             public ViewHolder(@NonNull View itemView) {
                 super(itemView);
                 leftLayout = itemView.findViewById(R.id.leftLayout);
                 rightLayout = itemView.findViewById(R.id.rightLayout);
                 leftMsg = itemView.findViewById(R.id.leftMsg);
                 rightMsg = itemView.findViewById(R.id.rightMsg);
             }
         }
     
         public MsgAdapter(List<Msg> mMsgList) {
             this.mMsgList = mMsgList;
         }
     
         @NonNull
         @Override
         public ViewHolder onCreateViewHolder(@NonNull ViewGroup parent, int viewType) {
             View view = LayoutInflater.from(parent.getContext()).inflate(R.layout.msg_item,parent,false);
             return new ViewHolder(view);
         }
     
         @Override
         public void onBindViewHolder(@NonNull ViewHolder holder, int position) {
             Msg msg = mMsgList.get(position);
             if (msg.getType() == Msg.TYPE_RECEIVED) {
                 //如果是收到的消息，显示左边的消息布局，右边的消息布局隐藏
                 holder.leftLayout.setVisibility(View.VISIBLE);
                 holder.rightLayout.setVisibility(View.GONE);
                 holder.leftMsg.setText(msg.getContent());
             }else if (msg.getType() == Msg.TYPE_SENT) {
                 //如果是发出的消息，显示右边的消息布局，左边的消息布局隐藏
                 holder.rightLayout.setVisibility(View.VISIBLE);
                 holder.leftLayout.setVisibility(View.GONE);
                 holder.rightMsg.setText(msg.getContent());
             }
         }
         @Override
         public int getItemCount() {
             return mMsgList.size();
         }
     }
     ```

5. 修改MainActivity 

   - ```Java
     public class MainActivity extends AppCompatActivity {
         private List<Msg> msgList = new ArrayList<>();
         private EditText inputText;
         private Button send;
         private RecyclerView msgRecyclerView;
         private MsgAdapter adapter;
         @Override
         protected void onCreate(Bundle savedInstanceState) {
             super.onCreate(savedInstanceState);
             setContentView(R.layout.activity_main);
             initMsgs();
             inputText = findViewById(R.id.inputText);
             send = findViewById(R.id.send);
             msgRecyclerView = findViewById(R.id.msgRecyclerView);
             LinearLayoutManager layoutManager = new LinearLayoutManager(this);
             msgRecyclerView.setLayoutManager(layoutManager);
             adapter = new MsgAdapter(msgList);
             msgRecyclerView.setAdapter(adapter);
             send.setOnClickListener(new View.OnClickListener() {
                 @Override
                 public void onClick(View view) {
                     String content = inputText.getText().toString();
                     if (!"".equals(content)) {
                         Msg msg = new Msg(content,Msg.TYPE_SENT);
                         msgList.add(msg);
                         adapter.notifyItemInserted(msgList.size() - 1);  //
                         msgRecyclerView.scrollToPosition(msgList.size() - 1);    //
                         inputText.setText("");  //清空输入框中的内容
                     }
                 }
             });
         }
         private void initMsgs(){
             Msg msg1 = new Msg("Hello guy!", Msg.TYPE_RECEIVED);
             msgList.add(msg1);
             Msg msg2 = new Msg("Hello! 你是谁？", Msg.TYPE_SENT);
             msgList.add(msg2);
             Msg msg3 = new Msg("我是哈哈哈，哈哈哈  哈哈哈哈", Msg.TYPE_RECEIVED);
             msgList.add(msg3);
         }
     }
     ```

   - 