---
title: Java--final和static与常量
date: 2019-01-27 20:59:20
edited:
tags: Java入门
categories: Java学习
---
## static
static关键字有三个注意点：

* static变量/方法依赖与类存在，而不是对象，通过类即可访问.
* 所有的对象实例中，static变量/方法都共享储存在同一空间（栈）.
* 所有的对象实例中，static变量/方法都共享储存在同一空间（栈）.

<!--more-->

```java
class A{
	static int a = 2;
}

public class B {
	public static void main(String[] args) {
		System.out.println(A.a);  //直接通过类即可引用
		A obj1 = new A();
		A obj2 = new A();
		obj1.a = 3;               //共享存储空间，obj1.a和obj2.a同时改变。
		System.out.println(obj2.a);
	}
}
```
输出：
>2
3

## final
注意点：
* final类不能被继承
* 子类不能重写父类中的final方法.
* final变量一旦被设定,不可再改变.
* final的对象不可变更其指针.

## 常量
相比C语言Java中没有constant关键字,那么在Java中如何定义一个常量呢？首先一个常量要具备一下条件：

* 不能被修改
* 只有一份
* 方便访问

知道这三点,在Java中定义一个常量就明确了: public static final A; //A就是一个常量.

* 此外,java中还有一种特殊的常量:接口中定义的变量默认为常量

```java
public interface A{
	String color = "yellow"  //此为常量
}
```

# 常量池

Java为很多基本类型都建立了常量池，就是说我们不需要自己去定义常量池中的常量

* 常量池如下：

>Boolen : true/false
Byte,Character : 0-127
Short Int Long : -128-127
Float Double : 没有常量池

**使用的时候要注意：**
* 这些类新建的对象并一定是是静态变量,这会取决与其建立方式

```java
Integer a = 10;   //放在栈内存,常量化
Integer a = new Integer(10);   //放在堆内存,不会常量化
```


