[TOC]

# Java 基础加强笔记

## 1. Junit：单元测试

### 1.1 测试分类

1. 黑盒测试：不需要写代码。给输入值，看程序能否输出期望的值。
2. 白盒测试：需要写代码。关注程序具体的执行流程。

Junit测试是属于白盒测试的一种。

### 1.2 Junit使用：白盒测试

**使用步骤：**
1. 定义一个测试类（测试用例）
    - 建议：
        - 测试类名：被测试的类名Test	CalculatorTest
        - 包命：xxx.xxx.xx.test	com.my.test
2. 定义测试方法：可以独立运行
    - 建议：
        - 方法名：test测试的方法名	testAdd()
        - 返回值：void
        - 参数列表：空参
3. 给方法加 `@Test`
4. 导入 junit 依赖环境

**判定结果：**

- 红色：失败
- 绿色：成功
- 一般我们会使用**断言**操作来处理结果
    - Assert.assertEquals(期望的结果，实际的运算结果);
- 示例：
```java
import com.my.junit.Calculator;
import org.junit.After;
import org.junit.Assert;
import org.junit.Before;
import org.junit.Test;

public class CalculaotrTest {
    /**
     * 初始化资源方法
     * 用于资源申请：所有测试方法在执行之前都会先执行该方法
     */
    @Before
    public void init() {
        System.out.println("init...");
    }

    /**
     * 释放资源方法
     * 在所有测试方法执行完后，都会自动执行该方法
     */
    @After
    public void close() {
        System.out.println("close...");
    }

    /**
     * 测试add方法
     */
    @Test
    public void testAdd() {
        System.out.println("testAdd...");
        Calculator c = new Calculator();
        int result = c.add(1, 2);
        Assert.assertEquals(3, result);
    }

    /**
     * 测试sub方法
     */
    @Test
    public void testSub() {
        Calculator c = new Calculator();
        int result = c.sub(1, -1);
        Assert.assertEquals(2, result);
    }
}
```

**补充：**
- @Before：
    - 修饰的方法会在测试方法之前被自动执行。
- @After：
    - 修饰的方法会在测试方法执行之后自动被执行。


## 2. 反射：框架设计的灵魂

### 2.1 概述

* 框架：半成品软件。可以在框架的基础上进行软件开发，简化编码。
* 反射：将类的各个组成部分封装为其他对象，这就是反射机制。
    * 好处：
        1. 可以在**程序运行过程中**，操作这些对象。
        2. 可以**解耦**，提高程序的可扩展性。


* Java代码在计算机中的三个阶段
<img src="https://ae01.alicdn.com/kf/H56c3526a8a0a40cf86b25f073a3db571h.png" alt="Java代码在计算机中的三个阶段" style="zoom: 80%;" />

### 2.2 获取字节码文件的三种方式：

1.  `static Class<?> forName(String className)`：将字节码文件加载进内存，返回Class对象。
    * 多用于配置文件，将类名定义在配置文件中。读取文件，加载类。
2.  类名.class：通过类名的属性 class获取。
    * 多用于参数的传递。
3.  对象.getClass()：getClass()在Object类中定义着。
    * 多用于对象的获取字节码的方式。
* **结论**：同一个字节码文件(*.class)在一次程序的运行过程中，只会被加载一次，不论通过哪一种方式获取 Class对象都是同一个。

##### 示例：

```java
package com.my.reflect;

import com.my.domain.Person;

public class ReflectDemo1 {
    /*
        获取Class对象的三种方式：
            1.Class.forName("全类名"); --> 源代码阶段
            2.类名.class;     --> Class 类对象阶段
            3.对象.getClass(); --> Runtime 运行阶段
     */
    public static void main(String[] args) throws Exception {
        // 1.Class.forName("全类名");
        Class cls1 = Class.forName("com.my.domain.Person");
        System.out.println(cls1);
        // 2.类名.class
        Class cls2 = Person.class;
        System.out.println(cls2);
        // 3.对象名.getClass()
        Person person = new Person();
        Class cls3 = person.getClass();
        System.out.println(cls3);

        System.out.println(cls1 == cls2); // true
        System.out.println(cls1 == cls3); // true
        System.out.println(cls2 == cls3); // true
    }
}
```


### 2.3 Class对象功能：

#### 获取功能：

1. 获取成员变量们

    * Field[] getFields()：获取所有 public修饰的成员变量。
    * Field getField(String name) ：获取指定名称 public修饰的成员变量。

    * Field[] getDeclaredFields() ：获取所有的成员变量，不考虑修饰符。
    * Field getDeclaredField(String name) 获取指定名称成员变量，不考虑修饰符。
##### Field：成员变量

* 操作：
    1. 设置值
        * void set(Object obj, Object value) 
    2. 获取值
        * Object get(Object obj) 
    3. 【忽略】访问权限修饰符的安全检测
        - setAccessible(true)：暴力反射

##### 示例：

```java
package com.my.reflect;

import com.my.domain.Person;

import java.lang.reflect.Field;
import java.util.Arrays;
import java.util.stream.Stream;

public class ReflectDemo2 {
    public static void main(String[] args) throws Exception{
        /*
            获取public修饰的成员变量
         */
        // 0. 获取一个类的Class对象
        Class personClass = Person.class;
        // 1. Field[] getFields()：获取所有 public修饰的成员变量
        Field[] fields = personClass.getFields();
        // 使用Stream流
        Stream<Field> stream = Arrays.stream(fields);
        // 方法引用
        stream.forEach(System.out::println);
        // 2.Field getField(String name)：获取 public修饰的指定的成员变量
        Field a = personClass.getField("a");
        Person one = new Person();
        // 获取成员变量的值
        Object o = a.get(one);

        System.out.println(o);
        System.out.println("=================");
        // 设置成员变量的值
        a.set(one, "独孤");
        System.out.println(one);
        /*
            获取所有的成员变量
         */
        System.out.println("==================");
        // Field[] getdeclaredFields()：获取所有的成员变量
        Field[] declaredFields = personClass.getDeclaredFields();
        Stream<Field> stream2 = Arrays.stream(declaredFields);
        stream2.forEach(field -> System.out.println(field));
        // Field getdeclaredField(String name)
        Field d = personClass.getDeclaredField("d");
/*
        // 直接访问非 public 变量的值
        System.out.println(d.get(one)); // IllegalAccessException
*/
        // 【忽略】访问权限修饰符的安全检测
        d.setAccessible(true); // 暴力反射
        // 访问非 public 变量的值
        System.out.println(d.get(one)); // IllegalAccessException
    }
}
```

2. 获取构造方法们

    * Constructor`<?>`[] getConstructors() 
    * Constructor`<T>` getConstructor(Class`<?>`... parameterTypes) 
    * Constructor`<?>`[] getDeclaredConstructors() 
    *  Constructor`<T>` getDeclaredConstructor(Class`<?>`... parameterTypes) 


##### Constructor：构造方法

* 创建对象：
    * T newInstance(Object... initargs) ：创建对应的对象
    * 如果使用空参数构造方法创建对象，操作可以简化：`Class`对象的`newInstance()`方法
* 获取非 public 修饰的构造器时：
    * 【忽略】访问权限修饰符的安全检测
        - setAccessible(true)：暴力反射


##### 示例：

```java
package com.my.reflect;

import com.my.domain.Person;
import java.lang.reflect.Constructor;

public class ReflectDemo3 {
    public static void main(String[] args) throws Exception{
        // 获取对应类的Class对象
        Class<Person> pClass = Person.class;
        /*
            构造方法的获取
         */
        // 获取Person类中 String, int 的构造方法
        Constructor<Person> constructor = pClass.getConstructor(String.class, int.class);
        System.out.println(constructor);
        // 通过获取的Constructor对象创建Person对象
        Person person = constructor.newInstance("张三", 20);
        System.out.println(person);

        // 获取空参构造方法
        Constructor<Person> nullCon = pClass.getConstructor();
        Person person2 = nullCon.newInstance();
        System.out.println(person2);

        // 获取空参构造方法,直接使用Class类中的 newInstance()方法
        // Person person3 = pClass.newInstance(); // 此方法已过时
        Person person3 = pClass.getDeclaredConstructor().newInstance();
        System.out.println(person3);
    }
}
```


3. 获取成员方法们

    * Method[] getMethods() 
    * Method getMethod(String name, Class`<?>`... parameterTypes) 
    * Method[] getDeclaredMethods() 
    * Method getDeclaredMethod(String name, Class`<?>`... parameterTypes) 

##### Method：成员方法

* 执行方法：
    * Object invoke(Object obj, Object... args) ;
* 获取方法名称：
    * String getName();


##### 示例

```java

package com.my.reflect;

import com.my.domain.Person;
import java.lang.reflect.Method;

public class ReflectDemo4 {
    public static void main(String[] args) throws Exception{
        // 获取对应类的Class对象
        Class<Person> pClass = Person.class;
        /*
            获取成员方法
         */
        // 获取空参方法
        Method eat_method1 = pClass.getMethod("eat");
        Person p = new Person();
        eat_method1.invoke(p);

        // 获取有参方法
        Method eat_method2 = pClass.getMethod("eat", String.class);
        eat_method2.invoke(p, "饭");
        System.out.println("==============");

        // 获取所有public修饰的方法
        Method[] methods = pClass.getMethods();
        for (Method method : methods) {
            // 不仅仅有Person类中的方法，还有 Object类中的方法
            System.out.println(method);
            System.out.println(method.getName());
        }

        // 获取类名
        String className = pClass.getName();
        System.out.println(className); // com.my.domain.Person
    }
}

```

4. 获取类名
    * String getName() 



### 2.4 案例


* pro.properties配置文件

```java
className=com.my.domain.Student
methodName=sleep
```


* 代码：

```java
package com.my.reflect;

import java.io.IOException;
import java.io.InputStream;
import java.lang.reflect.Method;
import java.util.Properties;

/*
    需求：写一个“框架”，在不改变该类的任何代码的前提下
         可以帮助我们创建任意类的对象，并且执行其中任意方法
    实现方式：
        1.配置文件
        2.反射
     实现步骤：
        1.将需要创建的对象的全类名和需要执行的方法定义在配置文件中。
        2.在程序中加载读取配置文件。
        3.使用反射技术来加载类文件进内存
        4.创建对象
        5.获取方法对象
        6.执行方法
 */
public class ReflectPractise {
    public static void main(String[] args) throws Exception {
        // 1. 加载配置文件

        // 1.1 创建Properties对象
        Properties pro = new Properties();
        // 1.2 加载配置文件，转换为一个集合
        // 1.2.1 获取Class目录下的配置文件的方式:通过类加载器
        ClassLoader classLoader = ReflectPractise.class.getClassLoader();
        /*
        java.lang.Class<T>:
            ClassLoader getClassLoader​():返回类的类加载器。
            URL getResource​(String name):返回资源的路径
            InputStream getResourceAsStream​(String name):返回资源的Stream流
         */
        InputStream is = classLoader.getResourceAsStream("pro.properties");
        // load加载进内存
        pro.load(is);

        // 2.获取配置文件中定义的数据
        String className = pro.getProperty("className");
        String methodName = pro.getProperty("methodName");
        // 3.加载该类进内存
        Class cls = Class.forName(className);
        // 4.创建对象
        Object obj = cls.getConstructor().newInstance();
        // 5.获取方法对象
        Method method = cls.getMethod(methodName);
        // 6.执行方法
        method.invoke(obj);
    }
}
```




## 3. 注解

### 3.1 概述


* 概念：说明程序的。给计算机看的。
* 注释：用文字描述程序。给程序员看的。
* 定义：注解（Annotation），也叫元数据。一种代码级别的说明。它是JDK1.5及以后版本引入的一个特性，与类、接口、枚举是在同一个层次。它可以声明在包、类、字段、方法、局部变量、方法参数等的前面，用来对这些元素进行说明，注释。

* 概念描述：
    * JDK 1.5之后的新特性
    * 说明程序的
    * 使用注解：@注解名称
* 作用分类：
    ①编写文档：通过代码里标识的元数据生成文档【生成文档doc文档】
    ② 代码分析：通过代码里标识的元数据对代码进行分析【使用反射】
    ③编译检查：通过代码里标识的元数据让编译器能够实现基本的编译检查【Override】
    
    
### 3.2 JDK内置注解

* @Override：检测【被该注解标识】方法是否是继承自父类（接口）的。
* @Deprecated：该注解标注的内容，表示已过时。
* @SuppressWarnings：压制警告。
    * 一般传递参数 all: `@SuppressWarnings("all")`

```java
package com.my.annotation;

@SuppressWarnings("all")
public class AnnoDemo {
    @Override
    public String toString() {
        return super.toString();
    }

    @Deprecated
    public void show1() {

    }

    public void show2() {

    }
    
    public void demo() {
        show1();
    }
}
```

### 3.3 自定义的注解

- **格式**：
    - 元注解
    - `public @interface 注解名称{
    	属性列表;
    }`

* 本质：注解本质上就是一个接口，该接口默认继承 `Annotation`接口
    * public interface MyAnno extends java.lang.annotation.Annotation {}
* 属性：接口中的抽象方法
    * 要求：
        1. 属性的返回值类型有下列取值：
            * 基本数据类型
            * String
            * 枚举
            * 注解
            * 以上类型的数组
        2. 定义了属性，在使用时需要给属性赋值
            1. 如果定义属性时，使用`default`关键字给属性默认初始化值，则使用注解时，可以不进行属性的赋值。
            2. 如果只有一个属性需要赋值，并且属性的名称是`value`，则`value`可以省略，在使用时直接定义值即可。
            3. 数组赋值时，值使用 `{}`包裹。如果数组中只有一个值，则`{}`可以省略。

- **元注解**：用于描述注解的注解
    1.  @Target：描述注解能够作用的位置
        * ElementType取值：
            * TYPE：可以作用于类上。
            * METHOD：可以作用于方法上。
            * FIELD：可以作用于成员变量上。
    2. @Retention：描述注解被保留的阶段
        * @Retention(RetentionPolicy.RUNTIME)：当前被描述的注解，会保留到 class字节码文件中，并被 JVM读取到。
    3. @Documented：描述注解是否被抽取到 API文档中
    4. @Inherited:描述注解是否被子类继承


### 3.4 在程序中使用(解析)注解：获取注解定义的属性值

#### 使用方式：

1. 获取**注解定义的位置**的对象（Class， Method， Field）
2. 获取指定的注解
    * getAnnotation(Class)
```java
/*
其实就是在内存中生成了一个该注解接口的子类实现类对象
        public class ProImpl implements Prp {
            public String className() {
                return "com.my.annotation.Student";
            }

            public String methodName() {
                return "eat";
            }
*/
```

3. 调用注解中的抽象方法获取配置的属性值

#### 示例：

```java
public class Student {

    public void eat() {
        System.out.println("eat...food!");
    }
}

// -------------------------------

import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

/*
    描述需要执行的类名和方法名
 */
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
public @interface Pro {
    String className();

    String methodName();
}

// -------------------------------

import java.io.InputStream;
import java.lang.reflect.AnnotatedType;
import java.lang.reflect.Method;
import java.util.Properties;

/*
    需求：写一个“框架”，在不改变该类的任何代码的前提下
         可以帮助我们创建任意类的对象，并且执行其中任意方法
     不使用配置文件，使用注解来完成
 */
@Pro(className = "com.my.annotation.Student", methodName = "eat")
public class ReflectPractise {
    public static void main(String[] args) throws Exception {
        // 1.解析注解
        // 1.1 获取该类的字节码文件对象
        Class<ReflectPractise> reflectPractiseClass = ReflectPractise.class;
        // 2.获取上边的注解对象
        /*
           其实就是在内存中生成了一个该注解接口的子类实现类对象
        public class ProImpl implements Prp {
            public String className() {
                return "com.my.annotation.Student";
            }

            public String methodName() {
                return "eat";
            }
         */
        Pro anno = reflectPractiseClass.getAnnotation(Pro.class);
        // 3.调用注解对象中定义的抽象方法，获取返回值
        String className = anno.className();
        String methodName = anno.methodName();
        // 将该类加载进内存
        Class cls = Class.forName(className);
        // 创建对象
        Object obj = cls.getConstructor().newInstance();
        // 获取方法对象
        Method method = cls.getMethod(methodName);
        // 执行方法
        method.invoke(obj);
    }
}
```


### 3.5 案例：简单框架测试

- Calculator.java

```java
/*
    计算机类
 */
public class Calcluator {
    // 加法
    @Check
    public void add() {
        String str = null;
        str.toString();
        System.out.println("1 + 0 = " + (1 + 0));
    }

    // 减法
    @Check
    public void sub() {
        System.out.println("1 - 0 = " + (1 - 0));
    }

    // 乘法
    @Check
    public void mul() {
        System.out.println("1 * 0 = " + (1 * 0));
    }

    // 除法
    @Check
    public void div() {
        System.out.println("1 / 0 = "+ (1 / 0));
    }

    public void show() {
        System.out.println("永无bug！");
    }

}
```

- Check注解

```java
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface Check {

}
```

- TestCheck.java

```java
import java.io.BufferedWriter;
import java.io.FileWriter;
import java.io.IOException;
import java.lang.reflect.Method;

/**
 * 这是一个简单的测试框架
 * 当主方法执行后，会自动自行检测所有的方法（加了Check的方法）
 * 判断方法是否有异常，记录到文件中
 */
public class TestCheck {
    public static void main(String[] args) throws IOException {
        // 1.创建计算机对象
        Calcluator c = new Calcluator();
        // 2.获取字节码文件
        Class cls = c.getClass();
        // 3. 获取所有的方法
        Method[] methods = cls.getMethods();

        int number = 0; // 记录异常出现的次数
        BufferedWriter bw = new BufferedWriter(new FileWriter("bug.txt"));

        for (Method method : methods) {
            // 4. 判断方法上是否有指定的注解
            if (method.isAnnotationPresent(Check.class)) {
                // 5. 有就执行
                try {
                    method.invoke(c);
                } catch (Exception e) {
                    // 6. 捕获异常
                    // 记录到文件中
                    number++;
                    bw.write(method.getName()+" 方法出现异常了！");
                    bw.newLine();
                    bw.write("异常的名称:"+e.getCause().getClass().getSimpleName());
                    bw.newLine();
                    bw.write("异常的原因:" + e.getCause().getMessage());
                    bw.newLine();
                    bw.write("----------------------------");
                    bw.newLine();

                }
            }
        }
        bw.write("本次测试一个出现了"+number+"次异常.");
        bw.flush();
        bw.close();
    }
}

/*
add 方法出现异常了！
异常的名称:NullPointerException
异常的原因:null
----------------------------
div 方法出现异常了！
异常的名称:ArithmeticException
异常的原因:/ by zero
----------------------------
本次测试一个出现了2次异常.
*/
```

> 小结：
> 1. 以后大多时候，我们会使用注解，而不是自定义注解。
> 2. 注解给谁用？
    1. 编译器
    2. 给解析程序用
> 3. 注解不是程序的一部分，**可以理解为【注解】就是一个标签**。






