# Android 应用开发（前九章）

[TOC]

## 第1章 Android 概述

### 1 安卓简介

#### 1.1 Android 简介

1. 2008年9月，google发布Android 1.0
1. 是第一个免费的、开源的手机端操作系统，
1. 基于Linux内核，使用Java 语言开发。
1. 2008年10月，第一款Android手机HTC G1在美国上市

#### 1.2 Android 体系结构

1. 整体体系结构：应用程序层，应用程序框架层、系统运行库层、Linux内核层

   <img src="Android 应用开发.assets/image-20230522190616118-1684831363612120.png" alt="image-20230522190616118" style="zoom:50%;" />

2. 应用程序层

   - Android平台的应用层上包括各类与用户直接交互的应用程序，或由java语言编写的运行于后台的服务程序
   - 例如：SMS短信，电话拨号，图片浏览器，日历，游戏， 地图，web浏览器等程序，以及开发人员开发的其他应用 程序。

3. 应用程序框架层

   <img src="Android 应用开发.assets/image-20230522191054238-1684831363612119.png" alt="image-20230522191054238" style="zoom: 50%;" />

4. 系统运行库层

   - 程序库：
     - C库、媒体库、Surface Manager、**WebKit**、SGL、3D libraries、FreeType、SSL、**SQLite**等
   - Android 运行库：
     - 每一个Android应用程序都在它自己的进程中运行，都拥有一 个独立的Dalvik虚拟机实例。
     - Dalvik虚拟机依赖于linux内核的一些功能，比如线程机制和 底层内存管理机制。

5. 关于JVM和Dalvik VM

   -  JVM：执行class文件（字节码 ）
   -  Dalvik VM：执行dex文件（Dalvik Executes缩写）
   -  Dalvik最大的好处在于可以根据硬件实现更大的优化，这更适合移动设备。

6. Linux内核层

   - Android的核心系统服务依赖于Linux内核，如安全性，内存管理，进程管理，网络协议栈和驱动模型。
   - Linux内核也同时作为硬件和软件栈之间的抽象层。

#### 1.3 Android 系统特点

1. 在内存和进程管理方面，Android有自己的运行时和虚拟机；

2. Android将界面设计与程序逻辑分离，界面布局使用XML文件，有利于界面的修改和维护；

3. Android提供轻量级的进程间通讯机制Intent，使跨进程组件通 信和发送系统广播成为可能；

4. Android提供Service作为无用户界面、长时间后台运行组件；

5. 支持快速、高效的数据存储：

   - SharedPreferences、文件存储、SQLite数据库

   - Android四大核心组件：Activity、Service、Broadcast Receiver、Content Provider

     <img src="Android 应用开发.assets/image-20230522200113269-1684831363612121.png" alt="image-20230522200113269" style="zoom:67%;" />

6. 为了跨进程共享数据，Android提供ContentProvider接口

   - 可以无需了解数据源、路径的情况下，对共享数据进行查询、添加、删除和更新等操作

     <img src="Android 应用开发.assets/image-20230522201007732-1684831363612122.png" alt="image-20230522201007732" style="zoom: 67%;" />

7. 其他

   <img src="Android 应用开发.assets/image-20230522201109489-1684831363612123.png" alt="image-20230522201109489" style="zoom: 33%;" />

## 第2章 Android开发环境搭建

略

## 第3章 Android项目的创建和运行

1. 关于Toast

   - Toast 在应用程序上浮动显示少量信息给用户，它永远不会获得焦点，不影响用户的输入等操作。

   - 基本用法：

     <img src="Android 应用开发.assets/image-20230522203411696-1684831363612124.png" alt="image-20230522203411696" style="zoom: 33%;" />

## 第4章 Activity

### 1 理解Activity

1. Activity和界面布局layout
   - 墙就类似Activity，壁橱就类似Layout布局管理器，壁橱上的东西就类似那些UI元素。
   - 墙的背后或许还有我们看不见的东西，这些东西可能是Java的实体类、逻辑控制类、网络连接类等。
2. 注意
   - 通常一个界面布局对应一个Activity，布局文件命名：activity_\*\*\*.xml， 其中***对应某个Activity名
   - 但一个Activity不一定对应一个界面布局

### 2 Activity切换不同的布局

1. 步骤：新建布局文件、创建菜单、添加菜单代码

   - 在新的布局中，将一个ImageView控件拖放到界面上，并设置图片属性android:src="@drawable/***"（先把图片复制到drawable目录中

   - 创建菜单的时候：新建menu资源文件夹，然后创建菜单文件

     - 新建 Android resource directory，命名为menu

     - 在menu文件夹中新建 menu resource file，命名为"main_menu"

     - 新建菜单项（Menu Item），设置id和title

     - 在MainActivity中添加菜单代码：

       <img src="Android 应用开发.assets/image-20230522210231419-1684831363612125.png" alt="image-20230522210231419" style="zoom:33%;" />

       <img src="Android 应用开发.assets/image-20230522210247400-1684831363612126.png" alt="image-20230522210247400" style="zoom: 40%;" />

     - 一个问题：当切换到布局2，然后再切换到布局1后，发现布局1中的按钮click事件 后失效！

       原因：onCreate()方法在Activity生命周期中只调用一次

       > 解决方法：（代码见后）
       >
       > 1. 将初始化代码(如click代码)单独定义为一个方法；
       > 2. 在onCreate中setContentView()之后调用一次；
       > 3. 然后在菜单onOptionsItemSelected的setContentView()之后再调用 一次。
       >
       > <img src="Android 应用开发.assets/image-20230522210449250-1684831363612128.png" alt="image-20230522210449250" style="zoom:40%;" />

### 3 设置Activity的布局背景

1. 设置背景色
   - 先在res/values/colors.xml中添加某个颜色值：
     - \<color name="bg">#FF0000\</color>	<!--红色-->
   - 然后给布局Layout设置background属性：
     - android:background="@color/bg"
2. 设置背景图片：
   - android:background="@drawable/图片id" （默认图片会自动拉伸）
   - 或者动态加载：getWindow().setBackgroundDrawableResource(R.drawable.***);

### 4 Activity生命周期

1. Activity生命周期指Activity从启动到销毁的过程。

2. Activity表现为四种状态：

   - **活动状态Active：**Activity在用户界面中处于最上层，完全能被 用户看到，能够与用户进行交互。
   - **暂停状态Pause：**Activity在界面上被部分遮挡，不再处于用户界面的最上层，且不能够与用户进行交互。(如弹出选择框时)
   - **停止状态Stop：**Activity被其他Activity全部遮挡，界面完全不 能被用户看到。(如玩游戏时来电了)
   - **非活动状态Dead：**Activity没有启动或者被finish()。

3. Activity栈：后进先出

   <img src="Android 应用开发.assets/image-20230522211131442-1684831363612127.png" alt="image-20230522211131442" style="zoom:50%;" />

4. Activity生命周期的事件回调函数

   - 随着Activity自身状态的变化，Android系统会调用不同的事件回调函数(7个)：

     ```java
     public class MainActivity extends Activity {
         protected void onCreate(Bundle savedInstanceState); 
         protected void onStart();
         protected void onRestart(); 
         protected void onResume(); 
         protected void onPause(); 
         protected void onStop();
         protected void onDestroy();
     }
     ```

   - 回调函数说明：

     <img src="Android 应用开发.assets/image-20230522211319457-1684831363612129.png" alt="image-20230522211319457" style="zoom:50%;" />

   - Activity生命周期图

     <img src="Android 应用开发.assets/image-20230522211548658-1684831363612130.png" alt="image-20230522211548658" style="zoom:67%;" />

5. 有关项目配置文件AndroidManifest.xml

   - 相关解释与代码信息：

     <img src="Android 应用开发.assets/image-20230522211841127-1684831363612131.png" alt="image-20230522211841127" style="zoom:50%;" />

     - Intent Filter就是 用来注册 Activity 、 Service 和 Broadcast Receiver 具有能在某种数据上执行一个动作的能力。使用 Intent Filter ，应用程序组件告诉 Android ，它们能为其它程序的组件的动作请求提供服务，包括同一个程序的组件、本地的或第三方的应用程序。

   - 每一个Android项目都有一个名为AndroidManifest.xml的配置文件，在所有项目中该文件的名称不变

   - 该文件是Android工程的一个全局配置文件，所有在项目 中使用的组件(如 Activity，Service，Content provider和Broadcast receivers等)都要在该文件中声明。

   - 该文件还可以声明一些android权限等信息。

6. Android工程中各个文件之间的关系

   <img src="Android 应用开发.assets/image-20230522214230111-1684831363612132.png" alt="image-20230522214230111" style="zoom: 67%;" />

7. 各类测试中Activity的回调函数调用顺序：

   - 正常启动程序：onCreate → onStart → onResume【创建、启动、恢复】
   - 程序启动后点击返回：onPause → onStop → onDestory【暂停、停止、再见】
   - 正常启动后按Home键：onPause → onStop
   - 按Home键后再单击应用进入：onRestart → onStart → onResume
   - 正常启动后切换为横屏（Ctrl+F11）：onPause → onStop → onDestroy → onCreate → onStart→ onResume
   - 锁屏后解锁：onPause → onStop → onRestart → onStart → onResume
   - 来电通话后挂机（来电是以通知的形式显示，接通后onPause部分遮挡？，挂机后onResume）：onPause → onResume

### 5 Android程序调试

1. Android程序使用断点调试有时可能不好（如多线程程序）

2. Android程序调试还可通常使用日志文件来记录调试信息。

3. Android提供一个静态Log类，利用Log类的相关方法，将 调试信息写入日志文件。

4. LogCat工具可以实时查看日志信息。

5. Log类方法：

   <img src="Android 应用开发.assets/image-20230522221028708-1684831363612133.png" alt="image-20230522221028708" style="zoom: 40%;" />

   - 查看Log信息：

     <img src="Android 应用开发.assets/image-20230522221133989-1684831363612134.png" alt="image-20230522221133989" style="zoom:80%;" />

## 第5章 Android用户界面

### 1 用户界面基础

1. 概述：用户界面UI（User Interface）是系统和用户之间进行信息交换的媒介。

2. 移动用户界面设计应解决的问题

   - **界面设计与程序逻辑完全分离**：这不仅有利于并行开发，而且在后期修改界面时，不用再次修改程序的逻辑代码
   - 根据不同型号的屏幕能自动调整界面元素的位置和尺寸：避免因为屏幕信息的变化而出现显示错误
   - 增强用户体验：能够合理利用较小的屏幕显示空间，构造出符合人机交互规 律的用户界面

3. MVC（Model-View-Controller）模式：

   - Android界面框架采用了当前流行的MVC模式：

     - 视图V：显示用户界面(XML布局)

     - 控制器C：处理用户输入(Activity)

     - 模型M：处理业务逻辑(数据/逻辑代码)

       <img src="Android 应用开发.assets/image-20230522221536364-1684831363612135.png" alt="image-20230522221536364" style="zoom:80%;" />

   - MVC目的：

     - M和V的实现代码分离，从而使同一个程序可以使用不同的表 现形式，C可确保M和V的同步，一旦M改变，V能同步更新。

### 2 界面基本组件

> TextView / ImageView
>
> Button / ImageButton / ToggleButton
>
> EditText
>
> RadioButton / CheckBox / Spinner
>
> ListView

1. TextView

   - 基本属性：

     <img src="Android 应用开发.assets/image-20230522221903215-1684831363612136.png" alt="image-20230522221903215" style="zoom: 40%;" />

   - 在程序中控制TextView

     <img src="Android 应用开发.assets/image-20230522222103447-1684831363612137.png" alt="image-20230522222103447" style="zoom:40%;" />

   - 补充：android:autoLink属性

     - android:autoLink="web"	自动识别web超链接
     - android:autoLink="email"	自动识别email地址
     - android:autoLink="phone"	自动识别电话号码
     - android:autoLink="all"    自动检测所有可能，并转换为可点击的链接

2. ImageView

   - 基本XML描述：

     <img src="Android 应用开发.assets/image-20230522223258767-1684831363612139.png" alt="image-20230522223258767" style="zoom: 40%;" />

   - 在程序中控制ImageView

     <img src="Android 应用开发.assets/image-20230522223441720-1684831363612138.png" alt="image-20230522223441720" style="zoom:40%;" />

   - 补充：ImageView从相册中选择图片

     <img src="Android 应用开发.assets/image-20230522223525418-1684831363612140.png" alt="image-20230522223525418" style="zoom:40%;" />

3. Button / ImageButton / ToggleButton   <img src="Android 应用开发.assets/image-20230522224209746-1684831363612141.png" alt="image-20230522224209746" style="zoom: 40%;" />

   - 基本属性

     - Button：

       <img src="Android 应用开发.assets/image-20230522224257434-1684831363612144.png" alt="image-20230522224257434" style="zoom: 50%;" />

     - ImageButton

       <img src="Android 应用开发.assets/image-20230522224322557-1684831363612142.png" alt="image-20230522224322557" style="zoom:50%;" />

     - ToggleButton

       <img src="Android 应用开发.assets/image-20230522224500754-1684831363612143.png" alt="image-20230522224500754" style="zoom:40%;" />

   - Button / ImageButton单击事件响应

     <img src="Android 应用开发.assets/image-20230522224400631-1684831363612145.png" alt="image-20230522224400631" style="zoom: 40%;" />

   - ToggleButton开关切换事件响应

     <img src="Android 应用开发.assets/image-20230522224741922-1684831363612146.png" alt="image-20230522224741922" style="zoom:40%;" />

4. EditText

   - 基本属性：

     <img src="Android 应用开发.assets/image-20230522224836470-1684831363612147.png" alt="image-20230522224836470" style="zoom:40%;" />

   - android:inputType的一些取值：

     - android:inputType="numberPassword"	数字密码框
     - android:inputType="textMultiLine"	多行文本
     - android:inputType="number"	数字键盘(只能输入数字)
     - android:inputType="phone"	拨号键盘
     - android:inputType="time"	时间键盘

   - android:digits属性：

     - 该属性用于设置允许输入哪些字符。
     - 如android:digits="abcd"	，表示只能输入abcd这四种字符

   - 在程序中控制EditText

     <img src="Android 应用开发.assets/image-20230522225339557-1684831363612148.png" alt="image-20230522225339557" style="zoom:40%;" />

5. RadioButton <img src="Android 应用开发.assets/image-20230522225413979-1684831363612149.png" alt="image-20230522225413979" style="zoom: 50%;" />

   - 单选按钮须归属一个组，每个组只能有一个选项被选中，所以通常使用的是RadioGroup

   - 主要属性：

     <img src="Android 应用开发.assets/image-20230522225744953-1684831363612150.png" alt="image-20230522225744953" style="zoom: 60%;" />

   - 如何获取RadioButton选中项的值

     <img src="Android 应用开发.assets/image-20230522230226777-1684831363612151.png" alt="image-20230522230226777" style="zoom:40%;" />

   - RadioGroup选中项 改变事件

     <img src="Android 应用开发.assets/image-20230522230433147-1684831363612152.png" alt="image-20230522230433147" style="zoom:40%;" />

6. CheckBox（多选框/勾选框）

   - 基本属性

     <img src="Android 应用开发.assets/image-20230523100304875-1684831363612153.png" alt="image-20230523100304875" style="zoom: 40%;" />

   - 如何获得多个CheckBox的选中值

     <img src="Android 应用开发.assets/image-20230523100350262-1684831363612155.png" alt="image-20230523100350262" style="zoom:45%;" />

   - CheckBox选中状态改变事件响应

     <img src="Android 应用开发.assets/image-20230523100531879-1684831363612154.png" alt="image-20230523100531879" style="zoom:45%;" />

7. Spinner（下拉列表）

   - 基本属性

     <img src="Android 应用开发.assets/image-20230523100653757-1684831363612156.png" alt="image-20230523100653757" style="zoom:40%;" />

     - prompt：设置下拉列表未展开时显示的提示文本

     - popupBackground：设置下拉列表的背景，通常使用@drawable/selector，使用xml文件来定义，例子如下：

       <img src="Android 应用开发.assets/image-20230523101133839-1684831363612157.png" alt="image-20230523101133839" style="zoom:80%;" />

     - entries：指定下拉列表数据源，通常使用@array/spinnerArray，使用xml文件来定义，例子如下

       <img src="Android 应用开发.assets/image-20230523104026170-1684831363613158.png" alt="image-20230523104026170" style="zoom:80%;" />

   - Spinner数据填充：

     - 打开res/values/strings.xml文件，添加：

       ```xml
       <string name="major">专业</string>
       ```

     - 新建res/values/arrays.xml资源文件：

       ```xml
       <string-array name="major_array">
       	<item>计算机科学\</item>
       	<item>软件工程\</item>
       </string-array>
       ```

     - 在Spinner对象中添加属性：

       <img src="Android 应用开发.assets/image-20230523104300622-1684831363613159.png" alt="image-20230523104300622" style="zoom: 33%;" />

   - 如何获取Spinner选中项值

     <img src="Android 应用开发.assets/image-20230523104653992-1684831363613160.png" alt="image-20230523104653992" style="zoom:40%;" />

   - Spinner选中项事件响应

     <img src="Android 应用开发.assets/image-20230523110313507-1684831363613161.png" alt="image-20230523110313507" style="zoom:40%;" />

     - Spinner选中项事件示例

       <img src="Android 应用开发.assets/image-20230523110354097-1684831363613162.png" alt="image-20230523110354097" style="zoom:40%;" />

   - Spinner级联操作：

     <img src="../../../学习/大三/移动计算技术/Android 应用开发/Android 应用开发.assets/image-20230523110603237.png" alt="image-20230523110603237" style="zoom:25%;" /><img src="Android 应用开发.assets/image-20230523110617555-1684831363613164.png" alt="image-20230523110617555" style="zoom:33%;" />

     <img src="Android 应用开发.assets/image-20230523110710559-1684831363613165.png" alt="image-20230523110710559" style="zoom:80%;" />

8. ListView 列表框：

   - ListView通常使用适配器来填充数据和设置显示样式。

   - ListView主要属性：

     <img src="Android 应用开发.assets/image-20230523111826647-1684831363613166.png" alt="image-20230523111826647" style="zoom:40%;" />

     - 数据填充（静态）：

       <img src="Android 应用开发.assets/image-20230523111904553-1684831363613168.png" alt="image-20230523111904553" style="zoom:35%;" />

     - 数据填充（动态）

       <img src="Android 应用开发.assets/image-20230523112102551-1684831363613167.png" alt="image-20230523112102551" style="zoom: 40%;" />

   - 系统提供的其他布局：

     <img src="Android 应用开发.assets/image-20230523112227169-1684831363613169.png" alt="image-20230523112227169" style="zoom:40%;" />

   - ListView选项单击事件响应

     <img src="Android 应用开发.assets/image-20230523112258474-1684831363613170.png" alt="image-20230523112258474" style="zoom:40%;" />

   - Adapter讲解：

     -  Adapter是连接后端数据和前端显示的适配器接口，是数据和UI（View）之间一个重要的纽带。

     - 在常见的View(ListView,GridView)等地方都需要用到Adapter。

       <img src="Android 应用开发.assets/image-20230523112602173-1684831363613171.png" alt="image-20230523112602173" style="zoom:40%;" />

     - BaseAdapter是一个抽象类，继承它需要实现较多的方法，所以也就具有较高的灵活性。

     - ArrayAdapter支持泛型操作，最为简单，只能展示一行字。

     - SimpleAdapter有最好的扩充性，可以自定义出各种效果。

     - SimpleCursorAdapter可以适用于简单的纯文字型ListView，它需要Cursor的字段和UI的id对应起来。

     - 如需要实现更复杂的UI也可以重写其他方法。可以认为是SimpleAdapter对数据库的简单结合，可以方便地把数据库的内容以列表的形式展示出来。

   - 如何获取选中的多选值

     <img src="Android 应用开发.assets/image-20230523112802214-1684831363613172.png" alt="image-20230523112802214" style="zoom:40%;" />

   - ListView示例：简单的音乐播放器（真机测试）

     - 关键问题：
       - android权限问题：如添加SD卡访问权限
   
       - java文件操作：如何获取SD卡指定位置.mp3音乐文件
   
       - ListView适配器的数据填充

       - MediaPlayer基本用法：播放、暂停、继续和停止

   - ListView自定义布局

     - 主要步骤：

       - 新建Fruit类
       - 自定义ListView布局
       - 自定义Adapter
       - 使用Adapter
   
     - 具体代码：
   
       <img src="Android 应用开发.assets/image-20230523113234004-1684831363613173.png" alt="image-20230523113234004" style="zoom: 50%;" />
   
       <img src="Android 应用开发.assets/image-20230523113253538-1684831363613174.png" alt="image-20230523113253538" style="zoom:50%;" />
   
       <img src="Android 应用开发.assets/image-20230523113331927-1684831363613176.png" alt="image-20230523113331927" style="zoom:50%;" />
   
       <img src="Android 应用开发.assets/image-20230523113344189-1684831363613175.png" alt="image-20230523113344189" style="zoom:50%;" />
   
       <img src="Android 应用开发.assets/image-20230523113353715-1684831363613177.png" alt="image-20230523113353715" style="zoom:50%;" />

### 3 界面布局

1. 6种界面布局：
   - 相对布局	RelativeLayout
   - 线性布局    LinearLayout
   - 框架布局    FrameLayout
   - 表格布局    TableLayout
   - 网格布局    GridLayout
   - 绝对布局    AbsoluteLayout

2. RelativeLayout
   - 相对布局是一种非常灵活的布局方式；
   - 通过指定界面元素与其他元素的相对位置关系，来确定界面中所有元素的布局位置；
   - **优点：能够最大程度保证在各种屏幕尺寸的手机上正确显示界面布局。**

3. LinearLayout  ★★★

   - 线性布局是常用的一种布局方式；

   - 通过设置android:orientation来确定是垂直还是水平布局
     - android:orientation="vertical"	垂直布局
     - android:orientation="horizontal"	水平布局 （默认）

   - 垂直布局时：每行仅包含一个界面元素

   - 水平布局时：所有界面元素都在一行

     <img src="Android 应用开发.assets/image-20230524092243617.png" alt="image-20230524092243617" style="zoom: 67%;" />

4. FrameLayout
   - FrameLayout中所有界面对象都是从屏幕的左上角(0,0)开始布局，不能指定位置；

   - 多个组件层叠排列，上一层的会覆盖下一层的控件；

   - 第一个添加的组件放到最底层，最后添加的显示在最上面。

     <img src="Android 应用开发.assets/image-20230524092259269.png" alt="image-20230524092259269" style="zoom:67%;" />

5. TableLayout
   - 表格布局以行列的形式管理子元素，每一行是一个TableRow布局对象，当然也可以是普通的View对象；

   - TableRow表现为一个水平LinearLayout，里面的元素会 水平排列。

     <img src="Android 应用开发.assets/image-20230524092314409.png" alt="image-20230524092314409" style="zoom: 67%;" />

6. GridLayout
   -  Android4.0新支持的布局；

   - 网格布局将用户界面划分为网格，界面元素可随意摆放在网格中，网格布局比表格布局更灵活；

   - 在网格布局中界面元素可以占用多个网格。

     <img src="Android 应用开发.assets/image-20230524092357904.png" alt="image-20230524092357904" style="zoom: 50%;" />

7. AbsoluteLayout
   - 绝对布局能通过指定界面元素的坐标位置，来确定用户界面的整体布局。**已经不推荐使用。**
   - 绝对布局不能够根据不同屏幕对界面元素进行位置调整。

### 4 界面菜单

#### 选项菜单OptionsMenu

1. 概述

   - Android手机上有个Menu键，当Menu按下的时候，在屏幕底部弹出一个菜单，这个菜单就叫OptionsMenu。
   - 每个Activity有且只有一个OptionsMenu，它为整个Activity服务。

2. OptionsMenu基本用法

   - 新建菜单资源文件并添加菜单项

     - 新建res/menu/main.xml

     - 添加菜单项描述：

       \<item android:id="@+id/id名称"	android:title="菜单名称">\</item>

     - 菜单资源示例：

       <img src="Android 应用开发.assets/image-20230523174346232.png" alt="image-20230523174346232" style="zoom: 67%;" />

   - 创建选项菜单：

     - 在Activity中覆盖onCreateOptionsMenu()方法

       <img src="Android 应用开发.assets/image-20230523202810868.png" alt="image-20230523202810868" style="zoom: 80%;" />

   - 添加选择菜单项事件：

     - 在Activity中覆盖onOptionsItemSelected()方法

       <img src="Android 应用开发.assets/image-20230523203405062.png" alt="image-20230523203405062" style="zoom: 80%;" />

#### 上下文菜单ContextMenu

1. 概述：

   - 上下文菜单类似Windows中的鼠标右键弹出菜单；

   - 上下文菜单一般是通过长按（大约2秒）屏幕弹出。

2. 上下文菜单的基本用法

   - 新建菜单资源并添加菜单项描述

   - 创建上下文菜单

     在Activity中覆盖onCreateContextMenu()方法

     <img src="Android 应用开发.assets/image-20230523203733932.png" alt="image-20230523203733932" style="zoom:80%;" />

   - 添加选择菜单项事件：

     在Activity中覆盖onContextItemSelected()方法

     <img src="Android 应用开发.assets/image-20230523204940369.png" alt="image-20230523204940369" style="zoom: 80%;" />

   - 将上下文菜单注册到某个View组件上的：

     <img src="Android 应用开发.assets/image-20230523205700162.png" alt="image-20230523205700162" style="zoom: 80%;" />

#### 子菜单

- Android系统使用浮动窗体的形式显示菜单子项，可以更好适应小屏幕的显示方式。

- 创建子菜单的方法：

  - 子菜单：在菜单项\<menu>\</menu>中嵌套\<item>即可

    <img src="Android 应用开发.assets/image-20230523205916564.png" alt="image-20230523205916564" style="zoom:80%;" />

### 5 事件响应

> 以Button按钮单击事件总结一下Android事件响应方式。

1. 方式1：匿名接口实现（常规）

   <img src="Android 应用开发.assets/image-20230523210217332.png" alt="image-20230523210217332" style="zoom:80%;" />

2. 方式2：内部类实现接口

   <img src="Android 应用开发.assets/image-20230523212706166.png" alt="image-20230523212706166" style="zoom:80%;" />

3. 方式3：绑定监听器 （也较常用）

   <img src="Android 应用开发.assets/image-20230523212834295.png" alt="image-20230523212834295" style="zoom:80%;" />

   - 示例：多个按钮注册到同一个事件的监听器上

     <img src="Android 应用开发.assets/image-20230523213331392.png" alt="image-20230523213331392" style="zoom:80%;" />

4. 方式4：activity实现接口

   <img src="Android 应用开发.assets/image-20230523213413868.png" alt="image-20230523213413868" style="zoom:80%;" />

5. 方式5：在组件的属性中绑定事件(紧耦合不推荐)

   <img src="Android 应用开发.assets/image-20230523213554432.png" alt="image-20230523213554432" style="zoom:80%;" />

### 6 附录

#### 附录1：动态添加菜单（以上下文菜单为例）

<img src="Android 应用开发.assets/image-20230523214408714.png" alt="image-20230523214408714" style="zoom:80%;" />

#### 附录2：Android剪贴板用法

<img src="Android 应用开发.assets/image-20230523214434675.png" alt="image-20230523214434675" style="zoom:80%;" />

#### 附录3：类关系图

<img src="Android 应用开发.assets/image-20230523214453689.png" alt="image-20230523214453689" style="zoom:80%;" />

#### 附录4：AlertDialog用法详解

1. 确定取消对话框

   <img src="Android 应用开发.assets/image-20230523214554617.png" alt="image-20230523214554617" style="zoom:80%;" />

2. 多个按钮信息框

   <img src="Android 应用开发.assets/image-20230523214611859.png" alt="image-20230523214611859" style="zoom:80%;" />

3. 单选列表框对话框

   <img src="Android 应用开发.assets/image-20230523214626079.png" alt="image-20230523214626079" style="zoom:80%;" />

4. 复选列表框对话框

   <img src="Android 应用开发.assets/image-20230523214709412.png" alt="image-20230523214709412" style="zoom: 70%;" />

5. 自定义布局的对话框

   <img src="Android 应用开发.assets/image-20230523214835585.png" alt="image-20230523214835585" style="zoom:80%;" />

   <img src="Android 应用开发.assets/image-20230523215340469.png" alt="image-20230523215340469" style="zoom: 67%;" />

#### 附录5：AutoCompleteTextView用法

1. 基本XML描述

   <img src="Android 应用开发.assets/image-20230523215432553.png" alt="image-20230523215432553" style="zoom: 50%;" />

2. AutoCompleteTextView示例

   - 在res/values/arrays.xml中定义：

     <img src="Android 应用开发.assets/image-20230523215508645.png" alt="image-20230523215508645" style="zoom: 80%;" />

   - 在MainActivity中添加代码：

     <img src="Android 应用开发.assets/image-20230523215547267.png" alt="image-20230523215547267" style="zoom:80%;" />

#### 附录6：DatePicker用法

1. 基本XML描述

   <img src="Android 应用开发.assets/image-20230523215758162.png" alt="image-20230523215758162" style="zoom:80%;" />

2. 基本用法：

   <img src="Android 应用开发.assets/image-20230523215824695.png" alt="image-20230523215824695" style="zoom:80%;" />

3. DatePicker init()方法

   <img src="Android 应用开发.assets/image-20230523215845414.png" alt="image-20230523215845414" style="zoom:80%;" />

4. DatePickerDialog用法

   - 在界面上放置一个TextView和Button（Button弹出）

     DatePickerDialog来设置日期 TextView用于显示日期

     <img src="Android 应用开发.assets/image-20230523220000430.png" alt="image-20230523220000430" style="zoom: 67%;" />

   - 添加如下代码

     <img src="Android 应用开发.assets/image-20230523220039081.png" alt="image-20230523220039081" style="zoom: 67%;" />

     <img src="Android 应用开发.assets/image-20230523220108131.png" alt="image-20230523220108131" style="zoom: 67%;" />

## 第6章 Intent

### 1 Intent简介

> 解决的问题：如何启动其他的Activity并实现它们之间的单/双向通信

#### Intent简介

1. Intent是一种组件之间消息传递机制，它是一个动作的完整描述：包含了动作产生组件、接收组件和传递的数据信息。

   <img src="Android 应用开发.assets/image-20230523220618321.png" alt="image-20230523220618321" style="zoom:80%;" />

2. Intent主要用途：启动Activity、Service，在Android系统上发布Broadcast消息。

   <img src="Android 应用开发.assets/image-20230523220659742.png" alt="image-20230523220659742" style="zoom:50%;" />

### 2 使用Intent启动Activity【显示与隐式】

#### 显示启动

1. 概念：在Intent中指明要启动的Activity类。

2. 示例：

   <img src="Android 应用开发.assets/image-20230523220755060.png" alt="image-20230523220755060" style="zoom:67%;" />

#### 隐式启动

- 说明：

  - 无需指明具体启动哪一个Activity，而由Android系统根据Intent的动作和数据来决定启动哪一个Activity。

  - 例如：希望启动一个浏览器，却不知道具体应该启动哪一 个Activity，此时则可以使用Intent的隐式启动，由Android 系统决定启动哪一个Activity来接收这个Intent。

  - 隐式启动的可以是Android系统内置的Activity，也可是程序本身的Activity，还可是第三方应用程序的Activity。

1. 隐式启动示例1：启动浏览器打开一个网址

   <img src="Android 应用开发.assets/image-20230523220904456.png" alt="image-20230523220904456" style="zoom:80%;" />

2. 隐式启动示例2：打开播放器播放音乐

   <img src="Android 应用开发.assets/image-20230523221210335.png" alt="image-20230523221210335" style="zoom:80%;" />

   - 播放音乐例子在7.0报错问题的解决：

     - 报错：FileUriExposedException: …exposed beyond app throughIntent.getData()

     - 原因：按照Android N的要求，如果 file://格式的Uri的Intent离开应 用将导致 FileUriExposedException 异常。若要在应用间共享文件， 应发送content://格式的Uri，并授予Uri临时访问权限，实现此方法的 是FileProvider类。

     - 解决方法：不推荐

       <img src="Android 应用开发.assets/image-20230523221445197.png" alt="image-20230523221445197" style="zoom: 80%;" />

   - 常见的Intent动作

     <img src="Android 应用开发.assets/image-20230523221550582.png" alt="image-20230523221550582" style="zoom:80%;" />

     <img src="Android 应用开发.assets/image-20230523221612126.png" alt="image-20230523221612126" style="zoom:80%;" />

3. 隐式启动示例3：程序拨打电话例子

   - 在AndroidManifest.xml添加权限：

     ```xml
     <uses-permission android:name="android.permission.CALL_PHONE" />
     ```

   - 核心程序代码：

     ```java
     Intent intent = new Intent(Intent.ACTION_CALL, Uri.parse("tel:18908643860"));
     startActivity(intent);
     ```

   - 权限申请 (6.0以后)：代码见后

     <img src="Android 应用开发.assets/image-20230523221918156.png" alt="image-20230523221918156" style="zoom:80%;" />

     <img src="Android 应用开发.assets/image-20230523221938312.png" alt="image-20230523221938312" style="zoom:80%;" />

   - 添加v4依赖（在安卓开发中，添加 v4 依赖是为了使用 Android Support Library 中的类和方法。Android Support Library 是一组库，可帮助您在旧版 Android 平台上使用最新 API 功能。v4 库是其中的一个库，它包含了许多有用的类和方法，例如 Fragment、ViewPager、NotificationCompat 等等。），此处方法我觉得不会考

     <img src="Android 应用开发.assets/image-20230523225523104.png" alt="image-20230523225523104" style="zoom:80%;" />

     <img src="Android 应用开发.assets/image-20230523225642391.png" alt="image-20230523225642391" style="zoom:80%;" />

### 3 Activity间数据传递

#### 总体图

<img src="Android 应用开发.assets/image-20230523225921467.png" alt="image-20230523225921467" style="zoom: 80%;" />

#### 单向传递数据

1. A 利用Bundle给 B 传递数据：★★★

   <img src="Android 应用开发.assets/image-20230523230044070.png" alt="image-20230523230044070" style="zoom: 50%;" />

   <img src="Android 应用开发.assets/image-20230523230034718.png" alt="image-20230523230034718" style="zoom:80%;" />

2. 关于Bundle

   - Bundle类是一个存储和管理key-value值对的类，多应用于Activity之间相互传递值。

   - 用法示例：

     <img src="Android 应用开发.assets/image-20230523230131776.png" alt="image-20230523230131776" style="zoom: 80%;" />

   - Bundle类的一些方法：

     > clear()：清除此Bundle映射中的所有保存的数据。
     >
     > clone()：克隆当前Bundle
     >
     > containsKey(String key)：返回指定key的值
     >
     > getString(String key)：返回指定key的字符
     >
     > isEmpty()：如果这个捆绑映射为空，则返回true
     >
     > putString(String key, String value):插入一个给定key的字符串值
     >
     > remove(String key)：移除指定key的值

3. Intent类的putExtras()、getExtras()方法

   - putExtras(Bundle)：往Intent中添加一个Bundle对象
   - getExtras()：从Intent中取出Bundle对象

4. 单向传递数据示例1 -- 传递普通数据

<img src="Android 应用开发.assets/image-20230523230711647.png" alt="image-20230523230711647" style="zoom:80%;" />

<img src="Android 应用开发.assets/image-20230523230726499.png" alt="image-20230523230726499" style="zoom:80%;" />

5. 单向传递数据示例2 -- 传递对象数据

   - 针对前例，如何用一个user对象传递数据：

     <img src="Android 应用开发.assets/image-20230523230828100.png" alt="image-20230523230828100" style="zoom:80%;" />

   - 关键点在于Bundle如何传递对象数据。

   - Bundle可以传递对象，但前提是这个**对象需要序列化**。

     > 什么是序列化：序列化是一种用来处理对象流的机制， 以解决如网络传播、磁盘读写等对对象流读写操作时所引发的问题。

   - Bundle的putSerializable()方法，可以存储已经序列化的对象数据(仍然是Key-Value形式)；

   - 接收数据时Bundle用getSerializable()方法，获得数据需要强制转化一下原来的对象类型。

   - 具体过程如下：

     （1）先将User类序列化(直接实现Serializable接口即可)

     <img src="Android 应用开发.assets/image-20230523231057425.png" alt="image-20230523231057425" style="zoom:80%;" />

     （2）发送端：RegisterActivity主要代码

     <img src="Android 应用开发.assets/image-20230523231117861.png" alt="image-20230523231117861" style="zoom:80%;" />

     （3）接收端：WelcomeActivity主要代码

     <img src="Android 应用开发.assets/image-20230523231249223.png" alt="image-20230523231249223" style="zoom:80%;" />

#### 双向传递数据

1. 总图

   <img src="Android 应用开发.assets/image-20230523231358057.png" alt="image-20230523231358057" style="zoom:80%;" />

2. A ↔ B 工作原理

<img src="Android 应用开发.assets/image-20230523232344449.png" alt="image-20230523232344449" style="zoom: 67%;" />

3. B 返回数据给 A 的核心代码框架 ★★★

<img src="Android 应用开发.assets/image-20230523232911832.png" alt="image-20230523232911832" style="zoom:80%;" />

<img src="Android 应用开发.assets/image-20230523232943961.png" alt="image-20230523232943961" style="zoom:80%;" />

4. 双向传递数据示例1：A_Activity输入两个数，B_Activity求和并返回值

   <img src="Android 应用开发.assets/image-20230523233316859.png" alt="image-20230523233316859" style="zoom:80%;" />

   - A_Activity主要代码1

     <img src="Android 应用开发.assets/image-20230523233349370.png" alt="image-20230523233349370" style="zoom:80%;" />

   - B_Activity主要代码

     <img src="Android 应用开发.assets/image-20230523233415369.png" alt="image-20230523233415369" style="zoom:80%;" />

   - A_Activity主要代码2 -- 关键事件onActivityResult

     <img src="Android 应用开发.assets/image-20230523233434913.png" alt="image-20230523233434913" style="zoom:80%;" />

5. 双向传递数据示例2 -- 将DatePicker控件单独作为一个activity（自学）

   <img src="Android 应用开发.assets/image-20230523233507199.png" alt="image-20230523233507199" style="zoom:80%;" />

   - MainActivity主要代码1

     <img src="Android 应用开发.assets/image-20230523233559047.png" alt="image-20230523233559047" style="zoom:80%;" />

   - MainActivity主要代码2

     <img src="Android 应用开发.assets/image-20230523233615222.png" alt="image-20230523233615222" style="zoom:80%;" />

   - CalendarActivity主要代码1

     <img src="Android 应用开发.assets/image-20230523233652555.png" alt="image-20230523233652555" style="zoom:80%;" />

   - CalendarActivity主要代码2

     <img src="Android 应用开发.assets/image-20230523233710202.png" alt="image-20230523233710202" style="zoom:80%;" />

## 第7章 Android广播机制

### 1 什么是广播

#### 概述

1. 在Android中，广播（Broadcast）是一种广泛运用在应用程序之间传输信息的机制。
2. 广播消息实质就是将一个Intent对象用sendBroadcast方法 发送出去 (详见7.3节自定义广播)。
3. 两种类型广播：
   - 系统广播：如系统启动完成了、拨打电话了、收到短信了、 手机没电了等系统发送的消息
   - 自定义广播：将自定义的消息广播给应用程序

#### BroadcastReceiver

1. BroadcastReceiver是对发送出来的Broadcast进行过滤并响应的一类组件，是Android系统的四大组件之一。

2. BroadcastReceiver一般要**在AndroidManifest.xml中注册**

   <img src="Android 应用开发.assets/image-20230524000054038.png" alt="image-20230524000054038" style="zoom: 67%;" />

### 2 系统广播

#### 概述

1. 系统广播就是由Android系统发出的广播。
2. 如系统启动完成了、系统关闭了、拨打电话了、收到短信 了、手机没电了、新安装了一个应用程序、在耳机口上插 入耳机、设备内存不足、屏幕关闭等等。

#### 系统广播示例1：手机关机时播放关机音乐

1. 准备工作：新建一个新Module（名为TestBroadcast）

   - 在res文件夹下新建raw文件夹

   - 将音乐文件复制到raw文件夹中

     <img src="Android 应用开发.assets/image-20230524000219653.png" alt="image-20230524000219653" style="zoom:80%;" />

2. 创建广播接收器BroadcastReceiver

   <img src="Android 应用开发.assets/image-20230524000316179.png" alt="image-20230524000316179" style="zoom: 60%;" />

   - 生成的MyReceiver.java代码框架：

     <img src="Android 应用开发.assets/image-20230524000435995.png" alt="image-20230524000435995" style="zoom:80%;" />

3. 处理广播：在onReceive添加处理代码：

   <img src="Android 应用开发.assets/image-20230524000504211.png" alt="image-20230524000504211" style="zoom:80%;" />

4. 注册接收器：（静态注册）

   > 注册：告诉系统这个BroadcastReceiver要接收哪种广播消息（Intent-filter）

   在AndroidManifest.xml文件中添加注册信息（红色部分）

   <img src="Android 应用开发.assets/image-20230524001107139.png" alt="image-20230524001107139" style="zoom:60%;" />

5. 权限设置：有的广播需要权限支持，**本例不需要**。

   如需添加权限，则在AndroidManifest.xml中添加，例如：

   <img src="Android 应用开发.assets/image-20230524001154271.png" alt="image-20230524001154271" style="zoom: 80%;" />

6. 测试运行： 先运行一下MainActivity，将app安装到手机中(告诉系统有BroadcastReceiver)；然后再锁屏，就有音乐放了。

7. 部分系统广播

   <img src="Android 应用开发.assets/image-20230524001503090.png" alt="image-20230524001503090" style="zoom:80%;" />

#### 示例2：监听拨打电话（去电）

1. 给Receiver添加接收拨打电话的广播

   <img src="Android 应用开发.assets/image-20230524001539020.png" alt="image-20230524001539020" style="zoom:80%;" />

2. 给app添加处理拨打电话的权限

   ```xml
   <uses-permission android:name="android.permission.PROCESS_OUTGOING_CALLS"/>
   ```

3. 补充：AS 6.0以后的模拟器

   <img src="Android 应用开发.assets/image-20230524001757288.png" alt="image-20230524001757288" style="zoom:80%;" />

4. 修改Receiver代码

   <img src="Android 应用开发.assets/image-20230524001905404.png" alt="image-20230524001905404" style="zoom: 67%;" />

### 3 自定义广播

#### 主要步骤

1. 发送广播消息

   在广播发送端(如某个APP的Activity)，把信息装入一个Intent对象(含一个自定义的消息标识串，值任意，能保证唯一性即可)， 然后sendBroadcast(intent)把 Intent 广播出去。

   <img src="Android 应用开发.assets/image-20230524002120847.png" alt="image-20230524002120847" style="zoom:67%;" />

2. 接收广播

   接收端创建Broadcast Receiver

   - 静态注册：在AndroidManifest.xml中注册Receiver，添加intent-filter

     <img src="Android 应用开发.assets/image-20230524002227995.png" alt="image-20230524002227995" style="zoom:67%;" />

   - 动态注册：代码实现注册

     <img src="Android 应用开发.assets/image-20230524002242248.png" alt="image-20230524002242248" style="zoom:67%;" />

3. 接收端处理广播消息：onReceive()方法

   <img src="Android 应用开发.assets/image-20230524002347750.png" alt="image-20230524002347750" style="zoom:67%;" />

4. 关于消息广播标识串：

   - 消息广播标识串，**自定义值，具有唯一性即可**

   - 这个标识在后面有3个地方要用：

     - 发送广播时：用于创建Intent对象，Intent intent = new Intent("广播标识串");
     - manifest中注册时用：作为BroadcastReceiver的intent-filter的action name值
     - 处理消息时：在onReceive()方法中作为消息类型的判断用

   - 通常也可以把标识串定义为符号常量，如：

     ```java
     final String BROADCAST_ACTION_NAME = “如 wust.zz.mybroadcast";
     ```

#### 自定义广播示例（静态注册）

1. 总图

   <img src="Android 应用开发.assets/image-20230524002624032.png" alt="image-20230524002624032" style="zoom:80%;" />

2. 发送端app：MainActivity主要代码

   <img src="Android 应用开发.assets/image-20230524002640515.png" alt="image-20230524002640515" style="zoom:80%;" />

3. 接收端app：MyReceiver主要代码

   <img src="Android 应用开发.assets/image-20230524002700764.png" alt="image-20230524002700764" style="zoom:80%;" />

4. MyReceiver配置 （静态注册）

   AndroidManifest.xml文件：红色部分

   <img src="Android 应用开发.assets/image-20230524002733368.png" alt="image-20230524002733368" style="zoom:80%;" />

5. 补充：Android8.0自定义广播无法接收问题解决

   - 方法1：使用动态广播代替静态广播

   - 方法2：保留原来的静态广播，但intent但加入Component参数：

     <img src="Android 应用开发.assets/image-20230524002834501.png" alt="image-20230524002834501" style="zoom:80%;" />

### 附录：短信黑名单示例

#### 概述

1. 监听SMS短信信息（SMS，Short Messaging Service）

   - 如果对方发送的短信包含“@echo”串，就自动回发“denyyou!”信息，并将该号码列为黑名单

2. BroadcastReceiver采用动态注册

   <img src="Android 应用开发.assets/image-20230524003122649.png" alt="image-20230524003122649" style="zoom: 50%;" />

#### MainActivity界面（activity_main.xml）

```xml
<ToggleButton
    android:id="@+id/toggleButton1" android:layout_width="wrap_content" 
    android:layout_height="wrap_content" 
    android:layout_alignParentLeft="true" 
    android:layout_alignParentTop="true" 
    android:layout_marginLeft="74dp" 
    android:layout_marginTop="44dp"
    android:text="监控SMS" 
    android:textOff="SMS监控已关闭" 
    android:textOn="SMS监控已开启" />
```

#### SMSReceiver代码

<img src="Android 应用开发.assets/image-20230524003441180.png" alt="image-20230524003441180" style="zoom: 67%;" />

<img src="Android 应用开发.assets/image-20230524003456563.png" alt="image-20230524003456563" style="zoom:67%;" />

#### 获取短信说明

<img src="Android 应用开发.assets/image-20230524003530986.png" alt="image-20230524003530986" style="zoom: 67%;" />

#### AndroidManifest.xml添加SMS权限

```xml
<uses-permission android:name="android.permission.RECEIVE_SMS" />
<uses-permission android:name="android.permission.READ_SMS" />
<uses-permission android:name="android.permission.SEND_SMS" />
```

#### MainActivity主要代码

<img src="Android 应用开发.assets/image-20230524003622369.png" alt="image-20230524003622369" style="zoom: 67%;" />

## 第8章 Service

### 1 什么是Service

1. Service是Android系统四大组件之一，它是一种长生命周期的，没有可视化界面，运行于后台的一种服务程序。

   <img src="Android 应用开发.assets/image-20230524003756058.png" alt="image-20230524003756058" style="zoom:50%;" />

### 2 Service类型

#### Started启动的

1. Started启动的：常用于应用程序内部

2. Started形式是指当一个应用组件(如Activity)通过startService()方法 开启的服务。一旦开启，该服务就可以无限期地在后台运行，哪怕开启它的组件被销毁。

3. 基本特点：

   - 在应用程序中定义service组件。

   - 服务通过调用startService(intent)启动，stopService(intent)结束。

   - 在服务内部可以调用stopSelf() 来自己停止。

   - 通常,开启的服务执行一个单独的操作且并不向调用者返回一个结果。

   - 无论调用了多少次startService()，只需调用一次stopService()来停止。

#### Bound绑定的

1. Bound绑定的：用于应用程序之间
2. Bound形式是指一个应用组件通过调用 bindService() 方法与服务绑定。调用 unbindService()关闭连接。
3. Bound形式的服务一旦启用，调用者就与服务绑定在一起，调用者一旦退出，服务也就终止。
4. 多个组件可以同时绑定到一个服务，但当全部绑定解除后，服务就被销毁

#### Service生命周期

<img src="Android 应用开发.assets/image-20230524004359260.png" alt="image-20230524004359260" style="zoom: 67%;" />

### 3 本地服务示例

#### 概述

1. 示例图

   <img src="Android 应用开发.assets/image-20230524004459973.png" alt="image-20230524004459973" style="zoom:50%;" />

2. 主要步骤：

   - 准备工作
   - 创建MusicService
   - 主程序
   - 功能改进

#### 准备工作

1. 在res文件夹下新建raw文件夹

2. 将音乐文件复制到raw文件夹中

   <img src="Android 应用开发.assets/image-20230524004556330.png" alt="image-20230524004556330" style="zoom:50%;" />

#### 创建MusicService

<img src="Android 应用开发.assets/image-20230524004619386.png" alt="image-20230524004619386" style="zoom:67%;" />

- MusicService代码框架

  <img src="Android 应用开发.assets/image-20230524004704822.png" alt="image-20230524004704822" style="zoom:60%;" />

- MusicService代码

  <img src="Android 应用开发.assets/image-20230524004824291.png" alt="image-20230524004824291" style="zoom: 67%;" />

#### 主程序(主要代码)

<img src="Android 应用开发.assets/image-20230524004853476.png" alt="image-20230524004853476" style="zoom: 60%;" />

#### 功能改进（自学）

- 增加暂停、重播、结束(停止服务)功能

  <img src="Android 应用开发.assets/image-20230524005011423.png" alt="image-20230524005011423" style="zoom: 50%;" />

- 思路：通过在Intent传递参数值给Service

  - 假设参数值：
    - 1 -- 播放
    - 2 -- 暂停
    - 3 -- 重播
    - 0 -- 结束(内部结束服务)
    - -1 -- 退出(外部结束服务和Activity)

- 主界面activity_main.xml

  <img src="Android 应用开发.assets/image-20230524005045959.png" alt="image-20230524005045959" style="zoom:67%;" />

- MainActivity主要代码

  <img src="Android 应用开发.assets/image-20230524005102406.png" alt="image-20230524005102406" style="zoom: 67%;" />

  <img src="Android 应用开发.assets/image-20230524005120549.png" alt="image-20230524005120549" style="zoom: 67%;" />

  <img src="Android 应用开发.assets/image-20230524005152005.png" alt="image-20230524005152005" style="zoom:67%;" />

- MusicService主要代码

  <img src="Android 应用开发.assets/image-20230524005218900.png" alt="image-20230524005218900" style="zoom:67%;" />

  <img src="Android 应用开发.assets/image-20230524005228814.png" alt="image-20230524005228814" style="zoom:67%;" />

### 4 广播交互

- 在Activity中点击Button后启动Service

  ```java
  public void onClick(View v) {
      Intent intent = new Intent(this, CounterService.class);
      intent.putExtra("counter", counter); //counter用来计数
      startService(intent);
  }
  ```

- CounterService.java

  ```java
  public class CounterService extends Service { 
      int counter; 
      @Override
      public IBinder onBind(Intent intent) {
          // TODO Auto-generated method stub
          return null;
      } 
      @Override
      public int onStartCommand(Intent intent, int flags, int startId) {
          // TODO Auto-generated method stub
          counter = intent.getIntExtra("counter", 0);
          new Timer().schedule(new TimerTask() {
              @Override
              public void run() {
                  // TODO Auto-generated method stub
                  Intent counterIntent = new Intent();
                  counterIntent.putExtra("counter", counter);
                  counterIntent.setAction("com.example.counter.COUNTER_ACTION");
                  sendBroadcast(counterIntent);
                  counter++;
              }
          }
          return START_STICKY;
      } 
  }
  ```

- 在Service的onStartCommand()中启动一个定时器，每隔1秒钟counter计数加1，通过广播发送将counter发送出去，在Activity中的CounterReceiver收到广播后取出counter，将counter通过handler 发送出去

  ```java
  // 广播接收器，定义在Activity中 
  class CounterReceiver extends BroadcastReceiver {
  	Handler handler;
  	CounterReceiver(Handler handler){
  		this. handler = handler;
  	}
      @Override
      public void onReceive(Context context, Intent intent) {
          counter = intent.getIntExtra("counter", 0);
  	    handler.sendEmptyMessage(counter);
      }
  }
  //运行程序，点击按钮开始计时
  ```

- 在Activity中通过Handler设置到Button上，Button要设计为Activity的全局量, 并在onCreat中初始化：

  ```java
  CounterReceiver receiver;
  Button start;
  Handler handler = new Handler(){
  	 @Override
  	 public void handleMessage(Message msg){
  		start. setText(String.valueOf (msg.what)) 
  		super.handleMessage(msg); 
  	} 
  }
  ```

- 在Activity的onCreat中

  ```java
  Receiver = new CounterReceiver(handler)
  start = (Button)findViewById(R.id.start)
  ```

- 示例图：

  <img src="Android 应用开发.assets/image-20230524010953805.png" alt="image-20230524010953805" style="zoom: 33%;" />

- 在程序运行过程遇到一个问题，在这里说明一下

  - 广播类是在Activity里定义的，是Activity的内部类，这个内部类在使用静态注册的时候，会发生程序运行崩溃，原因是内部广播类如果使用静态注册，必须是静态内部类。
  - 但是如果是静态内部类，只能访问外部类的静态成员变量，所以内部广播类推荐使用动态注册方式。
  - 而且这类广播一般只在程序内部使用，没有必须在进程结束以后继续接收广播。

- 通过广播实现Activity和Service的交互简单容易实现。

  - 缺点是发送不广播受系统制约，系统会优先发送系统级的广播，自定义的广播接收器可能会有延迟。
  - 在广播里也不能有耗时操作，否则会导致程序无响应。

- Activity中动态注册receiver ，如放在onCreat中：

  ```java
  IntentFilter filter = new IntentFilter();
  filter.addAction("com.example.counter.COUNTER_ACTION ");
  BroadcastReceiver receiver = new MyReceiver();
  registerReceiver( receiver , filter);
  ```

- 此外Activity销毁的时候，注销(关闭)广播：

  ```java
  unregisterReceiver(receiver);
  ```

- 进一步学习：有关远程服务的创建和调用

  > 参考：
  >
  > - http://www.apkbus.com/android-131250-1-1.html
  > - http://liangruijun.blog.51cto.com/3061169/653344/

## 第9章 1+简单数据存储和访问（简单）（SharedPreference）

### 0 简单数据存储

1. 应用程序一般允许用户自己定义配置信息，如界面背景颜色、字体大小和字体颜色等。
2. 使用SharedPreferences保存用户的自定义配置信息，并在程序启动时自动加载这些自定义的配置信息。

### 1 简单存储

#### 概述

1. 用户在界面上的输入的信息，在Activity关闭时进行保存。当应用程序重新开启时，保存信息将被读取出来，并重新呈现在用户界面上

#### SharedPreferences

1. SharedPreferences是一种轻量级的数据保存方式。

   - 通过SharedPreferences可以将NVP（Name/Value Pair，名称/值对）保存在Android的文件系统中，而且SharedPreferences完全屏蔽的对文件系统的操作过程。
   - 开发人员仅是通过调用SharedPreferences对NVP进行保存和读取。

2. SharedPreferences不仅能够保存数据，还能够实现不同应用程序间的数据共享

   - SharedPreferences支持三种访问模式
     - 私有（MODE_PRIVATE）：仅有创建程序有权限对其进行读取或写入。
     - 全局读（MODE_WORLD_READABLE）：不仅创建程序可以对其进行读取或写入，其他应用程序也读取操作的权限，但没有写入操作的权限。
     - 全局写（MODE_WORLD_WRITEABLE）：创建程序和其他程序都可以对其进行写入操作，但没有读取的权限。

3. 在使用SharedPreferences前，先定义SharedPreferences的访问模式。

   - 下面的代码将访问模式定义为私有模式：

     ```java
     public static int MODE = MODE_PRIVATE;
     ```

   - 有的时候需要将SharedPreferences的访问模式设定为即可以全局读，也可以全局写，这样就需要将两种模式写成下面的方式：

     ```java
     public static int MODE = Context.MODE_WORLD_READABLE + Context.MODE_WORLD_WRITEABLE;
     ```

4. 定义SharedPreferences的名称

   - 这个名称与在Android文件系统中保存的文件同名。因此，只要具有相同的SharedPreferences名称的NVP内容，都会保存在同一个文件中

     ```java
     public static final String PREFERENCE_NAME = "SaveSetting";
     ```

   - 为了可以使用SharedPreferences，需要将访问模式和SharedPreferences名称作为参数，传递到getSharedPreferences()函数，并获取到SharedPreferences对象

     ```java
     SharedPreferences sharedPreferences = getSharedPreferences(PREFERENCE_NAME, MODE);
     ```

5. 修改SharedPreferences

   - 在获取到SharedPreferences对象后，则可以通过SharedPreferences.Editor类对SharedPreferences进行修改，最后调用commit()函数保存修改内容。

   - SharedPreferences广泛支持各种基本数据类型，包括整型、布尔型、浮点型和长型等等。

     ```java
     SharedPreferences.Editor editor = sharedPreferences.edit();
     editor.putString("Name", "Tom");
     editor.putInt("Age", 20);
     editor.putFloat("Height", 1.67);	//这里PPT没有float的内容，猜测是这样
     editor.commit();
     ```

6. 读取SharedPreferences

   - 如果需要从已经保存的SharedPreferences中读取数据，同样是调用getSharedPreferences()函数，并在函数的第1个参数中指明需要访问的SharedPreferences名称，最后通过get\<Type>()函数获取保存在SharedPreferences中的NVP。

     ```java
     SharedPreferences sharedPreferences = getSharedPreferences(PREFERENCE_NAME, MODE);
     String name = sharedPreferences.getString("Name","Default Name");
     int age = sharedPreferences.getInt("Age", 20);
     float height = sharedPreferences.getFloat("Height",);
     ```

     - get\<Type>()函数的第1个参数是NVP的名称。
     - 第2个参数是在无法获取到数值的时候使用的缺省值。

#### 示例

> 通过SimplePreferenceDemo示例介绍具体说明SharedPreferences的文件保存位置和保存格式。

1. 下图是SimplePreferenceDemo示例的用户界面

   用户在界面上的输入的信息，将通过SharedPreferences在Activity关闭时进行保存。当应用程序重新开启时，保存在SharedPreferences的信息将被读取出来，并重新呈现在用户界面上。

   <img src="Android 应用开发.assets/image-20230524102128850.png" alt="image-20230524102128850" style="zoom:50%;" />

2. 文件信息

   - SimplePreferenceDemo示例运行后，通过FileExplorer查看/data/data下的数据，Android为每个应用程序建立了与包同名的目录，用来保存应用程序产生的数据，这些数据包括文件、SharedPreferences文件和数据库等。
   - SharedPreferences文件就保存在/data/data/\<package name>/shared_prefs目录下。

   - 在本示例中，shared_prefs目录下生成了一个名为SaveSetting.xml的文件：

     <img src="Android 应用开发.assets/image-20230524102435288.png" alt="image-20230524102435288" style="zoom: 50%;" />

     - 这个文件就是保存SharedPreferences的文件，文件大小为170字节，在Linux下的权限为“-rw-rw-rw”

       > 在Linux系统中，文件权限分别描述了创建者、同组用户和其他用户对文件的操作限制。
       >
       > - x表示可执行，r表示可读，w表示可写，d表示目录，-表示普通文件。因此，“-rw-rw-rw”表示SaveSetting.xml可以被创建者、同组用户和其他用户进行读取和写入操作，但不可执行。
       > - 产生这样的文件权限与程序人员设定的SharedPreferences的访问模式有关，“-rw-rw-rw”的权限是“全局读+全局写”的结果。
       > - 如果将SharedPreferences的访问模式设置为私有，则文件权限将成为“-rw-rw ---”，表示仅有创建者和同组用户具有读写文件的权限。

   - SaveSetting.xml文件是以XML格式保存的信息，内容如图如下

     ```xml
     <?xml version='1.0' encoding='utf-8' standalone='yes' ?>
     <map>
     	<float name="Height" value="1.81" />
     	<string name="Name">Tom</string>
     	<int name="Age" value="20" />
     </map>
     ```

3. 具体功能与相关代码

   - SimplePreferenceDemo示例在onStart()函数中调用loadSharedPreferences()函数，读取保存在SharedPreferences中的姓名、年龄和身高信息，并显示在用户界面上。

     - 当Activity关闭时，在onStop()函数调用saveSharedPreferences()，保存界面上的信息

     - SimplePreferenceDemo.java的完整代码：

       ```java
       package edu.hrbeu.SimplePreferenceDemo;
        
       import android.app.Activity;
       import android.content.Context;
       import android.content.SharedPreferences;
       import android.os.Bundle;
       import android.widget.EditText;
        
       public class SimplePreferenceDemo extends Activity {
       	
       	private EditText nameText;
       	private EditText ageText;
       	private EditText heightText;
       	public static final String PREFERENCE_NAME = "SaveSetting";
       	public static int MODE = Context.MODE_WORLD_READABLE + Context.MODE_WORLD_WRITEABLE;
       	
       	@Override
       	public void onCreate(Bundle savedInstanceState) {
       		super.onCreate(savedInstanceState);
       		setContentView(R.layout.main);
       		nameText = (EditText)findViewById(R.id.name);
       		ageText = (EditText)findViewById(R.id.age);
       		heightText = (EditText)findViewById(R.id.height);
       	}
          
           @Override
       	public void onStart(){
       		super.onStart();
       		loadSharedPreferences();
       	}
       	@Override
       	public void onStop(){
       		super.onStop();	
       		saveSharedPreferences();
       	}
           
       	private void loadSharedPreferences(){
       	  	SharedPreferences sharedPreferences = getSharedPreferences(PREFERENCE_NAME, MODE);
       		String name = sharedPreferences.getString("Name","Tom");
       		int age = sharedPreferences.getInt("Age", 20);
       		float height = sharedPreferences.getFloat("Height",);
        		nameText.setText(name);
       		ageText.setText(String.valueOf(age));
       		heightText.setText(String.valueOf(height));    	
       	}
           
           private void saveSharedPreferences(){
       		SharedPreferences sharedPreferences = getSharedPreferences(PREFERENCE_NAME, MODE);
       		SharedPreferences.Editor editor = sharedPreferences.edit();
       		    
       		editor.putString("Name", nameText.getText().toString());
       		editor.putInt("Age", Integer.parseInt(ageText.getText().toString()));
       		editor.putFloat("Height", Float.parseFloat(heightText.getText().toString()));
       		editor.commit();
       	}
       }
       ```

   - 示例SharePreferenceDemo将说明如何读取其他应用程序保存的SharedPreferences数据。

     - 下图是SharePreferenceDemo示例的用户界面。

     - 示例将读取SimplePreferenceDemo示例保存的信息，并在程序启动时显示在用户界面上。

       <img src="Android 应用开发.assets/image-20230524103140761.png" alt="image-20230524103140761" style="zoom:40%;" />

   - SharePreferenceDemo示例的核心代码

     <img src="Android 应用开发.assets/image-20230524103217095.png" alt="image-20230524103217095" style="zoom:80%;" />

     <img src="Android 应用开发.assets/image-20230524110121702.png" alt="image-20230524110121702" style="zoom:80%;" />

4. 访问其他应用程序的SharedPreferences必须满足三个条件:

   - 共享者需要将SharedPreferences的访问模式设置为全局读或全局写。
   - 访问者需要知道共享者的包名称和SharedPreferences的名称，以通过Context获得SharedPreferences对象。
   - 访问者需要确切知道每个数据的名称和数据类型，用以正确读取数据。

### 2 文件存储

#### 概述

1. Android使用的是基于Linux的文件系统。
2. 程序开发人员可以建立和访问程序自身的私有文件。
3. 也可以访问保存在资源目录中的原始文件和XML文件。
4. 还可以在SD卡等外部存储设备中保存文件。

#### 内部存储

1. 在内部存储器上进行文件写入和读取

   <img src="Android 应用开发.assets/image-20230524103912679.png" alt="image-20230524103912679" style="zoom:50%;" />

2.  Android系统允许应用程序创建仅能够自身访问的私有文件，文件保存在设备的内部存储器上，在Linux系统下的/data/data/\<package name>/files目录中。

   - Android系统不仅支持标准Java的IO类和方法，还提供了能够简化读写流式文件过程的函数。
   - 主要介绍的两个函数：
     - openFileOutput()
     - openFileInput()

3. openFileOutput()函数

   - openFileOutput()函数为写入数据做准备而打开的应用程序私文件，如果指定的文件不存在，则创建一个新的文件。

   - openFileOutput()函数的语法格式如下：

     ```java
     public FileOutputStream openFileOutput(String name, int mode)
     ```

     - 第1个参数是文件名称，这个参数不可以包含描述路径的斜杠。
     - 第2个参数是操作模式。

   - 函数的返回值是FileOutputStream类型。

   - Android系统支持四种文件操作模式：

     <img src="Android 应用开发.assets/image-20230524104208645.png" alt="image-20230524104208645" style="zoom: 80%;" />

   - 使用openFileOutput()函数建立新文件的示例代码如下：

     ```java
     String FILE_NAME = "fileDemo.txt";  //定义了建立文件的名称fileDemo.txt
     FileOutputStream fos = openFileOutput(FILE_NAME,Context.MODE_PRIVATE)  //使用openFileOutput()函数以私有模式建立文件
     String text = “Some data”;
     fos.write(text.getBytes());  //调用write()函数将数据写入文件
     fos.flush();  //调用flush()函数将所有剩余的数据写入文件
     fos.close();  //调用close()函数关闭FileOutputStream
     ```

     - 为了提高文件系统的性能，一般调用write()函数时，如果写入的数据量较小，系统会把数据保存在数据缓冲区中，等数据量累积到一定程度时再一次性的写入文件中。
     - 由上可知，在调用close()函数关闭文件前，务必要调用flush()函数，将缓冲区内所有的数据写入文件。

4. openFileInput()函数

   - openFileInput()函数为读取数据做准备而打开应用程序私文件。

   - openFileInput()函数的语法格式如下：

     ```java
     public FileInputStream openFileInput (String name)
     ```

     - 第1个参数也是文件名称，同样不允许包含描述路径的斜杠。

   - 使用openFileInput ()函数打开已有文件的示例代码如下：

     ```java
     String FILE_NAME = "fileDemo.txt";
     FileInputStream fis = openFileInput(FILE_NAME);
      
     byte[] readBytes = new byte[fis.available()];
     while(fis.read(readBytes) != -1){
     }
     ```

     - 上面的两部分代码在实际使用过程中会遇到错误提示，因为文件操作可能会遇到各种问题而最终导致操作失败，因此代码应该使用try/catch捕获可能产生的异常。

5. InternalFileDemo示例用来演示在内部存储器上进行文件写入和读取。

   - InternalFileDemo示例用户界面如图：

     <img src="Android 应用开发.assets/image-20230524104916033.png" alt="image-20230524104916033" style="zoom:50%;" />

   - InternalFileDemo示例的核心代码：

     ```java
     OnClickListener writeButtonListener = new OnClickListener() { 
     	@Override
     	public void onClick(View v) {
     		FileOutputStream fos = null;	
     		try {
     		      if (appendBox.isChecked()){
     		           fos = openFileOutput(FILE_NAME,Context.MODE_APPEND);
     	                        }
                                        else {
     		          fos = openFileOutput(FILE_NAME,Context.MODE_PRIVATE);
     		      }
     		String text = entryText.getText().toString();
     		fos.write(text.getBytes());
     		labelView.setText("文件写入成功，写入长度："+text.length());
     		entryText.setText("");
                                   } catch (FileNotFoundException e) {
                                                    e.printStackTrace();
     		}
     		catch (IOException e) {
     			e.printStackTrace();
     		}
     		finally{
     			if (fos != null){
     				try {
     					fos.flush();
     					fos.close();
     				} catch (IOException e) {
     					e.printStackTrace();
     				}
     			}
     		}
     	}
     };
     OnClickListener readButtonListener = new OnClickListener() { 
     @Override
      	public void onClick(View v) {
      		displayView.setText("");
      		FileInputStream fis = null;
      		try {
     		            fis = openFileInput(FILE_NAME);
     		            if (fis.available() == 0){
     			   return;
     	                               }
     		             byte[] readBytes = new byte[fis.available()];
     		             while(fis.read(readBytes) != -1){
     		             }
     		             String text = new String(readBytes);
     		             displayView.setText(text);
     		              labelView.setText("文件读取成功，文件长度："+text.length());
                                   } catch (FileNotFoundException e) {
                                                    e.printStackTrace();
     		}
     		catch (IOException e) {
     			e.printStackTrace();
     		}
     	}
     };
     ```

     - 程序运行后，在/data/data/edu.hrbeu.InternalFileDemo/files/目录下，找到了新建立的fileDemo.txt文件。

     - fileDemo.txt文件：

       <img src="Android 应用开发.assets/image-20230524105441595.png" alt="image-20230524105441595" style="zoom: 50%;" />

       - fileDemo.txt从文件权限上进行分析，“-rw-rw---”表明文件仅允许文件创建者和同组用户读写，其他用户无权使用。
       - 文件的大小为9个字节，保存的数据为“Some data”

#### 外部存储

1. Android的外部存储设备指的是SD卡（Secure Digital Memory Card），是一种广泛使用于数码设备上的记忆卡。不是所有的Android手机都有SD卡，但Android系统提供了对SD卡的便捷的访问方法。

2. SD卡适用于保存大尺寸的文件或者是一些无需设置访问权限的文件，可以保存录制的大容量的视频文件和音频文件等。

   - SD卡使用的是FAT（File Allocation Table）的文件系统，不支持访问模式和权限控制，但可以通过Linux文件系统的文件访问权限的控制保证文件的私密性。
   - Android模拟器支持SD卡，但模拟器中没有缺省的SD卡，开发人员须在模拟器中手工添加SD卡的映像文件。

3. 使用\<Android SDK>/tools目录下的mksdcard工具创建SD卡映像文件，命令如下：

   ```shell
   mksdcard -l SDCARD 256M E:\android\sdcard_file
   ```

   - 第1个参数-1表示后面的字符串是SD卡的标签，这个新建立的SD卡的标签是SDCARD。
   - 第2个参数256M表示SD卡的容量是256兆。
   - 最后一个参数表示SD卡映像文件的保存位置，上面的命令将映像保存在E:\android目录下sdcard_file文件中。在CMD中执行该命令后，则可在所指定的目录中找到生产的SD卡映像文件。

4. 如果希望Android模拟器启动时能够自动加载指定的SD卡，还需要在模拟器的“运行设置”（Run Configurations）中添加SD卡加载命令。

   - SD卡加载命令中只要指明映像文件位置即可。

   - SD卡加载命令：

     <img src="Android 应用开发.assets/image-20230524105723436.png" alt="image-20230524105723436" style="zoom:50%;" />

5. 测试SD卡映像是否正确加载

   - 在模拟器启动后，使用FileExplorer向SD卡中随意上传一个文件，如果文件上传成功，则表明SD卡映像已经成功加载。

   - 向SD卡中成功上传了一个测试文件test.txt，文件显示在/sdcard目录下

     <img src="Android 应用开发.assets/image-20230524105808587.png" alt="image-20230524105808587" style="zoom:50%;" />

6. 编程访问SD卡

   - 首先需要检测系统的/sdcard目录是否可用。
   - 如果不可用，则说明设备中的SD卡已经被移除，在Android模拟器则表明SD卡映像没有被正确加载。
   - 如果可用，则直接通过使用标准的Java.io.File类进行访问。

7. 将数据保存在SD卡

   - 通过“生产随机数列”按钮生产10个随机小数。
   - 通过“写入SD卡”按钮将生产的数据保存在SD卡的目录下。
   - SDcardFileDemo示例说明了如何将数据保存在SD卡。

8. SDcardFileDemo示例

   - 用户界面

     <img src="Android 应用开发.assets/image-20230524110010947.png" alt="image-20230524110010947" style="zoom:50%;" />

   - SDcardFileDemo示例运行后，在每次点击“写入SD卡”按钮后，都会在SD卡中生产一个新文件，文件名各不相同。

     - SD卡中生产的文件：

       <img src="Android 应用开发.assets/image-20230524110044850.png" alt="image-20230524110044850" style="zoom:50%;" />

   - SDcardFileDemo示例的核心代码

     <img src="Android 应用开发.assets/image-20230524110219083.png" alt="image-20230524110219083" style="zoom:80%;" />

     <img src="Android 应用开发.assets/image-20230524110238366.png" alt="image-20230524110238366" style="zoom:80%;" />

#### 资源文件

1. 程序开发人员可以将程序开发阶段已经准备好的原始格式文件和XML文件分别存放在/res/raw和/res/xml目录下，供应用程序在运行时进行访问。

   - 原始格式文件可以是任何格式的文件，例如视频格式文件、音频格式文件、图像文件和数据文件等等，在应用程序编译和打包时，/res/raw目录下的所有文件都会保留原有格式不变。
   - /res/xml目录下的XML文件，一般用来保存格式化的数据，在应用程序编译和打包时会将XML文件转换为高效的二进制格式，应用程序运行时会以特殊的方式进行访问。

2. ResourceFileDemo示例

   - ResourceFileDemo示例演示了如何在程序运行时访问资源文件。

     - 当用户点击“读取原始文件”按钮时，程序将读取/res/raw/raw_file.txt文件，并将内容显示在界面上。

     <img src="Android 应用开发.assets/image-20230524110426768.png" alt="image-20230524110426768" style="zoom:50%;" />

     - 当用户点击“读取XML文件”按钮时，程序将读取/res/xml/people.xml文件，并将内容显示在界面上。

       <img src="Android 应用开发.assets/image-20230524110532679.png" alt="image-20230524110532679" style="zoom:50%;" />

   - 读取原始格式文件，首先需要调用getResource()函数获得资源对象，然后通过调用资源对象的openRawResource()函数，以二进制流的形式打开指定的原始格式文件。在读取文件结束后，调用close()函数关闭文件流。

     - ResourceFileDemo示例中关于读取原始格式文件的核心代码如下：

       ```java
       Resources resources = this.getResources();
       InputStream inputStream = null;
       try {
       	inputStream = resources.openRawResource(R.raw.raw_file);    
       	byte[] reader = new byte[inputStream.available()]; 
       	while (inputStream.read(reader) != -1) { 
       	}
           displayView.setText(new String(reader,"utf-8")); 
       } catch (IOException e) {
       	Log.e("ResourceFileDemo", e.getMessage(), e); 
       } finally {
       	if (inputStream != null) { 
       		try {
       			inputStream.close(); 
       		} catch (IOException e) { } 
       	}
       }
       ```

       - 代码第8行的new String(reader,"utf-8")，表示以UTF-8的编码方式，从字节数组中实例化一个字符串。

     - 程序开发人员需要确定/res/raw/raw_file.txt文件使用的是UTF-8编码方式，否则程序运行时会产生乱码。确认的方法：

       - 右击raw_file.txt文件。

       - 选择“Properties”打开raw_file.txt文件的属性设置框。

       - 在Resource栏下的Text file encoding中，选择“Other：UTF-8”。

         <img src="Android 应用开发.assets/image-20230524111134519.png" alt="image-20230524111134519" style="zoom: 67%;" />

   - /res/xml目录下的XML文件会转换为一种高效的二进制格式

     - 如何在程序运行时读取/res/xml目录下的XML文件

       - 首先在/res/xml目录下创建一个名为people.xml的文件。
       - XML文件定义了多个\<person>元素，每个\<person>元素都包含三个属性name、age和height，分别表示姓名、年龄和身高。

     - /res/xml/people.xml文件代码如下：

       ```xml
       <people> 
       	<person name="李某某" age="21" height="1.81" /> 
       	<person name="王某某" age="25" height="1.76" /> 
       	<person name="张某某" age="20" height="1.69" /> 
       </people>
       ```

   - 读取XML格式文件，首先通过调用资源对象的getXml()函数，获取到XML解析器XmlPullParser。XmlPullParser是Android平台标准的XML解析器，这项技术来自一个开源的XML解析API项目XMLPULL。

     - ResourceFileDemo示例中关于读取XML文件的核心代码如下：

       <img src="Android 应用开发.assets/image-20230524111511617.png" alt="image-20230524111511617" style="zoom:80%;" />

       <img src="Android 应用开发.assets/image-20230524111534940.png" alt="image-20230524111534940" style="zoom:80%;" />

       <img src="Android 应用开发.assets/image-20230524111552026.png" alt="image-20230524111552026" style="zoom:80%;" />

     - XmlPullParser的XML事件类型

       <img src="Android 应用开发.assets/image-20230524111618923.png" alt="image-20230524111618923" style="zoom:80%;" />

## 第9章 2++数据存储和访问(SQLite示例)

<img src="Android 应用开发.assets/image-20230524111841475.png" alt="image-20230524111841475" style="zoom:80%;" />

### 1 ListView显示数据库数据（ShowActivity）

#### ListView 2个关键点

1. 自定义layout布局；

2. 使用SimpleCursorAdapter适配器填充：(用于显示数据库数据)         

   该适配器允许将一个Cursor(查询结果集)的数据列绑定到ListView自定 义布局中的TextView 或 ImageView组件上(详见后面代码)

   <img src="Android 应用开发.assets/image-20230524112403877.png" alt="image-20230524112403877" style="zoom:80%;" />

3. 代码详解：

   - ListView自定义layout布局，新建一个名为listview.xml布局：

     <img src="Android 应用开发.assets/image-20230524112444563.png" alt="image-20230524112444563" style="zoom:80%;" />

   - 使用SimpleCursorAdapter填充ListView，SimpleCursorAdapter基本方法：

     <img src="Android 应用开发.assets/image-20230524112546497.png" alt="image-20230524112546497" style="zoom:80%;" />

   - ShowAcitvity的OnCreate()主要代码：

     <img src="Android 应用开发.assets/image-20230524112839870.png" alt="image-20230524112839870" style="zoom:80%;" />

### 2 添加记录功能（InsertActivity）

<img src="Android 应用开发.assets/image-20230524112956668.png" alt="image-20230524112956668" style="zoom:80%;" />

1. ShowActivity“添加”按钮

   <img src="Android 应用开发.assets/image-20230524113108634.png" alt="image-20230524113108634" style="zoom:80%;" />

2. InsertActivity“保存”按钮

   <img src="Android 应用开发.assets/image-20230524113144450.png" alt="image-20230524113144450" style="zoom:80%;" />

   <img src="Android 应用开发.assets/image-20230524113333413.png" alt="image-20230524113333413" style="zoom:80%;" />

3. ShowActivity接收回传信息（下面代码与onCreate并列）

   <img src="Android 应用开发.assets/image-20230524113435762.png" alt="image-20230524113435762" style="zoom:80%;" />

### 3 给ListView添加ContextMenu菜单：删除+编辑（UpdateActivity）

#### 主要步骤

1. 第1步：新建菜单资源

   - 在res下新建menu目录

   - 在menu目录下新建一个manage.xml文件，代码：

     <img src="Android 应用开发.assets/image-20230524113633187.png" alt="image-20230524113633187" style="zoom:80%;" />

2. 第2步：创建上下文菜单（下面代码放在ShowActivity中，与onCreate并列）

   <img src="Android 应用开发.assets/image-20230524113707414.png" alt="image-20230524113707414" style="zoom:80%;" />

3. 第3步：添加上下文菜单选中项方法（下面代码放在ShowActivity中，与onCreateContextMenu并列）

   <img src="Android 应用开发.assets/image-20230524113945191.png" alt="image-20230524113945191" style="zoom:80%;" />

4. 第4步：将上下文菜单注册到ListView上（完）（下面代码放在ShowActivity中onCreate中）

   <img src="Android 应用开发.assets/image-20230524114028238.png" alt="image-20230524114028238" style="zoom:80%;" />

5. 后续删除、修改操作分析：

   - 两个操作的关键点：

     - **如何得到选中行的id值以及其他字段值（name、age）。** 
     - 删除：根据id值作为删除记录的条件。
     - 修改：将id值及其他字段的值传到UpdateActivity显示和修改。

   - 关键点代码：

     <img src="Android 应用开发.assets/image-20230524165040061.png" alt="image-20230524165040061" style="zoom: 80%;" />

   - 具体代码：

     - 删除操作 delete(MenuItem item) 方法

       <img src="Android 应用开发.assets/image-20230524165116701.png" alt="image-20230524165116701" style="zoom:80%;" />

     - 修改操作

       <img src="Android 应用开发.assets/image-20230524165252238.png" alt="image-20230524165252238" style="zoom:80%;" />

       UpdateActivity：oncreat()代码

       <img src="Android 应用开发.assets/image-20230524165506477.png" alt="image-20230524165506477" style="zoom:80%;" />

       <img src="Android 应用开发.assets/image-20230524165526980.png" alt="image-20230524165526980" style="zoom:80%;" />

       <img src="Android 应用开发.assets/image-20230524165550979.png" alt="image-20230524165550979" style="zoom:80%;" />

     - ShowActivity接收回传信息

       <img src="Android 应用开发.assets/image-20230524165623811.png" alt="image-20230524165623811" style="zoom:80%;" />
