title: 《Java核心技术卷I》笔记——第四章

date: 2016-02-22 14:10:33

tags: [Java, 笔记]



---

本文为《Java核心技术卷I》的第四章的读书笔记，因为为Java的基础部分，因此大部分知识都已经掌握，主要对需要**进一步理解**和**之前没有掌握**的知识进行了摘录总结。

<!--more-->

## 一、 对面向对象思想的理解

### 　对象的三个主要特性：

1. 对象的行为（behaviour） —— 能够对对象施加哪些操作
2. 对象的状态（state） —— 施加方法的时候，对象应该如何响应
3. 对象的标识（identity） —— 如何辨别具有相同行为和状态的不同对象

对象状态的改变必须通过调用方法实现， 如果不经过方法调用就可以改变对象状态，说明封装性遭到了破坏。

每个对象的标识永远是不同的， 状态常常存在差异。

对象的这些关键特征彼此相互影响。

## 二、 日期的处理

不推荐使用Date类，而推荐使用GregorianCalendar类。



``` Java
new GregorianCalendar(1991, Calendar.DECEMBER, 31, 23, 29, 29);
```

之前不知道有GregorianCalendar这个类，在处理日期的时候，用的一直是Date类，因此用了很多被标记为已经过期的API，如`getYear()`, `getMonth()`等。这次学习之后，了解到，**当用到记录时间点的时候，使用Date类**， 而**处理日期的时候，使用GregorianCalendar类**。

## 三、getter的返回值

在返回可变对象的引用的时候，不能直接返回，因为其引用的对象是同一个，这样做会破坏其封装性。应该返回其克隆对象。

eg：

``` java
return date.clone();
```

## 四、Java的函数传递使用值传递

**Java中只有值传递**

方法参数的使用情况：

* 一个方法不能修改一个基本数据类型的参数（数值型和布尔型）
* 一个方法可以改变一个对象参数的状态
* 一个方法不能让对象参数引用给一个新的对象

## 五、类的初始化步骤

1. 所有数据域被初始化为默认值（0， false，null）
2. 按照在类生命中出现的次序，依次执行所有域初始化语句和初始化块
3. 如果构造器第一行调用了第二个构造器，则执行第二个构造器主体
4. 执行这个构造器的主体

也就是说，如果一个数据域（成员变量）的初始化语句（直接写在变量定义之后的）调用了一个类的方法的话，这个方法可以先于构造器执行。

一个简单的例子如下：

``` java
	class Foo {
	
	    {
	        System.out.println("This is a initial block of Foo!");
	    }
	
	    int a = assign();
	
	
	    public Foo()
	    {
	        System.out.println("This is the construction method of Foo!");
	    }
	
	    private int assign()
	    {
	        System.out.println("this is the assign method of Foo!");
	        return 5;
	    }
	}

```

这个类在创建新的实例的过程中，打印出的语句如下：

``` 
This is a initial block of Foo!
this is the assign method of Foo!
This is the construction method of Foo!
```

其中，第一句和第二句的顺序，与初始化语句块和成员变量a的定义顺序相对应。