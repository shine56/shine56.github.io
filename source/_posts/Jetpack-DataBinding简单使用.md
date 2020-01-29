---
title: Jetpack--DataBinding简单使用
date: 2020-01-11 19:18:29
edited:
tags: Android学习
categories: Android
---

>数据绑定库(DataBinding)是一种支持库，借助该库，您可以使用声明性格式将布局中的界面组件绑定到应用中的数据源。 --谷歌

意思就是,一些UI组件和应用的一些数据绑定在一起,数据改变<--->UI改变. 无需写代码,达到两者自动同步的效果.DataBinding通常用户MVVM模式在Android的实现.连接View和ViewModel.

### 使用方法
#### 配置

在app的gradel中加入代码

```xml
android{
	...
	dataBinding{
       enabled = true
    }
}
```
<!--more-->


#### 创建数据

```java
public class User{
    private String name;

    public User(String name) {
        this.name = name;
    }
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
}
```
这里定义了了一个User类,里面有一天数据name.

#### 改变布局

打开xml布局文件, 把光标放在父布局(根布局)，然后 **Alt+Enter**，选择**“Convert to data binding layout”** 布局自动转换成DataBinding布局。像这样：

```xml
<?xml version="1.0" encoding="utf-8"?>
<layout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto">
    <data>
        <variable
            name="user"
            type="com.example.databindtest.User" />
    </data>
    <LinearLayout
        android:orientation="vertical"
        android:layout_width="match_parent"
        android:layout_height="match_parent">

        <EditText
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="@{user.name}">
        </EditText>
    </LinearLayout>
</layout>
```
可以看到布局父布局是 layout，它有两个子布局 data 和 一个普通的view布局。

	data布局用于声明数据表变量及其类型。有两种方法

```xml
<data>
    <variable
        name="user"
        type="com.example.databindtest.User" />
</data>
```
第二种，import导入类型，该类型可以被多次使用

```xml
<import type="com.example.databindtest.User"/>
<variable
     name="myUser"
     type="User" />
<variable
     name="user"
     type="User" />
```

第二种还可以用**alias**重命名该类型

```xml
<import type="com.example.databindtest.User"
	alias="myUser"/>
<variable
    name="user"
    type="myUser" />
```
至于原始布局和从前一样，这里我定义了一个**EditText**
#### 绑定UI和数据
这里绑定将**EditText**和**User的name**绑定在一起，EditText加入 **android:text"@{}"** 就行

```xml
<EditText
	android:layout_width="match_parent"
	android:layout_height="wrap_content"
	android:text="@{user.name}">
</EditText>
```
Activity代码

```java

@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    ActivityMainBinding binding = DataBindingUtil.setContentView(this, R.layout.activity_main);
    
    User user = new User("Test");
    binding.setUser(user);
}
```
##### 单向绑定
###### 方式一：BaseObservable
数据表继承**BaseObservable**  用注释 **@Bindable** 绑定数据。
BaseObservable提供了两个刷新数据的方法
* **notifyChange();** //刷新所有值
* **notifyPropertyChanged(BR.name);** 刷新BR指定值

实例代码如下:

```java
public class User extends BaseObservable {
    @Bindable
    private String name;

    public User(String name) {
        this.name = name;
    }
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
        notifyPropertyChanged(BR.name);
    }
}
```
到这里便实现了单向绑定即，数据改变 -> UI改变。

###### 方式二：ObservableField
用BaseObservable绑定要注解，还要notifyPropertyChanged，当数据比较多的时候要写蛮多代码的。所以官方还提供一系列 **ObservableField**。其中包括Observable**Boolean**, Observable**Byte**, Observable**Cha**r, Observable**Shor**t, Observable**Int**, Observable**Long**, Observable**Float** Observable**Double**, 以及 Observable**Parcelable**等。用法：

* 数据表这样写：

```java
public class User{
    public final ObservableField<String> name = new ObservableField<>();
    public final ObservableField<String> imaUrl = new ObservableField<>()};
    public final ObservableInt id = new ObservableInt();
    public final ObservableDouble score = new ObservableDouble();
}
```
* Activity这样写
```java
user = new User();
binding.setUser(user);
user.name.set("初始");
```
##### 双向绑定
想要达到数据改变 ->UI改变，UI改变 -> 数据改变。改变布局的一行代码就行了

android:text="@{}" 改成 android:text="@={}" 

```xml
<EditText
	android:layout_width="match_parent"
	android:layout_height="wrap_content"
	android:text="@={user.name}">
</EditText>
```

#### 点击事件
* 定义点击事件响应类，先在activity写逻辑
```java
public void onClick(View view) {
	switch (view.getId()){
	    case R.id.tv1:
	        Log.d("测试","按钮1");
	        break;
	    case R.id.tv2:
	       Log.d("测试","按钮2");
	       break;
	}
}
```

* data中添加声明

```xml
<data>
	<variable
        name="mainActivity"
        type="com.example.databindtest.MainActivity"/>
</data>
```

* button引用

```xml
<Button
	android:id="@+id/tv1"
    android:onClick="@{mainActivity.onClick}"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="buttom1">
</Button>
<Button
    android:id="@+id/tv2"
    android:onClick="@{mainActivity.onClick}"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="button2">
</Button>
```
* 最后在activity绑定一下就行了

```java
binding.setMainActivity(this);
```

#### 图片的显示
* User数据表添加一条数据 **imaUrl**

```java
public class User extends BaseObservable {
    @Bindable
    private String name;
    private String imaUrl;

    public User(String name, String imaUrl) {
        this.name = name;
        this.imaUrl = imaUrl;
    }
    
    public String getImaUrl() {
        return imaUrl;
    }
    public void setImaUrl(String imaUrl) {
        this.imaUrl = imaUrl;
    }
    
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
        notifyPropertyChanged(BR.name);
    }
}
```

* 写逻辑
需要导入Glide库
```java
@BindingAdapter("app:imaUrl")
public static void loadIma(ImageView imageView, String url){
    Log.d("测试","图片测试+utl:"+url);
    Glide.with(imageView.getContext()).load(url).into(imageView);
}
```
这里通过 **@BindingAdapter** 注解拿到url
* 布局

```xml
<ImageView
	android:layout_width="wrap_content"
	android:layout_height="400dp"
	android:background="@color/colorPrimary"
	app:imaUrl="@{user.imaUrl}"/>
```
看到这里有一个属性 **app:imaUrl=""** 便是上面注解 **@BindingAdapter** 里面的
* 最后在activity绑定一下就行了。如果加载的是网络图片记得给权限

```java
binding.setUser(user);
```
* 更新图片

如果数据表数据变了，图片并不会同步改变。想要更新ImageView需要重新绑定一下。

```java
user.setImaUrl("newImaUrl");
binding.setUser(user);
```
#### 使用RecyclerView
* 数据表 

```java
public class User{
    public final ObservableField<String> name = new ObservableField<>();
    public final ObservableField<String> imaUrl = new ObservableField<>();
}
```

* 写个item布局，一个文字加一张图片，数据用上面那个表
```xml
<?xml version="1.0" encoding="utf-8"?>
<layout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto">

    <data>
        <variable
            name="user"
            type="com.example.databindtest.User" />
    </data>
    <LinearLayout
        android:orientation="horizontal"
        android:layout_width="match_parent"
        android:layout_height="match_parent">
        <TextView
            android:layout_width="0dp"
            android:layout_weight="2"
            android:layout_height="40dp"
            android:text="@={user.name}"/>
        <ImageView
            android:layout_width="0dp"
            android:layout_weight="9"
            android:layout_height="100dp"
            app:imaUrl="@{user.imaUrl}"/>
    </LinearLayout>
</layout>
```
* 适配器
```java
public class MyAdapter extends RecyclerView.Adapter {
    private Context context;
    List<User> userList;

    public MyAdapter(Context context, List<User> userList) {
        this.context = context;
        this.userList = userList;
    }

    @NonNull
    @Override
    public RecyclerView.ViewHolder onCreateViewHolder(@NonNull ViewGroup parent, int viewType) {
        LayoutInflater inflater = LayoutInflater.from(context);
        ItemBinding binding = DataBindingUtil.inflate(inflater,R.layout.item,parent,false);
        return new myHolder(binding.getRoot());
    }

    @Override
    public void onBindViewHolder(@NonNull RecyclerView.ViewHolder holder, int position) {
        ItemBinding binding = DataBindingUtil.getBinding(holder.itemView);
        binding.setUser(userList.get(position));
        binding.executePendingBindings();
    }
    @Override
    public int getItemCount() {
        if(userList == null){
            return 0;
        }else return userList.size();
    }
    public class myHolder extends RecyclerView.ViewHolder{

        public myHolder(@NonNull View itemView) {
            super(itemView);
        }
    }
}
```
可以看到适配器和传统的用法是差不多的，主要看 **onCreateViewHolder()** 和**onBindViewHolder()** 这两个函数。
* 其他部分适合以前一样使用即可。