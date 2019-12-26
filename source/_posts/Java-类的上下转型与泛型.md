---
title: Java--类的上下转型与泛型
date: 2019-01-27 21:14:19
edited:
tags:
categories:
---
## 向上向下转型

* 向上转型：即子类转换成父类
* 向下转型：父类转换成子类是**不允许的**,即Java不支持向下转型,**但是**如果该父类本身就是由子类转换而来，则可向下转型
* 类型转换带来的效用就是**多态**
* 类型转换前后的对象引用的方法皆为子类的方法(即使子类重写过父类的方法)

<!-- more-->
```java
class animal1 {
	public void eat() {
		System.out.println("You can eat!");
	}
}
public class human extends animal1{
	public void eat() {
		System.out.println("I can eat!");
	}
	public static void main(String[] args) {
		human obj1 = new human();   
		obj1.eat();     //调用的是子类的eat();
		animal1 obj2 = (animal1) obj1;//向上转型
		obj2.eat();     //调用的是子类的eat();  
		human obj3 = (human) obj2;  //向下转型
		obj3.eat();     //调用的是子类的eat();  
	}
}
```
输出：

>I can eat!
I can eat!
I can eat!

## 泛型类
* 泛型类是一种特殊的类。前面说类型的转换只能向上不能向下,这就给会造成一些不必要的麻烦或是安全问题,为此,泛型类应运而生.
* 语法：class+类名+<T>{}

这个**T**可以代表任何类型，看例子：
```java
class man1 <T>{
	public T a;
	public man1(T a) {
		this.a = a;
	}
	public T get() {
		return a;
	}
}
public class human{
	
	public static void main(String[] args) {
		//有了泛型便可随意给a赋值，无需担心类型转换带来不安全
		man1 obj1 = new man1(1);
		man1 obj2 = new man1(1.1);
		man1 obj3 = new man1("abc");
		man1 obj4 = new man1(true);
		System.out.println("a="+obj1.get());
		System.out.println("a="+obj2.get());
		System.out.println("a="+obj3.get());
		System.out.println("a="+obj4.get());
	}
}
```
输出：

>a=1
a=1.1
a=abc
a=true

## 泛型接口

* 定义一个泛型接口

```java
public interfase man1 <T>{
	public T eat();
}
```
* 实现这个接口的方法时，**如果没有泛型实参传入** ，则需要在声明这个类时，声明该类为**泛型类**

```java 
lass human<T> implements man1<T>{
    @Override
    public T eat() {
        return null;
    }
}
```
* 有实参传入时,必须将所有泛型标识换成实参类型

```java
class human implements man1<String>{
    public String eat() {
		public s = "I can eat!";
        return s;
    }
}

```














