[TOC]

# Java 异常

## 1. 异常概念

异常：指的是在程序执行过程中，出现的非正常情况，最终会导致JVM的非正常停止。
在Java等面向对象的编程语言中，异常本身是一个类，产生异常就是创建异常对象并抛出一个异常对象。Java处理异常的方式是**中断处理**。

> 注意：异常指的并不是语法错误。语法错了，编译不通过，不会产生字节码文件，根本不能运行。
> 


## 2. 异常体系

异常的根类是java.lang.Throwable，其下有两个子类：
java.lang.Error与java.lang.Exception，平常说的异常就是java.lang.Exception。


![异常体系](https://ae01.alicdn.com/kf/H2e103957e3ad4add8d45031361aa63eb1.png)


## 3. 异常分类

+ java.lang.Throwable类是java语言中所有错误或异常的超类。
+ Exception：编译器异常，进行编译（写代码）java程序出现的问题。
+ public class RuntimeException extends Exception
+ RuntimeException：运行期异常，java程序运行出现的问题。
+ 示例：

```java
public class DemoException {
    public static void main(String[] args) /*throws ParseException*/ {
        /*SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
        Date date = null;
        try {
            date = sdf.parse("19991013");
        } catch (ParseException e) {
            e.printStackTrace();
        }
        System.out.println(date);*/

        int[] arr = {1, 2, 3};
        try {
            System.out.println(arr[3]);
        }catch (Exception e){
            System.out.println(e);
        }
        System.out.println("================");
//        int[] arr2 = new int[1024*1024*1024]; // OutOfMemoryError
        
    }
}
```

## 4. 异常的产生过程解析

以下面这段代码示例：

```java
public class DemoExceptionParse{
	public static void main(String [] args){
		// 创建int数组，并赋值
		int[] arr = {1, 2, 3};
		int e = getElement(arr, 3);
		System.out.println(e);
	}
	/*
		定义一个方法返回数组指定索引处的元素
		参数：
				int[] arr
				int index
	*/
	public static int getElement(int[] arr, int index){
		return arr[index];
	}
}
```

运行结果：

```java
Exception in thread "main" java.lang.ArrayIndexOutOfBoundsException: Index 3 out of bounds for length 3
	at com.my.package7.DemoExceptionParse.getElement(DemoExceptionParse.java:10)
	at com.my.package7.DemoExceptionParse.main(DemoExceptionParse.java:6)
```

分析：

一、访问了数组中的3索引，而数组是没有3索引的，JVM就会检测出程序出现异常。
JVM会做两件事：

1. JVM会根据异常产生的原因，创建一个异常对象，这个异常对象包含了异常产生的**（内容，原因，位置）**
`new ArrayIndexOutOfBoundsException("3");`
2. 在getElement方法中，没有异常的处理逻辑（try catch），那么JVM就会把异常对象抛给方法的调用者，也就是main方法来处理这个异常。

二、main方法接受到了这个异常对象，如果main方法也没有处理这个异常的逻辑，如果没有会继续把这个异常对象抛给main方法的调用者JVM处理。

三、JVM接受到这个异常对象，做了两件事：

1. 把异常对象**（内容，原因，位置）**以红色字体打印在控制台。
2. JVM会终止当前正在执行的java程序 --> **中断处理**


简而言之：
1. 方法把异常对象抛给main方法
2. main方法再把异常对象抛给JVM
3. JVM将异常对象的内容，原因，位置打印在控制台，并中断当前程序的运行。


## 5. 异常的处理

java异常处理的五个关键字：**try、catch、finally、throw、throws**

### throw

+ 作用：可以使用throw关键字在指定的方法中抛出指定的异常。
+ 格式：`throw new xxxException("异常产生的原因");

> 注意事项：
> 1. throw关键字必须写在方法的内部。
> 2. throw关键字后边new的对象必须是Exception或者Exception的子类对象。
> 3. throw关键字抛出指定的异常对象，我们就必须处理这个异常对象。
>> 1. throw关键字后边创建的是RuntimeException或者RuntimeException的子类对象，我们可以不处理，默认交给JVM处理（打印异常对象，中断程序）
>> 2. throw关键字后边创建的是**编译异常**（写代码的时候报错），我们就必须处理这个异常，要么**throws**，要么**try...catch**。
> 

+ 示例

```java
public class Demo03Throw{
	public static void main(String [] args){
		int[] arr = new int[3];
		int e = getElement(arr, 3);
		System.out.println(e);
		
	}
	public static int getElement(int[]arr, int index){
		if (arr == null){
			throw new NullPointerException("传递的数组的值为null");
		}
		if (index < 0 || index >= arr.length){
			throw new ArrayIndexOutOfBoundsException("传递的索引超出数组的使用范围");
		}
		return arr[index];
	}
}
```


### Objects非空判断 requireNonNull方法

+ public static <T> T requireNonNull(T Obj)：查看指定对象不是null

+ 源码：


```java
public static <T> T requireNonNull(T obj){
	if (obj == null){
		throw new NullPointerException();
	}
	return obj;
}
```

+ 示例：

```java
import java.util.Objects;

public class Demo04Objects{
	public static void main(String [] args){
		method(null);
	}
	public static Object method(Object obj){
		/* if (obj == null){
			throw new NullPointerException("传递的对象值为null");
		}*/
		Objects.requireNonNull(obj,"传递的对象值为null");
		return obj;
		
	}
}
```

### 声明异常throws

+ throws关键字：异常（编译）处理的第一种方式，交给别人处理。

+ 作用：当方法内部抛出异常对象时，我们就必须处理这个异常对象，可以使用 **throws**关键字处理异常对象，会把异常对象声明抛出给方法的调用者（自己不处理，交给别人），最终交给**JVM处理 --> 中断处理**。

+ 格式：（在方法声明时使用）

```java
修饰符 返回值类型 方法名称 (参数列表) throws AAAException, BBBException ... { 
	throw new AAAException("产生原因");
	throw new BBBException("产生原因");
	...
}
```


> 注意事项：
> 1. throws关键字必须写在方法的声明处。
> 2. throws关键字后边声明的异常必须是 Exception或者Exception的子类。
> 3. 方法内部如果抛出了多个异常对象，那么 throws后边必须也声明多个异常。
> > 如果抛出的多个异常对象有子父类关系，那么直接声明父类异常即可,但是对使用的子类异常还是要导包的。
> 4. 调用了一个声明抛出异常的方法,我们就必须处理声明的异常.
> > 要么继续使用 throws声明抛出,交给方法的调用者,最终交给 JVM处理.
> > 要么 try ... catch 自己处理.

+ 示例:

```java
/*FileNotFoundException extends IOException extends Exception*/

import java.io.IOException;
import java.io.FileNotFoundException;

public class Demo05Throws {
	public static void main(String [] args) throws Exception{
		readFile("c:\\a.docx");
	}
	public static void readFile(String fileName) throws Exception{
		/*if (!fileName.equals("c:\\a.txt")){
			throw new FileNotFoundException("文件路径c:\\a.txt没有找到");
		}*/
		if (!fileName.endsWith(".txt")){
			throw new IOException("文件名后缀不是.txt");
		}
		System.out.println("读取文件成功");
	}
}
```


### 捕获异常 try ... catch

+ try ... catch:异常处理的第二种方式,自己处理.

+ 格式:

```java
try {
	可能产生异常的代码
} catch (异常类型 变量名称) {
	异常的处理逻辑
} catch  (异常类型 变量名称) {
	异常的处理逻辑
} 
...
```

> 注意事项:
> 1. try 中可能会抛出多个异常对象,那么就可以使用多个 catch来捕获这些异常对象.
> 2. 如果 try中产生了异常,那么就会执行catch中的异常处理逻辑,执行完毕后继续执行 try...catch后面的代码.
> 3. 如果 try中没有产生异常,那么就不会执行catch中的异常处理逻辑,而是执行 try...catch后面的代码.
> 

+ 示例:

```java
import java.io.IOException;

public class Demo06TryCatch{
	public static void main(String [] args){
		try{
			readFile("c:\\a.docx");
		}catch (IOException/*Exception*/ e){
			System.out.println("传递的文件名后缀不是.txt");
		}
		System.out.println("后续代码");
	}
	public static void readFile(String fileName) throws IOException/*Exception*/{
		if (!fileName.endsWith(".txt")){
			throw new IOException("文件名后缀不是.txt");
		}
		System.out.println("读取文件成功");
	}
}
```

### Throwable类中3个异常处理的方法

1. String getMessage():返回此 Throwable的简短描述.
2. String toString():返回此 Throwable的详细消息字符串.
3. void printStackTrace():JVM打印异常对象,默认此方法,打印的异常信息是最全面的.

### finally 代码块


**finally**:有一些特定的代码,无论异常是否发生,都需要执行,另外,因为异常会引发程序跳转,导致有些语句执行不到.而finally语句就是为了解决这个问题的,在finally代码块中存放的代码是一定会被执行到的.

+ 格式:

```java
try {
	可能产生异常的代码
} catch (异常类型 变量名称) {
	异常的处理逻辑
}
...
catch  (异常类型 变量名称) {
	异常的处理逻辑
} finally{
	无论是否出现异常都会执行
}

```

> 注意事项：
> 1. finally 不能单独使用，必须和 try一起使用。
> 2. finally一般用于资源释放（资源回收），无论程序是否出现异常，最后都要资源释放（IO）。


## 6. 异常注意事项

### 多异常的捕获处理

+ 多个异常使用捕获又该如何处理？
    1. 多个异常分别处理。
    2. 多个异常一次捕获，多次处理。
    3. 多个异常一次捕获，一次处理。

一般我们使用一次捕获，多次处理的方式。

+ 示例：


```java
// 1. 多个异常分别处理
        try{
            int[] arr = {1, 2, 3};
            System.out.println(arr[3]);
        } catch (ArrayIndexOutOfBoundsException e){
            System.out.println(e);
        }
        try {
            List<Integer> list = new ArrayList<>();
            Collections.addAll(list, 1, 2, 3);
            System.out.println(list.get(3));
        } catch (IndexOutOfBoundsException e) {
            System.out.println(e);
        }
        
        
// 2. 多个异常，一次捕获，多次处理
        try {
            int [] arr = {1, 2, 3};
//            System.out.println(arr[3]);
            ArrayList<Integer> list = new ArrayList<>();
            Collections.addAll(list, 1, 2, 3);
            System.out.println(list.get(3));
        } catch (ArrayIndexOutOfBoundsException e) {
            System.out.println(e);
        } catch (IndexOutOfBoundsException e){
            System.out.println(e);
        }

// 3. 多个异常，一次捕获，一次处理
        try {
            int [] arr = {1, 2, 3};
            //System.out.println(arr[3]);
            ArrayList<Integer> list = new ArrayList<>();
            Collections.addAll(list, 1, 2, 3);
            System.out.println(list.get(3));
        } catch (Exception /*IndexOutOfBoundsException*/ e) {
            System.out.println(e);
        }
        
        /* java.lang.ArrayIndexOutOfBoundsException
            List<Integer> list = List.of(1, 2, 3);
            System.out.println(list.get(3)); */
```

+ 一个 try 多个 catch注意事项：
catch里边定义的异常变量，如果有子父类关系，那么**子类的异常变量必须写在父类异常变量的上边**，否则就会报错。

ArrayIndexOutOfBoundsException extends IndexOutOfBoundsException extends Exception

+ ***运行时异常被抛出可以不处理，即不捕获处理也不声明抛出。默认给虚拟据 JVM处理，终止程序***


### finally中有return语句

如果 finally中有 return语句，永远返回finally中的结果，避免该情况。

### 子父类异常

父类异常时什么样，子类异常就什么样。

1. 如果父类抛出了多了异常，子类覆盖父类方法时，只能抛出相同的异常或者它的子集。
2. 父类方法没有抛出异常，子类覆盖父类该方法时也不可抛出异常，此时子类产生该异常，**只能捕获处理，不能声明抛出**。

## 7. 自定义异常

+ 格式：

```java
修饰符 class 类名称Exception extends Exception / RuntimeException {
	添加一个空参的构造方法
	添加一个带异常信息的构造方法
}
```

> 注意事项：
> 1. 自定义异常类一般都以 Exception结尾，说明该类是一个异常类。
> 2. 自定义异常类必须继承 Exception 或者 RuntimeException 
> Exception：编译期异常，必须处理异常，要么 throws，要么 try ... catch。
> RuntimeException：运行期异常，无需处理，交给 JVM虚拟机处理即可。


+ 练习

```java
/*
模拟注册操作，如果用户名已经存在，则抛出异常并提示：亲，您的用户名已经被注册了
分析：
1， 创建一个String类型数组用来存储已经注册的用户名（数据库）
2. 使用Scanner类获取用户输入的用户名（前端，页面）
3. 使用一个方法，遍历已经存在的存储用户名的数组,判断是否存在
 */

import java.util.Scanner;

public class Demo01ExceptionPractise {
    static String[] userNames = {"聂离", "杜泽", "周元"};

    public static void main(String[] args) /*throws RegisterException*/ {
        Scanner sc = new Scanner(System.in);
        System.out.println("请输入您要注册的用户名：");
        String username = sc.next();
        checkUseName(username);
    }

    /*
    用来判断输入的用户名是否已经被注册
    true：抛出RegisterException, 输出：“亲，该用户名已经被注册了”
    false：输出：“恭喜您，注册成功”
     */
    public static void checkUseName(String username) /*throws RegisterException*/ {
        for (String name : userNames) {
            if (name.equals(username)){
                try {
                    throw new RegisterException("亲，该用户名已经被注册了");
                } catch (RegisterException e) {
                    e.printStackTrace();
                    return;
                }
            }
        }
        System.out.println("恭喜您，注册成功”");
    }
}

```