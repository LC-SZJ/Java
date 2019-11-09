[TOC]

# Java 常用API（二）

## 1. Object类

+ Object类位于 java.lang.Object 包下。
+ 类Object是类层次结构的***根类（父类）***。
+ 每个类都使用Object类作为超类（父类）。
+ 所有对象（包括数组）都实现了这个类的方法。


+ public String toString()：返回该对象的字符串表示。
+ public boolean equals(Object obj)： 指示其他某个对象是否与此对象“相等”。

> 注意事项：
> 1. 看一个类是否重写了 toString方法，直接打印这个***类的对象***即可。
> 2. 如果没有重写 toString方法，那么打印的是对象的***地址值***。
> 3. Random类没有重写 toString方法
> 4. Scanner、ArrayList`<E>`类重写了 toString方法。
> 5. Object类中的equals方法默认比较的是两个对象的地址值，而String类中的equals方法是将Object类中的equals方法***覆盖重写*** 了，比较的是字符串的内容。
> 

多态的缺点：不能向下使用子类的特有的内容（属性和方法）。
解决：使用向下转型。

+ 示例：

```java
// Person.java
public class Person {
    private String name;
    private int age;

    public Person() {
    }

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    // 覆盖重写Object类中的toString方法
    @Override
    public String toString() {
       return "Person{" + "name='" + name + "\'" + ", age=" + age + "}";
    }
    
	// 覆盖重写Object类的equals方法
    @Override
    public boolean equals(Object obj) {
        // 判断是否等于自身
       if (obj == this){
           return true;
       }
       // 判断是否为空
       if (obj == null){
           return false;
       }
       // 判断是否为自定义的Person类
       if (obj instanceof Person){
           // 向下转型
           Person person = (Person)obj;
           return person.age == this.age && person.name.equals(this.name);
       }
       // 不是Person类直接返回false
       return false;
    }
    /*
    // 使用 alt + ins 添加的
    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        // getClass()是使用反射技术判断 o 是否是Person类型 等效于 o instanceof Person
        if (o == null || getClass() != o.getClass()) return false;
        Person person = (Person) o;
        return age == person.age &&
                Objects.equals(name, person.name);
    }
    */

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}

// Demo02Main.java

import java.util.ArrayList;

public class Demo02Main {
    public static void main(String[] args) {
        Person one = new Person("迪丽热巴", 20);
        Person two = new Person("迪丽热巴", 20);

        System.out.println(one); // 没有重写之前打印的是地址值
        one.toString(); // Person{name='迪丽热巴', age=20}
//        System.out.println(one.toString()); // 没有重写之前打印的也是地址值
        System.out.println(one.equals(two)); // true
        System.out.println(one == two); // false

    }
}
```


+ 关于Objects类的介绍

  在JDK 7添加了一个Objects工具类，它提供了一些方法来操作对象，它有一些静态的实用方法组成，这些方法是null-save（空指针安全）或者null-tolernt（容忍空指针的），用于计算对象的hashcode，返回对象的字符串表示形式，比较两个对象。

在比较 两个对象的时候，Object的equals方法容易抛出空指针异常，而Objects类中的equals方法就优化了这个问题，防止空指针异常。

+ Objects类中的equals方法

```java
public static boolean equals(Object a, Object b){
	return (a == b) || (a != null) && a.equals(b);
}
```



## 2. Date类

### 概述

+ java.util.Date：表示日期和时间类。
+ 类Date表示特定的瞬间，精确到毫秒。1 秒 = 1000 毫秒
+ 毫秒值的作用：可以对日期和时间进行计算。
+ 时间原点：1970 年 1 月 1 日 00:00:00（英国格林威治）
+ 1 天 = 24 × 60 × 60 = 86400 秒 = 86400000毫秒

> 注意事项：
> 中国属于东八区，会把时间增加8个小时
>  1970 年 1 月 1 日 08:00:00


```java
// 输出当前时间距离时间原点的毫秒数，这是一个long类型
// 1565579081183L
System.out.println(System.currentTimeMillis());
```
### 构造方法和成员方法

Date类的空参构造

+ Date()：获取当前系统的时间和日期。

```java
import java.util.Date;

public class DemoDate {
    public static void main(String[] args) {
        Date date = new Date();
        System.out.println(date); // Mon Aug 12 11:18:35 CST 2019 
    }
    
}
```

Date类的带参构造

+ Date(long date)：传递毫秒值，把毫秒值装换为date日期。

``` java
 Date dateOne = new Date(1565579081183L);
        // Mon Aug 12 11:04:41 CST 2019
        System.out.println(dateOne);
        
Date dateTwo = new Date(0L);
        // Thu Jan 01 08:00:00 CST 1970
        System.out.println(dateTwo);
```

Date类的成员方法：

+ long getTime()：返回自 1970 年 1 月 1 日 00:00:00 GMT 以来此 Date 对象表示的毫秒数。

```java
public static void method03(){
        Date date = new Date();
    	// 1565580373625
    	// 相当于System.currentTimeMillis()方法
        System.out.println(date.getTime()); 
    }
```

## 3. DateFormat类

### 概述

+ java.text.DateFormat：是日期/时间格式化子类的抽象类
+ 作用：格式化（日期 --> 文本），解析（文本 --> 日期）


成员方法：

1. String format(Date date)：按照指定的模式，把Date日期，格式化为符合模式的字符串。
2. Date parse(String source)：把符合模式的字符串，解析为Date日期。

DateFormat类是一个抽象类，无法直接创建对象使用，可以使用DateFormat类的子类，SimpleDateFormat就是DateFormat的一个子类。

java.text.SimpleDateFormat extends DateFormat


### SimpleDateFormat类

+ SimpleDateFormat(String pattern)：用指定的模式和默认语言环境的日期和时间格式符号构造 SimpleDateFormat
+ 参数：String pattern：传递指定的模式（区分大小写）。
+ 常用的格式规则：

|标识字母（区分大小写）|含义|
|---|---|
|y	|年|
|M	|月|
|d	|日|
|H	|时|
|m	|分|
|s	|秒|

> 注意事项：模式中的字母不能更改，连接模式的符号可以改变。
> 

DateFormat类中的format方法的使用步骤：

1. 创建一个SimpleDateFormat对象，构造方法中传递指定的模式。
2. 调用SimpleDateFormat中的方法format，按照构造方法中指定的模式，把Date日期格式化为符合模式的字符串（文本）。

> 使用parse方法时的注意事项：
> 1. public Date parse(String source) throws ParseException
> parse 方法声明了一个异常：ParseException
> 2. 如果字符串和构造方法的模式不一样，那么程序就会抛出此异常。
> 3. 调用一个抛出异常的方法，就必须处理这个异常，要么throws继续抛出这个异常，要么 try catch 自己处理。
> 

+ 示例：

```java
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;

public class DemoSimpleDateFormat {
    public static void main(String[] args) throws ParseException {
        // 首先将当前 Date格式化为 String
        // pattern:yyyy-MM-dd HH-mm-ss
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");

        // 创建一个Date对象
        Date date = new Date();
        
        // 调用SimpleDateFormat类中的format方法
        String format = sdf.format(date);
        System.out.println(format); // 2019-08-12 12:24:57

        // 将String解析为Date类
        Date parse = sdf.parse(format);
        System.out.println(parse); // Mon Aug 12 12:24:14 CST 2019

    }
}
```

### 练习

```java
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.Scanner;

/*
题目要求：
使用日期时间相关的API，计算一个人已经出生了多少天？

分析：
1.输入你的出生日期，使用Scanner类中的next方法获取
2.将出生日期装换为毫秒
3.获取当前时间并装换为毫秒
4.差值 = 当前时间 - 出生日期
5.打印结果: 差值 / 1000/60/60/24
 */
public class DemoSimpleDateFormatTest {
    public static void main(String[] args) throws ParseException {
        // 创建一个Scanner对象
        Scanner sc = new Scanner(System.in);
        System.out.println("请输入您的出生日期，格式：yyyy-MM-dd.");

        // 接受输入的出生日期
        String birthdayStr = sc.next();

        // 转换为 Date类
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
        Date birthdayDate = sdf.parse(birthdayStr);

        // 装换为毫秒
        long birthdayTime = birthdayDate.getTime();

        // 获取当前时间并装换为毫秒
        long currentTime = new Date().getTime();

        // 计算差值

        long difference = currentTime - birthdayTime;
        // 装换为天数
        long liveDay = difference/1000/60/60/24;
        // 打印结果
        System.out.println(liveDay);
    }
}
```

## 4. Calendar类

+ java.util.Calendar类：日历类
+ Calendar类是一个抽象类，里边提供了很多操控日历字段的静态方法（YEAR、MONTH、DAY_OF_MONTH、HOUR 等）。
+ Calendar类无法直接创建对象使用，里面有一个静态方法getInstance()，该方法返回了Calendar类的子类对象。
+ public static Calendar getInstance()：使用默认时区和语言环境获得一个日历。

常用方法

1. public int get(int field)：返回给定日历字段的值。
    参数：传递给定的日历字段（YEAR、MONTH）
    返回值：日历字段代表的具体值
    
2. public void set(int field, int value)：将给的的日历字段设置为给定值。
    int field：传递给定的日历字段。
    int value：给指定的日历字段设置值。
   
3. public abstract void add(int field, int amount)：根据日历的规则，为给定的日历字段添加 / 减去指定的时间量。
    int field：传递指定的日历字段。
    int amount：增加 / 减少指定的值。

4. public Date getTime()：将Calendar对象装换为Date类对象。

## 5. System类

java.lang.System类中提供了大量的静态方法，可以获取与系统相关的信息或者系统级操作。

常用方法

1. public static long currentTimeMillis()：返回以毫秒为单位的当前时间。
2. public static void arraycopy(Object src, int srcPos, Object dest, int destPos, int length)：将数组中指定的数据拷贝到另一个数组中。

System.currentTimeMillis()用来测试系统的运行时间

```java
public class DemoSystem {
    public static void main(String[] args) {
        long start = System.currentTimeMillis();

        int sum = 0;
        for (int i = 0; i < 9999; i++) {
            System.out.println(i);
        }
        long end = System.currentTimeMillis();
        long time = end - start;
        System.out.println("程序耗时：" + time); 
    }
}
```

## 6. StringBuilder类

+ java.lang.StringBuilder

### String类的原理

+ 字符串是常量：它们的值在创建之后不能改变。
+ 字符串底层是一个被final修饰的字节数组，不能改变，是一个常量。
  ***private final byte[ ] value;***
+ 进行字符串相加时，内存中就会有多个字符串，占用空间多，效率低下。

+ 例如：对字符串"a" + "b" + "c"计算的结果是一共有五个字符串，分别是：”a"，"b"，"c"，"ab"，"abc"。

### StringBuilder类的原理

+ StringBuilder也叫字符串缓冲区，可以提高字符串的操作效率（看成一个长度可以变化的字符串），底层也是一个字符串，但是没有被final修饰，可以改变长度。***byte[ ] value = new byte[16];***
+ 对字符串"a" + "b" + "c"来说，会得到四个字符串，分别是："a"， "b"，  "c"，"abc"。
+ StringBuilder在内存当中始终是一个数组，占用空间少，效率高。
+ 如果超出StringBuilder的容量，会自动的扩容。

### StringBuilder的构造方法

+ public StringBuilder()：构造一个空的字符串生成器，其初始容量为 16 个字符。
+ public StringBuilder(String str)：构造一个字符串生成器，并初始化为指定的字符串内容。

### StringBuilder的常用方法

+ public StringBuilder append(...)：添加任意类型数据的字符串表示形式，并返回当前对象自身。
+ public String toString()：将当前StringBuilder对象转换为String对象。 
+ StringBuilder reverse() ：将此字符序列用其**反转**形式取代。 

+ StringBuilder和String可以相互转换
    + String --> StringBuilder：使用StringBuilder的构造方法。
    + StringBuilder --> String：使用StringBuilder的toString方法。

> 注意事项：
> 1. append方法返回的是自身，也就是 this
> 2. 使用 append方法无需接受返回值
> 

## 7. 包装类

### 概述

基本数据类型使用起来非常方便，但是没有对应的方法来操作这些基本类型的数据，可以使用一个类，**把基本类型的数据包装起来**，在类中定义一些方法，这个类叫做***包装类***，我们可以使用类中的方法来操作这些基本类型的数据。

### 装箱与拆箱（以 int 类型为例）

基本类型与包装类对象之间，来回转换的过程称为”装箱“与”拆箱“。

+ 装箱：基本类型装换为对应的包装类对象。
+ 拆箱：从包装类对象装换为对应的基本类型。

一、装箱
装箱通过两种方式，一种构造方法，另一种静态方法，下列构造方法已过时。
1. 构造方法：

+ public Integer(int value)：构造一个新分配的Integer对象，它表示指定的的 int 值。
+ public Integer(String s)：构造一个新分配的Integer对象，它表示 String 参数所指示的 int 值。传递的字符串对应的基本类型的字符串，否则会抛出异常：***NumberFormatException***，例如："100" 正确，"a" 错误。

2. 静态方法：

+ public static Integer valueOf(int i)：返回一个表示指定 int 值的 Integer 实例。
+ public static Integer valueOf(String s)：返回保存指定的 String 的值的 Integer对象。

二、拆箱
成员方法：

+ int intValue() ：以 int 类型返回 Intrger 的值。

### 自动装箱与自动拆箱

+ 基本数据类型和包装类之间可以自动的相互转换。
+ 从JDK 1.5+开始支持自动装箱与自动拆箱。


### 基本类型与字符串类型之间的相互转换

一、基本类型 --> 字符串类型

1. 基本类型的值 + "" 最简单的方式（工作中常用）。
2. 包装类的静态方法：toString(参数)，不是Object类的toString()，是重载public static String toString(int i)：返回一个表示指定整数的 String 对象。
3. String类的静态方法：valueOf(int i)：返回 int 参数的字符串表示形式。

二、字符串类型 --> 基本类型

1. 使用包装类的静态方法 parseXXX("数值类型的字符串");
    Integer类：public static parseInt(String s)
    Double类：public static parseDouble(String s)
    
```java
// 基本类型 --> 字符串类型
        int i1 = 100;
        String s1 = i1 + "";
        System.out.println(s1); // 100

        int i2 = 100;
        String s2 = Integer.toString(i2);
        System.out.println(s2 + 200); // 100200

        int i3 = 100;
        String s3 = String.valueOf(i3);
        System.out.println(s2 + 200); // 100200

        // 字符串类型 --> 基本类型
        String s4 = "100200";
        int i4 = Integer.parseInt(s4);
        System.out.println(i4); // 100200
```

