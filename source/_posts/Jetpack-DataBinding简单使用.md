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

##### 双向绑定
想要达到数据改变 ->UI改变，UI改变 -> 数据改变。改变布局的一行代码就行了

android:text"@{}" 改成 android:text="@{}" 

```xml
<EditText
	android:layout_width="match_parent"
	android:layout_height="wrap_content"
	android:text="@={user.name}">
</EditText>
```

#### 点击事件
* 定义点击事件响应类
```java
public class myOnClick{
   public void bClick(){
        Log.d("测试","name="+user.getName());
    }
}
```

* data中添加声明

```xml
<data>
	<variable
	    name="user"
	    type="com.example.databindtest.User" />
	<variable
	    name="bclick"
	    type="com.example.databindtest.MainActivity.myOnClick" />
</data>
```

* button引用

```xml
<Button
	android:onClick="@{()->bclick.bClick()}"
	android:layout_width="wrap_content"
	android:layout_height="wrap_content"
	android:text="改变">
</Button>
```
完毕。