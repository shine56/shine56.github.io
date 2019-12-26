---
title: Android--使用LiteaPal操作SQLite
date: 2019-02-14 23:24:08
edited:
tags: Android入门
categories: Android
---
### 什么是LitePal?
LitePal是Github上的开源数据库框架，使用它呢，可以使我们不需要直接用SQL语句就可以操作安卓自带的SQLite数据库，而是用**面对对象**的思维去操着数据库，这对于只接触过Java没接触过SQL的小白(比如笔者)简直是一大福音。这里介绍Android Studio **Java语言**下使用LitePal。

### 配置LitePal
第一步、在app/build.gradle的dependencies中添加依赖：
<!--more-->

    implementation 'org.litepal.android:java:3.0.0'
第二步、在 app/src/main 目录下新建名为 assets 的目录，在 assets 目录下新建名为 litepal.xml 的的xml文件。打开litepal.xml将代码改成如下内容
```xml
<?xml version="1.0" encoding="utf-8"?>
<litepal>
    <!--
    	数据库名，例如：
    	<dbname value="demo" />
    -->
    <dbname value="demo" />

    <!--
    	数据库版本号，例如：
    	<version value="1" />
    -->
    <version value="1" />

    <!--
    	指定映射模型，下面会讲到，例如：
    	<list>
    		<mapping class="com.test.model.Reader" />
    		<mapping class="com.test.model.Magazine" />
    	</list>
    -->
    <list>
    </list>  
</litepal>
```
最后、配置AndroidManifest.xml，在application加入

    android:name="org.litepal.LitePalApplication"
```xml
...
    <application
        android:name="org.litepal.LitePalApplication"
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="i Note"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">
        
    </application>
```
这样算是配置完了。还是比较简单的。
### 创建数据库
* 首先我们先创建一个notelist_SQLite类，对应的是数据库中的notelist_SQLite表
```java
import org.litepal.crud.LitePalSupport;
//notelist_SQLite继承于LitePalSupport
public class notelist_SQLite extends LitePalSupport {
    private String note;
  
    public void setNote(String note) {
        this.note = note;
    }

    public String getNote() {
        return note;
    }
}
```
看到这个类，你应该知道怎么往表里添加数据了吧，面对对象的思维来表的操作是不是很直直观.hhh。。
* 接下来需要把这个映射模型添加到刚才的litepal.xml中。
```xml
<list>
        <mapping class = "com.example.a73233.i_note.notelist_SQLite"></mapping>
</list>
```
* 我们对这个表进行任意操做(如：增，删)时就会自动在数据库生成这个表了
### 升级数据库
这个简单
* 每次升级数据库，即：修改notelist_SQLite类，或者增加一个新的类。就在litepal,xml的版本号那里，将1改成2就行了。
* 这里在notelist_SQLite类增加了data
```java
public class notelist_SQLite extends LitePalSupport {
    private String note;
    private String date;

    public void setNote(String note) {
        this.note = note;
    }

    public String getNote() {
        return note;
    }

    public void setDate(String date) {
        this.date = date;
    }
    public String getDate() {
        return date;
    }
}
```
* 修改litepal,xml的版本号

       <version value="2" />
这就完成了数据库的升级
### 向数据库增加数据
直接上代码，看注释
```java
String text = "English text";
String date = "2019/2/14";
notelist_SQLite note = new notelist_SQLite();  //获取对象
note.set(text); //增加数据
note.set(date);
note.save();   //保存数据
```
### 更新数据
第一种方法LitePal.find(notelist_SQLite.class, id);找到对应表格的id，将其中数据修改
```java
notelist_SQLite note = LitePal.find(notelist_SQLite.class, id);
noteSQ.setDate("2019年2月14日");  //设置更新的数据
noteSQ.save();
```
第二种方法，用 update(id); 将对应id的数据修改
```java
notelist_SQLite noteSQ = new notelist_SQLite();
noteSQ.setDate("2019年2月14日");  //设置更新的数据
noteSQ.update(id);
```
第三种方法，updateAll(); 用法看注释
```java
notelist_SQLite noteSQ = new notelist_SQLite();
noteSQ.setDate("2019年2月14日");  //设置更新的数据
noteSQ.updateAll("note = ?", "English text");
//这里指定note内容是“English text”的表格的Date会被修改成
//"2019年2月14日"如果没有指定，则会修改所有表格的内容。
```
### 删除数据
第一种方法，LitePal.delete(notelist_SQLite.class, id);将对应id的表删除。
```java
LitePal.delete(notelist_SQLite.class, id)；
```
  第二种方法，LitePal.deleteAll();指定删除的数据
```java
LitePal.deleteAll(notelist_SQLite.class,"note = ?","English text");
```
### 查询信息
其实AUDQ都差不多，先看第一种，查询指定id
```java
notelist_SQLite note = LitePal.find(notelist_SQLite.class, id);
String text = note.getNote();
```
第二种，查询所有id的数据
```java
List<notelist_SQLite> note = 
LitePal.findAll(notelist_SQLite.class); //找出所有数据将其放在List集合
```
还有更多更复杂和更有用的查询方式可以查看LitePal的API。

* LitePal的基本操作就介绍完了，还有其他的操作，如: 多线程异步操作、监听数据库创建或升级。可以查看[https://github.com/LitePalFramework/LitePal](https://github.com/LitePalFramework/LitePal)
* 本文编写参考于：
[第一行代码](https://www.baidu.com/s?ie=utf-8&f=8&rsv_bp=1&rsv_idx=1&tn=baidu&wd=%E7%AC%AC%E4%B8%80%E8%A1%8C%E4%BB%A3%E7%A0%81&oq=litepal%25E6%2593%258D%25E4%25BD%259Csqlite&rsv_pq=9929d52700008f29&rsv_t=2ffdjB%2B6VLYGqPtySqC%2BB%2Bdj4ARtl2f9B8%2FiGVgwyTjp0%2BIoPu2u1ACE%2BF4&rqlang=cn&rsv_enter=1&rsv_sug3=100&rsv_sug1=43&rsv_sug7=101&bs=litepal%E6%93%8D%E4%BD%9Csqlite)和[https://github.com/LitePalFramework/LitePal](https://github.com/LitePalFramework/LitePal)