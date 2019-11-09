[TOC]

# Java IO流


## 1. IO概述

### 1.1 IO的分类

根据数据的流向分为：**输入流**和**输出流**。

+ **输入流**：把数据从`其他设备`上读取到`内存中`的流。
+ **输出流**：把数据从`内存中`写入`其他设备`上的流。
+ **I**:input 输入（读取）。
+ **O**：output 输出（写入）。
+ **流**：数据（字符、字节） 1 个字符 = 2 个字节，1 个字节 = 8 个二进制尾位。

![IO概述](https://ae01.alicdn.com/kf/H04eaff04ae5d4ab893f48e9cefe7fb84c.png)

### 1.2 顶级父类们

|        | 输入流                      | 输出流                       |
| ------ | --------------------------- | ---------------------------- |
| 字节流 | 字节输入流<br />InputStream | 字节输出流<br />OutputStream |
| 字符流 | 字符输入流<br />Reader      | 字符输出流<br />Writer       |


## 2. 字节流

### 2.1 一切皆为字节

一切文件数据（文本、图片、音频、视频等等）在存储的时候，都是以二进制数字的形式保存，都是一个一个的字节，那么传输时也一样如此。所以，字节流可以传输任意文件数据。在操作流的时候，我们要时刻明确，无论使用什么样的流对象，底层传输的始终为二进制数据。

### 2.2 字节输出流【OutputStream】

+ java.io.OutputStream：字节输出流：此抽象类是表示输出字节流的所有类的超类。

定义了一些子类共性的成员方法：

1. public void close()：关闭此输出流并释放与此流相联的任何系统资源。
2. public void flush()：刷新此输出流并强制任何缓冲的输出字节被写出。
3. public void write(byte[] b)：将b.length字节从指定的字节数组写入此输出流。
4. public void write(byte[] b, int off, int len)：从指定的字节数组写入 len字节，从偏移量 off开始输出至此输出流。
5. public abstract void write(int b)：将指定的字节输出流。

### 2.3 FileOutputStream类


- java.io.FileOutputStream extends OutputStream
- FileOutputStream：文件字节输出流
- 作用：把内存中的数据写入到硬盘的文件中。

#### 2.3.1 构造方法

- FileOutputStream(String name)：创建一个向具有指定名称的文件中写入数据的输出文件流。
- FileOutputStream(File file)：创建一个向指定 File对象表示的文件中写入数据的文件输出流。
    - 参数：写入数据的目的地
        - String name：目的地是一个文件的路径。
        - File file：目的地是一个文件。

**构造方法的作用**：

1. 创建一个FileOutputStream对象。
2. 会根据构造方法中传递的文件 / 文件路径，创建一个空的文件。
3. 会把FileOutputStream对象指向创建好的文件。

#### 2.3.2 写入字节数据

**写入数据的原理（内存 --> 硬盘）**：
java程序 --> JVM(java虚拟机) --> OS(操作系统) --> OS调用写数据的方法 --> 把数据写入到文件中。

**字节输出流的使用步骤（重要）**：
1. 创建一个FileOutputStream对象，构造方法中传递写入数据的目的地。
2. 调用FileOutputStream对象中的方法write，把数据写入到文件中。
3. 释放资源（流使用会占用一定的内存，使用完毕要把内存清空，提高程序效率）。

- 实例：

```java
import java.io.FileOutputStream;
import java.io.IOException;

public class Demo01OutputStream {
    public static void main(String[] args) throws IOException {
        // 1. 创建FileOutputStream对象，构造方法中传递写入数据的目的地
        FileOutputStream fos = new FileOutputStream("D:\\aaa\\abc.txt");
        // 2. 调用FileOutputStream中的方法，write，把数据写入到文件
        fos.write(97);
        // 3. 释放资源
        fos.close();
       /*
       写入数据的原理：
       写数据的时候会把十进制的97装换成二进制的整数
       硬盘中存储的都是数据的字节
       1 个字节 = 8 个比特位
       注意：
       任意的文本编辑器（记事本、notepad++...)
       在打开文件的时候，都会查询编码表，把字节装换成字符表示
       0~127：查询ASCII表
       其他值：查询系统默认码表（中文系统 GBK）
        */
    }
}

```

#### 2.3.3 一次写入多个字节的方法

- public void write(byte[] b)：将 b.length字节从指定的字节数组写入此输出流。
    - 一次写多个字节
    - 如果写的第一个字节是正数（0~127），那么显示的时候会查询ASCII表。
    - 如果写的第一个字节是负数，那么第一个字节会和第二个字节，两个字节组成一个中文显示，查询系统默认码表（GBK）
    - GBK中一个字是 2 个字节，UTF-8中一个字是 3 个字节。

- public void write(byte[] b, int off, int len)：把字节数组的一部分写入到文件中。
    - int off：数组的开始索引。
    - int len：写几个字节。

#### 2.3.4 写入字符串的方法
可以使用 String类中的方法 getBytes，把字符串装换为字节数组。

+ byte[] getBytes()：把字符串装换为字节数组。

+ 实例：

```java
import java.io.FileOutputStream;
import java.io.IOException;
import java.util.Arrays;

public class Demo02FileOutputStream {
    public static void main(String[] args) throws IOException {
        // 创建FileOutputStream对象，构造方法中传递目的地
        FileOutputStream fos = new FileOutputStream("D:\\aaa\\abc.txt");
        // public void write(byte[] b)
        byte[] bytes = {97, 98, 99, 100};
        fos.write(bytes); // abcd
//        System.out.println(bytes);
//        System.out.println(Arrays.toString(bytes));

        // public void write(byte[] b, int off, int len)
        fos.write(bytes, 1, 3); // abcdbcd

        byte[] bytes2 = {-48, -49, -50, -51, 65};
        System.out.println(Arrays.toString(bytes2));
        fos.write(bytes2); // abcdbcd邢瓮A

        // 写入字符串
        byte[] bytes3 = "你好".getBytes();
        System.out.println(Arrays.toString(bytes3));
        fos.write(bytes3);

        byte[] bytes4 = {-28, -67, -96, -27, -91, -67};
        fos.write(bytes4);
        // 释放资源
        fos.close();
    }
}
```

#### 2.3.5 数据追加续写

**追加写 / 续写**：使用两个参数的构造方法

- FileOutputStream(String name, boolean append) ：创建一个向具有指定 name 的文件中写入数据的输出文件流。
- FileOutputStream(File file, boolean append) ：创建一个向指定 File 对象表示的文件中写入数据的文件输出流。
    - 参数：
        - String name，File file：写入数据的目的地。
        - boolean append：**追加写开关**。
            - true：创建对象不会覆盖源文件，继续在文件的末尾追加写数据。
            - false：创建一个新的文件，覆盖源文件。

- 写换行：写换行符
    - windows：\r\n
    - Linux：/n
    - mac：/r

- 实例

```java
import java.io.FileOutputStream;
import java.io.IOException;

public class Demo03FileOutputStream {
    public static void main(String[] args) throws IOException {
        FileOutputStream fos = new FileOutputStream("D:\\aaa\\abc.txt", true);

        for (int i = 0; i < 10; i++) {
            fos.write("你好".getBytes());
            // windows 换行
            fos.write("\r\n".getBytes());
        }
        fos.close();
    }
}
```

### 2.4 字节输入流【InputStream】

- java.io.InputStream：字节输入流
- 此抽象类是表示字节输入流的所有类的超类。 

定义了所有子类共性的方法：
1. abstract int read()：从输入流中读取数据的下一个字节。
2. int read(byte[] b)：从输入流中读取一定数量的字节，并将其存储到缓冲区数组 b 中。
3. void close()：关闭此输入流并释放与该流关联的所有系统资源。


### 2.5 FileInputStream类

- java.io.FileInputStream extends InputStream
- FileInputStream：文件字节输入流。
- 作用：把硬盘中的数据读取到内存中使用。

#### 2.5.1 构造方法

- FileInputStream(String name)
- FileInputStream(File file)
    - 参数：读取文件的数据源
        - String name：读取文件的路径。
        - File file：文件

**构造方法的作用**：
1. 会创建一个FileInputStream对象。
2. 会把FileInputStream对象指向构造方法中要读取的文件。

#### 2.5.2 读取字节数据

**读取数据的原理**
java程序 --> JVM(java虚拟机) --> OS(操作系统) --> OS调用读数据的方法 --> 读取文件。

- int read()：读取文件中的一个字节并返回，读取到文件的末尾返回 **-1**。


**字节输入流的使用步骤（重点）**：
1. 创建FileInputStream对象，构造方法中绑定要读取的数据源。
2. 使用FileInputStream对象中的方法 read，读取文件，
3. 释放资源。

#### 2.5.3  一次读取一个字节的方法
+ abstract int read();
+ 示例：

```java
import java.io.FileInputStream;
import java.io.IOException;

public class Demo04FileInputStream {
    public static void main(String[] args) throws IOException {
        // 1. 创建FileInputStream对象
        FileInputStream fis = new FileInputStream("D:\\aaa\\123.txt");
        // 2. 调用FileInputStream中的方法read，读取文件
/*
        int len = fis.read();
        System.out.println(len);
*/
        /*
        布尔表达式(len = fis.read()) != -1)
            1. fis.read():读取一个字节
            2. len = fis.read():把读取的字节赋值给变量len
            3. (len = fis.read()) != -1):判断变量len是否不等于-1
        错误写法：
        while (fis.read() != -1) { // 读取
        System.out.println(fis.read());
        }
         */
        int len = 0; // 记录读取到的字节
        while ((len = fis.read()) != -1) {
            System.out.println((char) len);
        }

        // 3. 释放资源
        fis.close();
    }
}
```


#### 2.5.4  一次读取多个字节的方法

+ int read(byte[ ] b)：从输入流中读取一定数量的字节，并将其存储在缓冲区数组 b 中。

明确两件事情：
1. 方法的参数 byte[ ] 的作用？
    - 起到缓冲作用：存储每次读取到的多个字节。
    - 数组的长度一般定义为1024（1KB）或者1024的整数倍。
2. 方法的返回值 int 是什么？
    - 每次读取到的有效字节个数。

String类的构造方法
1. String(byte[ ] byte)：把字节数组转换为字符串。
2. String(byte[ ] byte, int offset, int length)：把字节数组的一部分转换为字符串，
   + offset：数组的开始索引。
   + length：转换的字节个数。

- 示例
```java
import java.io.FileInputStream;
import java.io.IOException;

public class Demo05FileInputStream {
    public static void main(String[] args) throws IOException {
        // 创建FileInputStream对象，构造方法中传递读取的数据源
        FileInputStream fis = new FileInputStream("D:\\aaa\\123.txt");
        // 创建一个字节数组，用来缓冲一次读取到的字节
        byte[] bytes = new byte[1024];
        // 调用FileInputStream中的方法，read读取数据，参数中传递bytes
/*
        int len = fis.read(bytes);
        System.out.println("len = " + len); // 2
        System.out.println(new String(bytes)); // ab

        len = fis.read(bytes);
        System.out.println("len = " + len); // 1
        System.out.println(new String(bytes)); // cb

        len = fis.read(bytes);
        System.out.println("len = " + len); // -1
        System.out.println(new String(bytes)); // cb
*/
        int len = 0; // 记录读取有效字节的个数
        while ((len = fis.read(bytes)) != -1) {
            System.out.println(new String(bytes, 0, len));
        }

        // 释放资源
        fis.close();
    }
}
```

## 3. 文件复制练习：一读一写

```java
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;

/*
    文件复制练习：一读一写
    明确：
        数据源：
        数据的目的地：
    文件复制的步骤：
        1. 创建一个字节输入流对象，构造方法中绑定要读取的数据源
        2. 创建一个字节输出流对象，构造方法中绑定要写入的目的地
        3. 使用字节输入流对象中的 read方法读取文件
        4. 使用字节输出流对象中的 write方法，把读取到的字节写入到目的地中
        5. 释放资源
 */
public class Demo05CopyFile {
    public static void main(String[] args) throws IOException {
        FileInputStream fis = new FileInputStream("C:\\Users\\孙忠杰\\Pictures\\Saved Pictures\\3.jpeg");
        FileOutputStream fos = new FileOutputStream("D:\\3.jpeg");

        // 计算文件复制所耗费的时间
        long start = System.currentTimeMillis();

        // 一次读取一个字节，写入一个字节的方式
        /*int len = 0;
        while ((len = fis.read()) != -1) {
            fos.write(len);
        }*/

        // 使用字节数组缓冲，一次读取多个字节，写入多个字节的方式
        int len = 0;
        // 创建字节数组，缓冲一次读取到的多个字节
        byte[] bytes = new byte[1024];
        while ((len = fis.read(bytes)) != -1) {
            fos.write(bytes, 0, len);
        }


        // 释放资源（先关写的，后关读的；如果写完了，肯定读取完毕了）
        fos.close();
        fis.close();

        long end = System.currentTimeMillis();
        System.out.println("文件复制耗费：" + (end - start) + "毫秒");

    }
}
```

## 4. 字符流

  当使用字节读取文本文件时，可能会有一个小问题，就是遇到中文字符时，可能不会显示完整的字符，那是因为一个中文可能占用多个字节存储。所以Java提供一些字符流类，以字符为单位读写数据，专门用于处理文本文件。

### 4.1 字符输入流【Reader】

- java.io.Reader：字符输入流，是字符输入流最顶层的父类，定义了一些共性的成员方法，是一个抽象类。


共性的成员方法：
1. int read()：读取单个字符并返回。
2. int read(char[ ] cbuf)：一次读取多个字符，将字符读入数组。
3. void close()：关闭该流并释放与之关联的所有资源。

### 4.2 FileReader类

- java.io.FileReader extends InputStreamReader extends Reader
- FileReader：文件字符输入流。
- 作用：把硬盘中的数据以字符的方法读取到内存中。

#### 4.2.1 构造方法

- FileReader(String fileName)
- FileReader(File file)
    - 参数：读取文件的数据源
        - String fileName：文件的路径
        - File file：一个文件
    

**FileReader构造方法的作用**
1. 创建一个FileReader对象。
2. 会把FileReader对象指向要读取的文件。

**字符输入流的使用步骤：**
1. 创建FileReader对象，构造方法中绑定要读取的数据源。
2. 使用FileReader对象中的方法 read 读取文件。
3. 释放资源

**String类中的构造方法：**

- String(char[ ] value)：把字符数组转换成字符串。
- String(char[ ] value, int offset, int count)：	把字符数组的一部分转换为字符串。
    - offset：数组的开始索引
    - count：转换的个数

#### 4.2.2 读取字符数据

```java
import java.io.FileReader;
import java.io.IOException;

public class Demo01Reader {
    public static void main(String[] args) throws IOException {
        // 1. 创建FileReader对象，绑定要读取的数据源
        FileReader fr = new FileReader("D:\\aaa\\123.txt");
        // 2. 调用FileReader中的方法 read，读取数据

        // 一次读取一个字符
/*
        int len = 0; // 记录读取到的字符
        while ((len = fr.read()) != -1) {
            System.out.println((char)len);
        }
*/

        // 一次读取多个字符
        char[] cs = new char[1024]; // 存储一次读取的多个字符
        int len = 0; // 记录读取的有效字符个数
        while ((len = fr.read(cs)) != -1) {
            System.out.println(new String(cs, 0, len));
        }
    }
}
```

### 4.3 字符输出流【Writer】

- java.io.Writer：字符输出流，是所有字符输出流的最顶层的父类，是一个抽象类

**共性的成员方法：**

1. void writer(int c)：写入单个字符。
2. void writer(char[ ] cbuf)：写入字符数组。
3. abstract void writer(char[ ] cbuf, int off, int len)：写入字符数组的一部分，off：数组的开始索引，len：写的字符个数。
4. void writer(String str)：写入字符串。
5. void writer(String str, int off, int len)：写入字符串的一部分，off：字符串的开始索引，len：写入字符的个数。
6. void flush()：刷新该流的缓冲区。
7. void close()：关闭此流，但要先刷新它。

### 4.4 FileWriter

- java.io.FileWriter extends OutputStreamWriter extends Writer
- FIleWriter：文件字符输出流。
- 作用：把内存中的字符数据写入到文件中。

#### 4.4.1 构造方法

- FileWriter(File file) ：根据给定的 File 对象构造一个 FileWriter 对象。
- FileWriter(String fileName) ：根据给定的文件名构造一个 FileWriter 对象。
- 参数：写入数据的目的地
    - String fileName：文件的路径。
    - File file：是一个文件。

**构造方法的作用：**
1. 会创建一个FileWriter对象。
2. 会根据构造方法中传递的文件 / 文件的路径，创建文件。
3. 会把FileWriter对象指向创建好的文件。

**字符输出流的使用步骤（重点）：**
1. 创建FileWriter对象，构造方法中绑定要写入数据的目的地。
2. 使用FileWriter中的方法 writer，把数据写入到内存缓冲区中（字符转换为字节的过程）。
3. 使用FileWriter中的方法 flush，把内存缓冲区中的数据，刷新到文件中。
4. 释放资源（会先把内存缓冲区中的数据刷新到文件中）。

+ 示例

```java
import java.io.FileWriter;
import java.io.IOException;

/*
FileWriter的使用步骤：
 */
public class Demo01Writer {
    public static void main(String[] args) throws IOException {
        //  1. 创建FileWriter对象，构造方法中绑定要写入数据的目的地
        FileWriter fw = new FileWriter("D:\\ideaproject\\itcast-code\\a.txt");
        // 2. 调用FileWriter中的方法，writer，将数据写入到内存缓冲区中（字符转换为字节的过程）
        fw.write(97);
        // 3. 调用FileWriter中的方法，flush，将内存缓冲区中的内容刷新到文件中
        fw.flush();
        // 4. 释放资源（会先把内存缓冲区中的数据刷新到文件中）
        fw.close();
    }
}
```

#### 4.4.2 关闭和刷新

**flush方法和close方法的区别：**

- flush：刷新缓冲区，流对象可以继续使用。
- close：先刷新缓冲区，然后通知系统释放资源。流对象不可以再被使用了。

```java
import java.io.FileWriter;
import java.io.IOException;

/*
FileWriter中 flush方法和 close方法的区别：
 */
public class DemoFlushAndClose {
    public static void main(String[] args) throws IOException {
        // 创建字符输出流对象，构造方法中绑定写入数据的目的地
        FileWriter fw = new FileWriter("D:\\ideaproject\\itcast-code\\b.txt");
        // 调用FileWriter中的方法 writer，将数据写入到内存缓冲区中
        fw.write(97);
        // 调用flush方法，刷新内存缓冲区
        fw.flush();

        // 继续写入数据
        fw.write(48);
        // 关闭流对象
        fw.close();
        // fw.write(65); // Stream closed
    }
}
```

#### 4.4.3 写数据的其他方法

1. void writer(char[ ] cbuf)：写入字符数组。
2. abstract void writer(char[ ] cbuf, int off, int len)：写入字符数组的一部分，off：数组的开始索引，len：写的字符个数。
3. void writer(String str)：写入字符串。
4. void writer(String str, int off, int len)：写入字符串的一部分，off：字符串的开始索引，len：写入字符的个数。

+ 示例：

```java
import java.io.FileWriter;
import java.io.IOException;

/*
    字符输出流写数据的其他方法：
    1. void writer(char[] cbuf)
    2. void writer(char[] cbuf, int off, int len)
    3. void writer(String str)
    4. void writer(String str, int off, int len)
 */
public class Demo02Writer {
    public static void main(String[] args) throws IOException {
        // 创建字符输出流对象
        FileWriter fw = new FileWriter("D:\\ideaproject\\itcast-code\\b.txt");
        // 创建一个字符数组
        char[] cs = new char[]{'a', 'b', 'c', 'd', 'e'};
        // void writer(char[] cbuf)
        fw.write(cs);
        // void writer(char[] cbuf, int off, int len)
        // 写字符数组的一部分
        fw.write(cs, 1, 2);
        // void writer(String str)
        fw.write("传智播客");
        // void writer(String str, int off, int len)
        fw.write("黑马程序员", 2, 3);
        // 释放资源
        fw.close();
    }
}
```

#### 4.4.4 续写和换行

续写（追加写）：使用两个参数的构造方法
1. FileWriter(String fileName, boolean append)
2. FileWriter(File file, boolean append)
    + 参数：
        + String fileName，File file：写入数据的目的地。
        + boolean append：续写开关 
            + true：不会创建新文件覆盖源文件，可以续写。
            + false：创建新的文件覆盖源文件。

换行：换行符号

+ windows：`\r\n`
+ Linux：`\n`
+ mac：`\r`

> 当我们单纯读 或者 写文本文件时，使用字符流，其他情况使用字节流。


## 5.  IO异常的处理

### 5.1 在 JDK1.7 之前使用 try catch finally 处理流中的异常

+ 格式：
```java
	try {
		可能会出现异常的代码
	} catch (异常类型 变量名称) {
		异常的处理逻辑
	} finally {
		一定会执行的代码
		（资源释放）
	}
	
```

+ 示例

```java
import java.io.FileWriter;
import java.io.IOException;

/*
在jdk1.7之前，使用 try cathc finally 处理异常
 */
public class DemoTryCatchFinally {
    public static void main(String[] args) {
        /*
         提高变量 fw 的作用域，让finally可以使用
         【局部变量在定义的时候，可以没有值，但是使用的时候必须有值】

         new FileWriter("w:\\ideaproject\\itcast-code\\b.txt", true);
         执行失败，fw没有值，fw.close 会报错
         */
        FileWriter fw = null;
        try {
            fw = new FileWriter("w:\\ideaproject\\itcast-code\\b.txt", true);
            fw.write("hello java");
        } catch (IOException e) {
            System.out.println(e);

        } finally {
            // 创建对象失败，fw的给定值为null，null是不能调用方法的，
            // 会抛出NullPointerException，需要增加一个判断，不是null，把资源释放
            if (fw != null) {
                try {
                    // fw.close方法声明抛出了IOException异常，所以我们要处理这个异常对象，要么throws，要么try catch
                    fw.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

### 5.2 JDK 7 的新特性

  在try的后边可以增加一个()，在括号中可以定义流对象，那么这个流对象的作用域就在 try中有效，try中的代码执行完毕，会自动把流对象释放，不用写 finally

+ 格式：

```java
	try (定义流对象;定义流对象...) {
		可能会产生异常的代码
	} catch (异常类型 变量名称) {
		异常的处理逻辑
	}
```

+ 示例：

```java
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
/*
JDK 1.7 的新特性
 */
public class DemoJDK7 {
    public static void main(String[] args) {
        // 流对象的定义
        // 可能会产生异常的代码
        try (FileInputStream fis = new FileInputStream("C:\\Users\\孙忠杰\\Pictures\\Saved Pictures\\3.jpeg");
             FileOutputStream fos = new FileOutputStream("D:\\3.jpeg");) {
            // 一次读取一个字节，写入一个字节的方式
            int len = 0;
            while ((len = fis.read()) != -1) {
                fos.write(len);
            }
        } catch(IOException e){
            // 异常的处理逻辑
            System.out.println(e);
        }
    }
}
```

### 5.3 JDK 9 的新特性


  try的前边可以定义流对象，在try后边的**()**中可以直接引入流对象的名称（变量名），在try代码执行完毕之后，流对象也可以自动释放掉，不用写finally

+ 格式：
```java
        A a = new A();
        B b = new B();
        try (a;b) {
            // 可能产生异常的代码
        } catch (异常类型 变量名称) {
            // 异常的处理逻辑
        }
```

+ 示例：

```java
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;

/*
    JDK9新特性
    try的前边可以定义流对象
    在try后边的()中可以直接引入流对象的名称（变量名）
    在try代码执行完毕之后，流对象也可以自动释放掉，不用写finally
    格式：
        A a = new A();
        B b = new B();
        try (a;b) {
            // 可能产生异常的代码
        } catch (异常类型 变量名称) {
            // 异常的处理逻辑
        }
 */
public class DemoJDK9 {
    public static void main(String[] args) throws FileNotFoundException {
        // 1. 创建输入流对象，构造方法中绑定读取数据的目的地
        FileInputStream fis = new FileInputStream("D:\\3.jpeg");
        // 2. 创建输出流对象，构造方法中绑定写入数据的目的地
        FileOutputStream fos = new FileOutputStream("d:\\aaa\\3.jpeg");
        // 引入流对象
        try (fis; fos) {
            // 定义一个字节数组，用来缓冲数据
            byte[] bytes = new byte[1024];
            // 记录读取字节的有效个数
            int len = 0;
            while ((len = fis.read(bytes)) != -1) {
                fos.write(bytes, 0, len);
            }
        } catch (IOException e) {
            System.out.println(e);
        }
    }
}
```

## 6. 属性集

### 6.1 概述
  `java.util.Properties`继承于`Hashtable`，用来表示一个持久的属性集。它使用键值结构存储数据，每个键及其对应的值都是一个字符串，该类也被许多Java类使用，比如获取系统属性的时，`System.getProperties()`方法返回的就是一个`Properties`对象。

### 6.2 Properties类

+ java.util.Properties 集合 extends Hashtable<K, V> implements Map<K, V>
+ Properties 类表示一个持久的属性集。Properties 可保存在流中或从流中加载。
+ Properties 集合是唯一一个和IO流向结合的集合
    + 可以使用Properties集合中的方法 **store**，把集合中的**临时数据**，**持久化**写入到硬盘中存储。
    + 可以使用Properties集合中的方法**load**，把**硬盘中保存的的文件（键值对）**，**读取**到集合中使用。
+ 属性列表中每个键及其对应的值都是一个字符串。
+ Peoperties集合是一个双列集合，key和value默认都是字符串。

#### 6.2.1 构造方法

+ `public Properties()`：创建一个空的属性列表

#### 6.2.2 基本的存储方法

+ 使用Properties集合存储数据，遍历取出Properties集合中的数据
+ Properties集合是一个双列集合，key和value默认都是字符串
+ Properties集合有一些操作字符串特有的方法：
    1. Object setProperty(String key, String value)：调用 Hashtable 的方法 put。
    2. Set`<String>` stringPropertyNames()：返回此属性列表中的键集，其中该键及其对应值是字符串，此方法相当于Map集合中的keySet方法
    3.  String getProperty(String key)：通过key找到value值，此方法相当于Map集合中的get(key)方法


+ 示例：
```java
import java.util.Properties;
import java.util.Set;

public class Demo01Properties {
    public static void main(String[] args) {
        // 创建Properties集合对象
        Properties prop = new Properties();
        // 使用 setProperty往集合中添加元素
        prop.setProperty("赵丽颖", "168");
        prop.setProperty("迪丽热巴", "165");
        prop.setProperty("马儿扎哈", "170");
        // 使用stringPropertiesNames 把Properties集合中的键取出，存储到一个Set集合中
        Set<String> set = prop.stringPropertyNames();
        // 遍历Set集合,取出Properties集合中的每一个键
        for (String key : set) {
            String value = prop.getProperty(key);
            System.out.println(key + " = " + value);
        }
    }
}
```

#### 6.2.3 store方法

+ 可以使用Properties集合中的 store方法，把集合中的临时数据，持久化写入到硬盘中存储。
+ void store(Writer writer, String comments) 
+ void store(OutputStream out, String comments) 
+ 参数：
    + OutputStream out：字节输出流，不能写中文。
    + Writer writer：字符输出流，可以写中文。
    + String comments：注释，用来解释说明保存的文件是做什么用的。不能使用中文，会产生乱码，默认是Unicode编码，一般使用**" "空字符串**。

**使用步骤：**
1. 创建Properties集合对象，添加数据。
2. 创建字节输出流 / 字符输出流对象，构造方法中绑定要输出的目的地。
3. 使用Properties集合中的 store方法，把集合中的临时数据，持久化写入到硬盘中存储。
4. 释放资源。

+ 示例：

```java
import java.io.FileOutputStream;
import java.io.FileWriter;
import java.io.IOException;
import java.util.Properties;

public class Demo02Properties {
    public static void main(String[] args) throws IOException {
        // 创建Properties集合对象
        Properties prop = new Properties();
        // 使用 setProperty往集合中添加元素
        prop.setProperty("赵丽颖", "168");
        prop.setProperty("迪丽热巴", "165");
        prop.setProperty("马儿扎哈", "170");

/*
        // 创建字符输出流对象，构造方法中绑定要输出的目的地
        FileWriter fw = new FileWriter("D:\\ideaproject\\itcast-code\\prop.txt");
        // 使用Properties中的方法，store，临时数据 --> 硬盘
        prop.store(fw, "save data");
        // 释放资源
        fw.close();
*/
        // 使用字节输出流输出中文，出现乱码
        prop.store(new FileOutputStream("D:\\ideaproject\\itcast-code\\a.txt"), "");
    }
}
```


#### 6.2.3 load方法

+ 可以使用Properties集合中的方法**load**，把**硬盘中保存的的文件（键值对）**，**读取**到集合中使用。
+ void load(InputStream inStream) 
+ void load(Reader reader) 
+ 参数:
    + InputStream inStream:字节输入流,不能读取含有中文的键值对.
    + Reader reader:字符输入流,能读取含有中文的键值对.

**使用步骤:**
1. 创建Properties集合对象.
2. 使用Properties集合中的方法 **load**,读取保存键值对的文件.
3. 遍历Properties集合

> 注意事项:
> 1. 存储键值对的文件中,键与值的默认连接符号可以使用**=, 空格(其他符号)**
> 2. 存储键值对的文件中,可以使用**#**进行注释,被注释的键值对不会再被读取.
> 3. 存储键值对的文件中,键与值默认都是字符串,不用再加引号.
> 

+ 示例:
```java
import java.io.FileReader;
import java.io.IOException;
import java.util.Properties;
import java.util.Set;

public class Demo03Load {
    public static void main(String[] args) throws IOException {
        // 创建Properties对象
        Properties prop = new Properties();
        // 调用 load方法，读取硬盘中的数据
        prop.load(new FileReader("D:\\ideaproject\\itcast-code\\prop.txt"));
        // 遍历集合
        Set<String> set = prop.stringPropertyNames();
        for (String key : set) {
            String value = prop.getProperty(key);
            System.out.println(key + "=" + value);
        }
    }
}
```

## 7. 字节缓冲流

  能够高速读写的缓冲流，能够转换编码的转换流，能够持久化存储对象的序列化流等等，这些功能强大的流，都是在基本的流对象基础之上创建而来的，就像穿上铠甲的武士一样，相当于是对基本流对象的一种增强。

### 7.1 概述

  缓冲流，也叫高效流，是对4个基本的 FileXxx流的增强，所以也是4个流，按照数据类型分类：

- **字节缓冲流**：`BufferedInputStream`，`BufferedOutputStream`
- **字符缓冲流**：`BufferedReader`，`BufferedWriter`
- 缓冲流的基本原理：是在创建流对象时，会创建一个内置大小的缓冲区数组，通过缓冲区读写，减少系统IO次数，从而提高读写的效率。

![缓冲流原理](https://ae01.alicdn.com/kf/Hf977402d4fdd41bd8ac54833fccd2b7cn.jpg)


### 7.2 BufferedOutputStream类

- java.io.BufferedOutputStream extends OutputStream
- BufferedOutputStream：字节缓冲输入流。

#### 7.2.1 继承父类的共性成员方法
1. public void close()：关闭此输出流并释放与此流相联的任何系统资源。
2. public void flush()：刷新此输出流并强制任何缓冲的输出字节被写出。
3. public void write(byte[] b)：将b.length字节从指定的字节数组写入此输出流。
4. public void write(byte[] b, int off, int len)：从指定的字节数组写入 len字节，从偏移量 off开始输出至此输出流。
5. public abstract void write(int b)：将指定的字节输出流。

#### 7.2.2 构造方法

- BufferedOutputStream(OutputStream out) ：创建一个新的缓冲输出流，以将数据写入指定的底层输出流。 
- BufferedOutputStream(OutputStream out, int size) ：创建一个新的缓冲输出流，以将具有指定缓冲区大小的数据写入指定的底层输出流。 
- 参数：
    - OutputStream out：字节输出流。我们可以传递FileOutputStream对象，缓冲流会给FileOutputStream增加一个缓冲区，提高FileOutputStream的写入效率。
    - int size：指定缓冲流内部缓冲区数组的大小，不指定默认。

**使用步骤（重点）：**
1. 创建FileOutputStream对象，构造方法中绑定要输出数据的目的地。
2. 创建BufferedOutputStream对象，构造方法中传递FileOutputStream对象，提高FileOutputStrem的写入效率。
3. 使用BufferedOutputStream对象中的方法 writer，把数据写入到内部缓冲区中。
4. 使用BufferedOutputStream对象中的方法 flush，把内部缓冲区中的数据，刷新到文件中。
5. 释放资源（会先调用 flush方法，刷新数据，第4步可以省略）。

- 示例
```java
import java.io.BufferedOutputStream;
import java.io.FileOutputStream;
import java.io.IOException;

public class Demo01BufferedOutputStream {
    public static void main(String[] args) throws IOException {
        // 1. 创建FileOutputStream对象，构造方法中传递要输出数据的目的地
        FileOutputStream fos = new FileOutputStream("itcast-code\\a.txt");
        // 2. 创建BufferedOutputStream对象，构造方法中传递FileOutputStream对象，提高写入的效率
        BufferedOutputStream bos = new BufferedOutputStream(fos);
        // 3. 调用BufferedOutputStream对象的方法，write写入数据
        bos.write("我把数据写入到内部缓冲区中".getBytes());
        // 4. 调用flush方法，刷新缓冲区
        bos.flush();
        // 5. 释放资源
        bos.close();
    }
}
```

### 7.3 BufferedInputStream类

- java.io.BufferedInputStream extends InputStream
- BufferedInputStream：字节缓冲输入流。

#### 7.3.1 继承自父类的方法：

1. abstract int read()：从输入流中读取数据的下一个字节。
2. int read(byte[] b)：从输入流中读取一定数量的字节，并将其存储到缓冲区数组 b 中。
3. void close()：关闭此输入流并释放与该流关联的所有系统资源。

#### 7.3.2构造方法

- BufferedInputStream(InputStream in) ：创建一个 BufferedInputStream 并保存其参数，即输入流 in，以便将来使用。 
- BufferedInputStream(InputStream in, int size) ：创建具有指定缓冲区大小的 BufferedInputStream 并保存其参数，即输入流 in，以便将来使用。 
- 参数：
    - InputStream in：字节输入流。我们可以传递FileInputStream对象，缓冲流会给FileInputStream增加一个缓冲区，提高FileInputStream的读取效率。
    - int size：指定缓冲流内部缓冲区数组的大小，不指定默认。

**使用步骤（重点）：**
1. 创建FileInputStream对象，构造方法中绑定要读取的数据源.。
2. 创建BufferedInputStream对象，构造方法中传递FileInputStream对象，提高FileInputStrem的读取效率。
3. 使用BufferedInputStream对象中的方法 read，读取文件。
4. 释放资源（会先调用 flush方法，刷新数据）。


+ 示例

```java
import java.io.BufferedInputStream;
import java.io.FileInputStream;
import java.io.IOException;

public class Demo02BufferedInputStream {
    public static void main(String[] args) throws IOException {
        // 1. 创建FileInputStream对象，构造方法中绑定读取的数据源
        FileInputStream fis = new FileInputStream("itcast-code\\a.txt");
        // 2. 创建BufferedInputStream对象，构造方法中传递FileInputStream对象
        BufferedInputStream bis = new BufferedInputStream(fis);
        // 3. 调用BufferedInputStream中的方法，read，读取文件

        // 一次读取一个字节的方式
/*
        int len = 0; // 记录每次读取字节的有效个数
        while ((len = bis.read()) != -1) {
            System.out.println((char)len);
        }
*/
        // 一次读取多个字节的方式
        int len = 0; // 记录每次读取字节的有效个数
        // 用来缓冲读取的字节
        byte[] bytes = new byte[1024];
        // 读取到文件末尾时，返回-1
        while ((len = bis.read(bytes)) != -1) {
            System.out.println(new String(bytes, 0, len));
        }
    }
}
```

## 8. 文件复制练习(增强版 使用缓冲流)

```java
import java.io.*;

/*
使用缓冲流完成
    文件复制练习：一读一写
    明确：
        数据源：
        数据的目的地：
    文件复制的步骤：
        1. 创建字节缓冲输入流对象，构造方法中传递字节输入流对象。
        2. 创建字节缓冲输出流对象，构造方法中传递字节输出流对像.
        3. 使用字节缓冲输入流中的方法，read，读取文件。
        4. 使用字节缓冲输出流中的方法 writer，把读取到的数据写入到内部缓冲区中。
        5. 释放资源（会先把缓冲区中的数据，刷新到文件中）
 */
public class Demo03CopuFIle {
    public static void main(String[] args) throws IOException {
		// 记录开始时间
        long start = System.currentTimeMillis();
        // 1. 创建字节缓冲输入流对象
        BufferedInputStream bis = new BufferedInputStream(new FileInputStream("C:\\Users\\孙忠杰\\Pictures\\Saved Pictures\\3.jpeg"));
        // 2. 创建字节缓冲输出流对象
        BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream("D:\\3.jpeg"));
        // 3. 调用 read方法，读取文件
        // 一次读取一个字节的方式
        /*int len = 0;
        while ((len = bis.read()) != -1) {
            // 调用 write方法，写入数据
            bos.write(len);
        }*/
        // 使用字节数组缓冲一次读取到的多个字节
        byte[] bytes = new byte[1024];
        int len = 0;
        while ((len = bis.read(bytes)) != -1) {
            bos.write(bytes, 0, len);
        }

        // 5. 释放资源
        bos.close();
        bis.close();
		// 结束时间
        long end = System.currentTimeMillis();
        System.out.println("文件复制耗费：" + (end - start) + "毫秒");
    }
}
```

## 9. 字符缓冲流

- java.io.BufferedWriter extends Writer
- BufferedWriter：字符缓冲输出流。

### 9.1 BufferedWriter类

#### 9.1.1继承自父类的共性方法

1. void writer(int c)：写入单个字符。
2. void writer(char[ ] cbuf)：写入字符数组。
3. abstract void writer(char[ ] cbuf, int off, int len)：写入字符数组的一部分，off：数组的开始索引，len：写的字符个数。
4. void writer(String str)：写入字符串。
5. void writer(String str, int off, int len)：写入字符串的一部分，off：字符串的开始索引，len：写入字符的个数。
6. void flush()：刷新该流的缓冲区。
7. void close()：关闭此流，但要先刷新它。

#### 9.1.2构造方法

- BufferedWriter(Writer out) ：创建一个使用默认大小输出缓冲区的缓冲字符输出流。 
- BufferedWriter(Writer out, int sz) ：创建一个使用给定大小输出缓冲区的新缓冲字符输出流。 
- 参数：
    - Writer out：字符输出流。我们可以传递 FileWriter对象，缓冲流会给 FileWriter增加一个缓冲区，提高 FileWriter的写入效率。
    - int sz：指定缓冲区的大小，不写默认大小。

#### 9.1.3特有的成员方法

- void newLine() ：写入一个行分隔符。 会根据不同的操作系统，获取不同的行分隔符。
- 换行：换行符号
    - Windows：\r\n
    - Linux：/n
    - mac：/r

**使用步骤:**
1. 创建字符缓冲输出流对象，构造方法中传递字符输出流。
2. 调用字符缓冲输出流对象中的方法 writer，把数据写入到内存缓冲区中。
3. 调用字符缓冲输出流对象中的方法 flush，把内存缓冲区中的数据，刷新到文件中。
4. 释放资源。


- 示例：

```java
import java.io.BufferedWriter;
import java.io.FileWriter;
import java.io.IOException;

public class Demo04BufferedWriter {
    public static void main(String[] args) {

        try (// 1. 创建字符缓冲输出流对象，构造方法中绑定字符输出流对象
             BufferedWriter bw = new BufferedWriter(new FileWriter("itcast-code\\b.txt"));) {
            for (int i = 0; i < 10; i++) {
                // 2. 调用字符缓冲输出流对象中的方法，writer，把数据写入到内存缓冲区中
                bw.write("我爱学习java，耶耶耶！");
                // 使用特有的方法，newLine()，换行
                bw.newLine();
            }
            // 3. 调用字符缓冲输出流对象中的方法，flush，把内存缓冲区中的数据，刷新到文件中
            bw.flush();
        } catch (IOException e) {
            System.out.println(e);
        }

        // 4. 释放资源
        // bw.close();
    }
}
```

### 9.2 BufferedReader类

- java.io.BufferedReader extends Reader
- BufferedReader：字符缓冲输入流

#### 9.2.1 继承自父类的共性方法

1. int read()：读取单个字符并返回。
2. int read(char[ ] cbuf)：一次读取多个字符，将字符读入数组。
3. void close()：关闭该流并释放与之关联的所有资源。

#### 9.2.2 构造方法

- BufferedReader(Reader in) ：创建一个使用默认大小输入缓冲区的缓冲字符输入流。 
- BufferedReader(Reader in, int sz) ：创建一个使用指定大小输入缓冲区的缓冲字符输入流。 
- 参数：
    - Reader in：字符输入流。我们可以传递 FileReader，缓冲流会给 FileReader增加一个缓冲区，提高 FileReader的读取效率。
    - int sz：指定缓冲区的大小，不写默认大小。

#### 9.2.3 特有的成员方法

- String readLine() ：读取一个文本行。读取一行数据。
- 通过下列字符之一即可认为某行已终止：换行 ('\n')、回车 ('\r') 或回车后直接跟着换行 ( \r\n )。 
- 返回值：
    - 包含该行内容的字符串，不包含任何行终止符，如果已到达流末尾，则返回 **null**。

**使用步骤：**
1. 创建字符缓冲输入流对象，构造方法中传递字符输入流对象。
2. 使用字符输入流对象中的方法，read / readLine，读取文本。
3. 释放资源。

- 示例：

```java
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class Demo05BufferedReader {
    public static void main(String[] args) throws IOException {
        // 1. 创建字符缓冲输入流对象，构造方法中绑定字符输入流对象
        BufferedReader br = new BufferedReader(new FileReader("itcast-code\\b.txt"));
        /*
            在不知道文本中有多少行时，使用while循环
            while的结束条件，读取到 null时结束
         */
        // 2. 使用字符缓冲输入流对象中的方法，read / readLine，读取文本
        String line;
        while ((line = br.readLine()) != null) {
            System.out.println(line);
        }
        // 3. 释放资源
        br.close();
    }
}
```

## 10. 练习：文本排序

```java
import java.io.*;
import java.util.HashMap;
import java.util.Iterator;
import java.util.Map;
import java.util.Set;
/*
    练习：
        对文本的内容进行排序
        按照（1，2，3，4...)顺序排序
    分析：
        1. 创建一个HashMap集合对象
            key:存储每行的文本序号(1,2,3...)
            value:存储每行的文本
        2. 创建字符缓冲输入流对象，构造方法中绑定字符输入流对象
        3. 创建字符缓冲输入流对象，构造方法中绑定字符输出流对象
        4. 调用字符缓冲输入流中的方法，readLine，逐行读取文本
        5. 对读取到的文本进行切割，获取行中的序号和文本内容
        6. 把切割好的序号和文本内容存储到HashMap集合中(key是有序的，会自动排序1,2,3..)
        7. 遍历HashMap集合，获取每一个键值对
        8. 把每一个键值对，拼接成一个文本行
        9. 把拼接好的文本行，使用字符缓冲输入流的方法write，写入文件中
        10. 释放资源
 */
public class Demo06Test {
    public static void main(String[] args) throws IOException {
        // 1. 创建HashMap集合,key：序号，value：文本内容
        HashMap<String, String> map = new HashMap<>();
        // 2. 创建字符缓冲输入流对象
        BufferedReader br = new BufferedReader(new FileReader("itcast-code\\in.txt"));
        // 3. 创建字符缓冲输入流对象
        BufferedWriter bw = new BufferedWriter(new FileWriter("itcast-code\\out.txt"));
        // 4. 使用字符缓冲输入流中的方法，readLine方法，读取文本内容
        String line;
        while ((line = br.readLine()) != null) {
            // 5. 对读取到的字符串进行切割，key：序号，value：文本内容
            String[] arr = line.split("\\.");
            // 6. 把切割好的字符串放入到HashMap集合中
            map.put(arr[0], arr[1]);
        }
        // 7. 遍历HashMap集合，获取键值对
        Set<Map.Entry<String, String>> entrySet = map.entrySet();
        // 遍历Set集合的迭代器
        Iterator<Map.Entry<String, String>> it = entrySet.iterator();
        while (it.hasNext()) {
            Map.Entry<String, String> entry = it.next();
            String key = entry.getKey();
            String value = entry.getValue();
            // 8. 对键值对进行拼接
            line = key + "." + value;
            // 9. 使用字符缓冲输出流对象，输出到文件中
            bw.write(line);
            // 换行
            bw.newLine();
        }
        // 10. 释放资源
        bw.close();
        br.close();
    }
}
```


## 11. 转换流

### 11.1 字符编码和字符集

#### 11.1.1 字符编码

- **编码**：字符（能看懂的）--> 字节（看不懂的）。
- **解码**：字节（看不懂的）--> 字符（能看懂的）。 
- **字符编码（Character Encoding)**：就是一套自然语言的字符与二进制数之间的对应规则。
    - **编码表**：生活中的文字和计算机中二进制的对应规则。

#### 11.1.2 字符集

- **字符集（CharSet）**：也叫编码表，是一个系统支持的所有字符的集合，包括各国家的文字、标点符号、图形符号、数字等。
- **ASCII字符集**：
    - 基本的ASCII字符集，使用7位（bits）表示一个字符，共128个字符。ASCII的扩展字符集使用8位（bits）表示一个字符，共256个字符，方便支持欧洲常用字符。
- **ISO-8859-1字符集**：
    - 使用单字节编码，兼容ASCII编码。
- **GBxxx字符集**：
    - GB就是国标的意思，是为了显示中文而设计的一套字符集。
    - **GB2312**：简体中文码表。
    - **GBK**：最常用的中文码表，用2个字节表示一个中文。
    - **GB18030**：最新的中文码表，采用多字节编码，每个字可以由1、2、4个字节组成。
- **Unicode字符集**
    - 万国码
    - 它最多使用4个字节的数字表达字符。有三种编码方案：UTF-8、UTF-16、UTF-32。最为常用的是UTF-8。
    - **UTF-8编码**：
        1. 128个US-ASCII字符，只需一个字节编码。
        2. 拉丁文等字符，需要2个字节编码。
        3. 大部分常用字（含中文），使用3个字节编码。
        4. 其他极少使用的Unicode辅助字符，使用4个字节编码。


### 11.2 编码引出的问题

  在IDEA中，使用`FileReader`读取项目中的文本文件。由于IDEA的设置，都是默认的`UTF-8`编码，所以没有任何问题。但是当读取**Windows系统**中创建的文本文件时，由于Windows系统的默认是`GBK`，就会出现**乱码**。

- 示例：

```java
// 乱码问题
import java.io.FileReader;
import java.io.IOException;

public class Test {
    public static void main(String[] args) throws IOException {
        FileReader fr = new FileReader("D:\\ideaproject\\itcast-code\\a.txt");
        int len = 0;
        while ((len = fr.read()) != -1) {
            System.out.print((char)len);
        }
        fr.close();
    }
}
/*
输出结果：
��ã����磡
 */
```

### 11.3 转换流的原理

- **FileReader类**：
    - 用来读取字符文件的便捷类。
    - 此类的构造方法假定默认字符编码和默认字节缓冲区大小都是适当的。
    - 要自己指定这些值，可以先在 FileInputStream 上构造一个 InputStreamReader。

- **FileWriter类**：
    - 来写入字符文件的便捷类。
    - 此类的构造方法假定默认字符编码和默认字节缓冲区大小都是可接受的。
    - 要自己指定这些值，可以先在 FileOutputStream 上构造一个 OutputStreamWriter。

### 11.4 OutputStreamWriter类

- java.io.OutputStreamWriter extends Writer
- OutputStreamWriter：是字符流通向字节流的桥梁；可使用指定的 `charset` 将要写入流中的字符编码成字节。(编码：把能看懂的 --> 看不懂的)

#### 11.4.1 继承父类的共性成员方法

1. void writer(int c)：写入单个字符。
2. void writer(char[ ] cbuf)：写入字符数组。
3. abstract void writer(char[ ] cbuf, int off, int len)：写入字符数组的一部分，off：数组的开始索引，len：写的字符个数。
4. void writer(String str)：写入字符串。
5. void writer(String str, int off, int len)：写入字符串的一部分，off：字符串的开始索引，len：写入字符的个数。
6. void flush()：刷新该流的缓冲区。
7. void close()：关闭此流，但要先刷新它。

#### 11.4.2 构造方法

- OutputStreamWriter(OutputStream out) ：创建使用默认字符编码的 OutputStreamWriter。
- OutputStreamWriter(OutputStream out, String charsetName) ：创建使用指定字符集的 OutputStreamWriter。
- 参数：
    - OutputStream out：字节输出流，可以用来写转换之后的字节到文件中。
    - String charsetName：指定编码表的名称，不区分大小写，可以是：utf-8 / UTF-8，gbk / GBK，...。不指定，默认使用 utf-8。

**使用步骤：**

1. 创建OutputStreamWriter对象，构造方法中传递**字节输出流对象**和**指定编码表名称**。
2. 使用OutputStreamWriter对象中的方法 writer，把字符转换为字节，存储在缓冲区中（编码）。
3. 使用OutputStreamWriter对象中的方法 flush，把内存缓冲区中的字节刷新到文件中（使用字节流写的过程）。
4. 释放资源。

- 示例：

```java
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.OutputStreamWriter;

public class Demo01OutputStreamWriter {
    public static void main(String[] args) throws IOException {
        // write_utf_8();
        write_gbk();

    }

    private static void write_gbk() throws IOException {
       OutputStreamWriter osw = new OutputStreamWriter(new FileOutputStream("itcast-code\\gbk.txt"), "gbk");
       osw.write("你好"); // 3个字节
       osw.flush();
       osw.close();
    }

    private static void write_utf_8() throws IOException {
        // 1. 创建OutputStreamWriter对象，构造方法中传递字节输出流对象和指定编码表名称
        OutputStreamWriter osw = new OutputStreamWriter(new FileOutputStream("itcast-code\\utf_8.txt"), "utf-8");
        // 2. 使用OutputStreamWriter对象中的方法，writer，把字符转换成字节，存储在缓冲区中。（编码）
        osw.write("你好"); // 2个字节
        // 3. 调用 flush方法，刷新数据到文件中、
        osw.flush();
        // 4. 释放资源
        osw.close();
    }
}
```

### 11.5 InputStreamReader类

- java.InputStreamReader extends Reader
- InputStreamReader：是字节流通向字符流的桥梁；它使用指定的 charset 读取字节并将其解码为字符（解码：把看不懂的 --> 能看懂的）。


#### 11.5.1 继承自父类的共性方法

1. int read()：读取单个字符并返回。
2. int read(char[ ] cbuf)：一次读取多个字符，将字符读入数组。
3. void close()：关闭该流并释放与之关联的所有资源。

#### 11.5.2 构造方法

- InputStreamReader(InputStream in) ：创建一个使用默认字符集的 InputStreamReader。
- InputStreamReader(InputStream in, String charsetName) ：创建使用指定字符集的 InputStreamReader
- 参数：
    - InputStream in：字节输入流，用来读取文件中保存的字节。
    - String charsetName：指定的编码表名称，不区分大小写，可以是：utf-8 / UTF-8，gbk / GBK，...。不指定，默认使用 utf-8。

**使用步骤：**
1. 创建InputStreamReader对象，构造方法中传递字节输入流对象和指定编码表名称。
2. 使用InputStreamReader对象中的方法，read，读取文件。
3. 释放资源。

> 注意事项：
> 构造方法中传递的编码表名称要和文件的编码表相同，否则会产生乱码。
> 

- 示例

```java
import java.io.FileInputStream;
import java.io.IOException;
import java.io.InputStreamReader;

public class Demo03InputStreamReader {
    public static void main(String[] args) throws IOException {
        // read_utf_8();
        read_gbk();

    }

    private static void read_gbk() throws IOException{
        // 1.创建InputStreamReader对象，构造方法中传递字节输入流对象和指定的编码表名称
        InputStreamReader isr = new InputStreamReader(new FileInputStream("itcast-code\\utf_8.txt"), "gbk");
        // 2.使用InputStreamReadr中的方法，read，读取文件。
        int len = 0; // 记录读取到字符的有效个数
        // 使用字符数组缓冲读取到的多个字符
        char[] cs = new char[1024];
        while ((len = isr.read(cs)) != -1) {
            System.out.println(new String(cs, 0, len));
        }
        // 3.释放资源
        isr.close();
    }

    private static void read_utf_8() throws IOException {
        // 1.创建InputStreamReader对象，构造方法中传递字节输入流对象和指定的编码表名称
        InputStreamReader isr = new InputStreamReader(new FileInputStream("itcast-code\\utf_8.txt"));
        // 2.使用InputStreamReadr中的方法，read，读取文件。
        int len = 0; // 记录读取到字符的有效个数
        // 使用字符数组缓冲读取到的多个字符
        char[] cs = new char[1024];
        while ((len = isr.read(cs)) != -1) {
            System.out.println(new String(cs, 0, len));
        }
        // 3.释放资源
        isr.close();
    }
}
```

## 12. 练习：转换文件编码

```java
import java.io.*;

/*
练习：转换文件编码
    将gbk编码的文件，转换为utf-8编码的文件。
分析：
    1.创建InputStreamReader对象，构造方法中传递字节输入流对象和指定gbk编码
    2.创建OutputStreamWriter对象，构造方法中传递字节输出流对象和指定utf-8编码
    3.使用InputStreamReader对象的方法 read，读取数据到缓冲区中
    4.使用OutputStreamWriter对象的方法 writer，将数据写入到文件中
    5.释放资源
 */
public class Demo02Test {
    public static void main(String[] args) throws IOException {
        InputStreamReader isr = new InputStreamReader(new FileInputStream("itcast-code\\gbk.txt"),"gbk");
        OutputStreamWriter osw = new OutputStreamWriter(new FileOutputStream("itcast-code\\utf_8.txt"), "utf-8");
        int len = 0;
        while ((len = isr.read()) != -1) {
            osw.write(len);
        }
        osw.close();
        isr.close();

    }
}
```


## 13. 序列化&反序列化

### 13.1 序列化&反序列化概述


![序列化和反序列化概述](https://ae01.alicdn.com/kf/H00004fcfa89c456789d9996dfb40d9734.jpg)


### 13.2 ObjectOutputStream类

- java.io.ObjectOutputStream extends OutputStream
- ObjectOutputStream：对象的序列化流
- 作用：把对象以流的方式写入到文件中保存。

#### 13.2.1 构造方法

- ObjectOutputStream(OutputStream out)：创建写入指定 OutputStream 的 ObjectOutputStream。
- 参数：
    - OutputStream out：字节输出流。

#### 13.2.2 特有的成员方法

- void writeObject(Object obj) ：将指定的对象写入 ObjectOutputStream。 

**使用步骤：**
1. 创建 ObjectOutputStream对象，构造方法中传递字节输出流。
2. 使用 ObjectOutputStream对象中的方法，writeObject，把对象写入到文件中。
3. 释放资源。

#### 13.2.3 示例

- Person.java

```java
import java.io.Serializable;
/*
    序列化和反序列化的时候，会抛出 NotSerializableException没有序列化异常
    类通过实现 java.io.Serializable 接口以启用其序列化功能。
    未实现此接口的类将无法使其任何状态序列化或反序列化。
    Serializable接口也叫标记型接口
        要进行序列化和反序列化的类，必须实现 Serializable接口，就会给类添加一个标记
        当我们进行序列化和反序列化的时候，就会检测类上是否有这个标记
            有：可以序列化和反序列化
            没有：就会抛出 NotSerializableException异常
 */
public class Person implements Serializable {
    private String name;
    private int age;

    public Person() {
    }

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }

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
```

- Demo01ObjectOutputStream.java


```java
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.ObjectOutputStream;

public class Demo01ObjectOutputStream {
    public static void main(String[] args) throws IOException {
        Person one = new Person("小美女", 18);
        // 1.创建ObjectOutputSream对象，构造方法中传递字节输出流
        ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("itcast-code\\person.txt"));
        // 2.使用ObjectOutputstream中的方法，writerObject，将对象写入到文件中
        oos.writeObject(one);
        // 3.释放资源
        oos.close();
    }
}
```

### 13.3 ObjectInputStream类

- java.io.ObjectInputStream extends InputStream
- ObjectInputStream：对象的反序列化流。
- 作用：把文件中保存的对象，以流的方式读取出来使用。

#### 13.3.1 构造方法

- ObjectInputStream(InputStream in) ：创建从指定 InputStream 读取的 ObjectInputStream。
- 参数：
    - InputStream in：字节输入流

#### 13.3.2 特有的成员方法

-  Object readObject() ：从 ObjectInputStream 读取对象。

**使用步骤：**
1. 创建ObjectInputStream对象，构造方法中传递字节输入流。
2. 使用ObjectInputStream对象中的方法 readObject读取保存对象的文件。
3. 释放资源。

> 注意事项：
> **readObject方法**声明抛出了 **ClassNotFoundException（class文件找不到异常）**
> 当不存在对象的 class文件时抛出此异常。
> 反序列化的前提：
> 1. 类必须实现 **Serializable接口**
> 2. 必须存在类对应的 **class文件**
> 

+ 示例

```java
import java.io.FileInputStream;
import java.io.IOException;
import java.io.ObjectInputStream;

public class Demo03ObjectInputStream {
    public static void main(String[] args) throws IOException, ClassNotFoundException {
        // 1.创建ObjectInputStream对象，构造方法中传递字节输入流
        ObjectInputStream ois = new ObjectInputStream(new FileInputStream("itcast-code\\person.txt"));
        // 2.使用ObjectInputStream中的方法，readObject,读取文件中保存的对象
        Object o = ois.readObject();
        System.out.println(o);
        Person one = (Person)o;
        System.out.println(one.getName()+one.getAge());
        // 3.释放资源
        ois.close();
    }
}
```

### 13.4 transient关键字

- **static关键字：静态关键字**
    - 静态优于非静态加载到内存中（静态优先于对象进入到内存中）。
    - 被 static修饰的成员变量不能被序列化，序列化的都是对象。

```java
private static int age;
oos.writeObject(new Person("小美女", 18));
Object o = ois.readObject();
Person{name='小美女', age=0}
```

- **transient关键字：瞬态关键字**
    - 被 transien修饰的成员变量，不能被序列化。

```java
private transient int age;
oos.writeObject(new Person("小美女", 18));
Object o = ois.readObject();
Person{name='小美女', age=0}
```

### 13.5 InvalidClassException异常

- 编译器（javac.exe）会把 Person.java文件编译成生成 Person.class 文件，Person类实现了 **Serializable接口**，就会根据类的定义，给 Person.class文件添加一个**序列号（serialVersionUID）**。

- 反序列化的时候，会使用Person.class文件中的序列号和Person.txt文件中的序列号进行比较，如果是一样的，则反序列化成功；如果不一样，则抛出**序列化冲突异常：InvalidClassException**。
- 修改了类的定义，那么就会给 Person.class 文件重新编译生成一个新的序列号（serialVersionUID）。

- 问题：
    - 每次修改了类的定义，都会给class文件重新生成一个新的序列号。
- 解决方案：
    - 无论是否对类的定义进行修改，都不重新生成新的序列号。
    - 可以手动给类添加一个序列号。
- 格式在 Serializable接口内有规定：
    - 可序列化类可以通过声明名为 "serialVersionUID" 的字段（该字段必须是静态 (static)、最终 (final) 的 long 型字段）显式声明其自己的 serialVersionUID：
    - `static final long serialVersionUID = 42L;`

## 14. 练习：序列化集合

```java
import java.io.*;
import java.util.ArrayList;

/*
练习：序列化集合
    当我们想在文件中保存多个对象的时候，可以把多个对象存储到一个集合中
    对集合进行序列化和反序列化
分析：
    1.定义一个存储Person对象的ArrayList集合
    2.往ArrayList集合中添加多个对象
    3.创建一个序列化ObjectOutputStream对象
    4.使用ObjectOutputStream对象中的 writeObject方法，对集合进行序列化
    5.创建一个反序列化ObjectInputStream对象
    6.使用ObjectInputStream对象中的方法 readObject方法，读取文件中保存的集合
    7.把 Object类型的集合强转为 ArrayList类型
    8.遍历ArrayList集合
    9.释放资源
 */
public class Demo02Pracitse {
    public static void main(String[] args) throws IOException, ClassNotFoundException {
        // 创建集合
        ArrayList<Person> list = new ArrayList<>();
        // 添加元素
        list.add(new Person("周元", 20));
        list.add(new Person("夭夭", 21));
        list.add(new Person("苏幼薇", 19));
        // 创建序列化流对象
        ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("itcast-code\\list.txt"));
        // 调用 writeObject方法，将集合写入
        oos.writeObject(list);
        // 创建反序列化流对象
        ObjectInputStream ois = new ObjectInputStream(new FileInputStream("itcast-code\\list.txt"));
        // 调用 readObject方法，读取集合
        Object o = ois.readObject();
        // 将 Object类型强转为 Arraylist类型
        ArrayList<Person> personArrayList = (ArrayList<Person>)o;
        // 遍历集合
        for (Person p : personArrayList) {
            System.out.println(p);
        }
        // 释放资源
        oos.close();
        ois.close();
    }
}
```

## 15. 打印流

### 15.1 概述

- java.io.PrintStream extends FilterOutputStream extends OutputStream
- java.io.PrintStream：打印流
- PrintStream：为其他输出流添加了功能，使它们能够方便地打印各种数据值表示形式。
- PrintStream特点：
    1. 只负责数据的输出，不负责数据的读取。
    2. 与其他输出流不同，PrintStream 永远不会抛出 IOException，但会抛出 FileNotFoundException。
    3. 有特有的方法：print，println
        - void print(任意类型的值)
        - void println(任意类型的值并换行)

### 15.2 构造方法

- PrintStream(File file) ：创建具有指定文件且不带自动行刷新的新打印流。
- PrintStream(OutputStream out) ：创建新的打印流。
- PrintStream(String fileName) ：创建具有指定文件名称且不带自动行刷新的新打印流。

### 15.3 继承自父类的共性方法

1. public void close()：关闭此输出流并释放与此流相联的任何系统资源。
2. public void flush()：刷新此输出流并强制任何缓冲的输出字节被写出。
3. public void write(byte[] b)：将b.length字节从指定的字节数组写入此输出流。
4. public void write(byte[] b, int off, int len)：从指定的字节数组写入 len字节，从偏移量 off开始输出至此输出流。
5. public abstract void write(int b)：将指定的字节输出流。

> 注意事项：
> 1. 如果使用继承自父类的 write方法写数据，那么查看数据的时候，会查询编码表：97 --> a。
> 2. 如果使用自己**特有的方法 print / println方法**写数据，那么写的数据会原样输出：97 --> 97。
> 

```java
import java.io.FileNotFoundException;
import java.io.PrintStream;

public class Demo01PrintStream {
    public static void main(String[] args) throws FileNotFoundException {
        // 1.创建PrintStream对象，构造方法中绑定输出的目的地
        PrintStream ps = new PrintStream("itcast-code\\print.txt");
        // 2.调用父类的 write方法
        ps.write(48); // 0
        // 2.调用自己的 print / println 方法
        ps.println();
        ps.println(97); // 97
        // 3.释放资源
        ps.close();
    }
}
```


### 15.4 改变输出流的流向

- 可以改变输出语句的目的地（打印流的流向）。
- 输出语句，默认在控制台输出。
- 使用 System.setOut 方法该变输出语句的目的地，目的地修改为PrintStream构造方法中传递的目的地。
- static void setOut(PrintStream out) ：重新分配“标准”输出流。 

```java
import java.io.FileNotFoundException;
import java.io.PrintStream;

public class Demo02PrintStream {
    public static void main(String[] args) throws FileNotFoundException {
        PrintStream ps = new PrintStream("itcast-code\\print.txt");
        // 在控制台输出
        System.out.println("我在控制台输出，哈哈哈");
        // 改变输出流的流向
        System.setOut(ps);
        // 这条语句在文件中原样输出
        System.out.println("我在print.txt中输出，呵呵呵");
        ps.close();
    }
}
```

