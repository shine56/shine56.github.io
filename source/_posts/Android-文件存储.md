---
title: Android-文件存储
date: 2019-02-12 18:30:52
edited:
tags: Android入门
categories: Android
---
移动数据的存储方式之一就是文件存储，这里对其进行介绍。
## 使用特点与对象
特点：
>不会对数据进行初始化处理，原封不动地讲数据存储到文件中

一般这几种数据可以用文件对数据进行存储：
>* 简单的文本数据
>* 二进制数据<!--more-->

## 使用方法
Android文件存储的方法代码和Java文件存储思路是一样的。但是Android文件存储创建文件和Java有所区别。这里Context类提供了一个openFileOutput方法指定存储数据的文件，如果指定文件不存在则会创建该文件。此方法接受两个参数：
* 第一个参数：指定的文件名
* 第二个参数：文件的操作方式，有两种可供选择：
>MODE_APPEND 追加内容
>MODE_PRIVATE 覆盖之前的内容

### 写入文件
```java
public void save(String text){
    FileOutputStream out = null;
    BufferedWriter writer = null;
    try {
    //文件名为"data", 操作方式为追加
        out = openFileOutput("data", Context.MODE_APPEND);
        writer = new BufferedWriter(new OutputStreamWriter(out));
        writer.write(text);
    }catch (IOException e){
        e.printStackTrace();
    }finally {
        try {
            if(writer != null){
                writer.close();
            }
        }catch (IOException e){
            e.printStackTrace();
        }
    }
}
```
###  写出文件
这里将数据读取出来存放在Builder，然后将其返回
```java
public String load(){
    FileInputStream in = null;
    BufferedReader reader = null;
    StringBuilder content = new StringBuilder();
    try {
    //指定读取文件"data"
        in = openFileInput("data");
        reader = new BufferedReader(new InputStreamReader(in));
        String line = "";
        while ((line = reader.readLine()) != null){
            content.append(line);  //将数据放在Builder
        }
    }catch (IOException e){
        e.printStackTrace();
    }finally {
        if (reader != null){
            try {
                reader.close();
            }catch (IOException e){
                e.printStackTrace();
            }
        }
    }
    return content.toString();
}
```
