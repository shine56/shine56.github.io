---
title: 自定义View学习
date: 2019-09-15  15:46:32
edited:
tags: Android学习
categories: Android
---
### 为什么要自定义View？
系统配置的View满足不了我们的需求，我们需要针对业务制作一个自己的View。
### 怎么自定义View？步骤是啥？
 我是小白啥不懂呀，怎么去自定义一个自己的view呢？下面跟着我一步步先定义一个正方形的view试试看。
<!--more-->
##### 在values目录下新建一个firs.xml文件,编写内容如下：
```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <declare-styleable name="first_test">
    </declare-styleable>
</resources>
```
这个是什么呢？这个是我们给自定义View设置的**属性文件**，但是我们现在只是把styleable命名为first_test，其他一个属性也没加。后面会讲

##### 新建一个java文件**继承于View**
```java
public class FirstTest extends View {
    public FirstTest(Context context) {
        super(context);
    }
     public FirstTest(Context context, AttributeSet first) {
        super(context, first);
    }
}
```
这里之重写了View的两个**构造函数**（View不止这两个构造函数，但是这两个是必写的，其他的后面会讲）
##### 然后把我们的view加入我们的布局当中
```xml
<com.example.a73233.test.FirstTest
        android:layout_width="match_parent"
        android:layout_height="200dp"
        android:background="#00BCD4"
        />
```
这里我们给他设置了三个系统属性，宽度和高度，以及背景颜色。

是的，这样自定义view的初步工作就完成了。但是显然这个View是空空的，啥也没有。。。好，我们回到Java文件给他搞点东西。
我们看到重写了View的一个**构造函数**，但显然是不够的。那么我们还要重写什么函数呢？如下：
##### **onMeasure()**
```java
public class FirstTest extends View {
    public FirstTest(Context context) {
        super(context);
    }

    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        super.onMeasure(widthMeasureSpec, heightMeasureSpec);
    }
}
```
onMeasure() 函数的功能负责**测量我们View的宽高尺寸** 这里出现了两个参数**widthMeasureSpec** 和**heightMeasureSpec** 。

这两个看着有点眼熟的是什么东东？他们其实就是我们从刚刚的布局中拿到的**宽度和高度** 。重点来了，以heightMeasureSpec为例，虽然它是**一个参数**，但是它其实携带了**两个量**，其一为我们拿到的view的高度，其二为**测量模式**。

我们用起来是这样的：
```java
int heightSize = MeasureSpec.getMode(heightMeasureSpec);//取出的高度
int heightMode = MeasureSpec.getMode(heightMeasureSpec);//取出的测量模式
```
这个测量模式有三种，如下表：


|测量模式| 二进制数值 |描述|
|:---:|:---:|:---:|
|  UNSPECIFIED|  00|默认值，父控件没有给子view任何限制，子View可以设置为任意大小。
|EXACTLY| 01|表示父控件已经确切的指定了子View的大小。
|AT_MOST| 10|表示子View具体大小没有尺寸限制，但是存在上限，上限一般为父View大小。

好了，理论搞完了，因为我们要做的是一个正方形的view ,我们添加多一点代码来完成这个onMeasure() 函数。代码比较简单。
```java
 private int getSize(int defaultSize, int measureSpec) {
        int mySize = defaultSize;

        int mode = MeasureSpec.getMode(measureSpec);
        int size = MeasureSpec.getSize(measureSpec);

        switch (mode) {
            case MeasureSpec.UNSPECIFIED: {
                mySize = defaultSize;//默认大小
                break;
            }
            case MeasureSpec.AT_MOST: {
                mySize = size;      //取最大值
                break;
            }
            case MeasureSpec.EXACTLY: {
                mySize = size;   //固定值不要改变
                break;
            }
        }
        return mySize;
    }
@Override
protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
    super.onMeasure(widthMeasureSpec, heightMeasureSpec);

    int heightSize = getSize(200,heightMeasureSpec);
    int widthtSize = getSize(200,widthMeasureSpec);

    if(heightSize > widthtSize){
        heightSize = widthtSize;
    }else {
        widthtSize = heightSize;
    }

    setMeasuredDimension(widthtSize,heightSize);
}

```
好了到这里我们可以运行一下了。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190915185650619.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjYxOTg1Ng==,size_16,color_FFFFFF,t_70)

ok,正方形的view。那么问题又来了，如果我们想定义的是一个圆形的View呢？这就涉及到下面重写的这个函数了。
##### **onDraw()**
```java
 @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);
    }
```
接下来重写的这个onDraw()函数大有来头，显而易见，上面我们把view的宽高测出来了，这个函数就把我们想要的效果画出来。
我们直接上代码（看注释）：
```java
 @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);

        Paint paint = new Paint();  //新建一个画笔对象
        paint.setColor(Color.BLACK);  //把画笔设置成黑色
        paint.setStrokeWidth(10f);   //设置画笔粗细
        paint.setStyle(Paint.Style.FILL);  //设置画笔为填充模式

        canvas.drawCircle(300,300,300, paint);//开始绘制圆形
    }
   ```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190915185545555.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjYxOTg1Ng==,size_16,color_FFFFFF,t_70)
在这里你可以发现明明onDraw函数画的是一个圆，怎么刚才的正方形还在？那是因为在onMeasure()函数我们确定了控件大小（占地面积），但是画圆的时候没用完，所以还能看见我们在**布局文件设定的背景颜色**，我们把背景色去掉就只剩下一个圆了。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190915190325978.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjYxOTg1Ng==,size_16,color_FFFFFF,t_70)
### 自定义view就这样？
怎么可能？上面只是画一个圆来简单的介绍了自定义一个view的步骤。
* 还记得我们一开始新建了一个view的firs.xml的文件吧，我没还可以给这个view添加更多的属性。[看这里](https://www.jianshu.com/p/8844de6addb3)
* 还有我们在重写view的onDraw()函数的时候，设置了画笔的属性，那么画笔还有那些属性呢？
* 除了圆，我们还可以画其他的图形，怎么画呢?[看这里](https://www.gcssloop.com/customview/Canvas_BasicGraphics
)

博客参考各种网络资源，侵删，若有错误，恳请指正。