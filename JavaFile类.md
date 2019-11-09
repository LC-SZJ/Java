[TOC]

# Java File类

## 1. 概述

+ java.io.File类
+ 文件和目录路径名的抽象表示形式。
+ java把电脑中的文件和文件夹（目录）封装成为了一个 File 类，我们可以使用File类对文件和文件夹进行操作。

**我们可以使用 File 类的方法：**

+ 创建一个文件 / 文件夹
+ 删除文件 / 文件夹
+ 获取文件 / 文件夹
+ 判断文件 / 文件夹 是否存在
+ 对文件夹进行遍历
+ 获取文件的大小

**File类是一个与操作系统无关的类，任何的操作系统都可以使用这个类中的方法。**

**重点：记住三个单词 **

+ file：文件
+ directory：文件夹 / 目录
+ path：路径

## 2. File类的静态成员变量

+ static String pathSeparator：与系统有关的路径分隔符，为了方便，它被表示为一个字符串。 
+ static char pathSeparatorChar ：与系统有关的路径分隔符。 
+ static String separator ：与系统有关的默认名称分隔符，为了方便，它被表示为一个字符串。 
+ static char separatorChar：与系统有关的默认名称分隔符。 

```java
/*
路径分隔符：
windows：分号 ；
Linux：冒号 ：
文件名称分隔符：
windows：反斜杠 \
Linux：正斜杠 /
 */
import java.io.File;
public class Demo01File {
    public static void main(String[] args) {
        String pathSeparator = File.pathSeparator;
        System.out.println(pathSeparator); // ;
        /*
        操作路径：路径不能写死了
        c:\demo\a\a.txt     windows
        c:/demo/a/a.txt     Linux
       "c:"+File.separator+"demo"+File.separator + "a"+File.separator+"a.txt"
         */
        String separator = File.separator;
        System.out.println(separator); // \
    }
}
```

## 3. 路径

1. 路径是不区分大小写的。
2. 路径中的文件名称分隔符，windows使用反斜杠，反斜杠是转义字符，两个反斜杠代表一个普通的反斜杠。

## 4. File类的构造方法

1. File(String pathname) ：通过将给定路径名字符串转换为抽象路径名来创建一个新 File 实例。
    + 参数：
        + String pathname：字符串的路径名称
    + 注意：
        + 路径可以是以文件结尾，也可以是以文件夹结尾。
        + 路径可以是相对路径，也可以是绝对路径。
        + 路径可以是存在的，也可以是不存在的。
        + 创建 File对象，只是把字符串路径封装成File对象，不考虑路径的真假情况。


2. File(String parent, String child)：根据 parent 路径名字符串和 child 路径名字符串创建一个新 File 实例。
    + 参数：把路径分成了两部分
        + String parent：父路径
        + String child：子路径
    + 好处：父路经和子路径可以单独书写，使用起来非常灵活；父路径和子路径都可以变化。

3. File(File parent, String child)：根据 parent 抽象路径名和 child 路径名字符串创建一个新 File 实例。
    + 参数：把路径分成了两部分
        + File parent：父路径
        + String child：子路径
    + 好处：
        + 父路经和子路径可以单独书写，使用起来非常灵活；父路径和子路径都可以变化。
        + 父路径是File类型，可以使用File的方法对路径进行一些操作，再使用路径创建对象。


## 4. 常用方法

### 获取功能的方法

1. String getAbsolutePath() ：返回此抽象路径名的绝对路径名字符串。
    + 获取构造方法中传递的路径，无论路径是绝对的，还是相对的，getAbsolutePath方法返回的都是绝对路径。 
2. String getPath() ：将此抽象路径名转换为一个路径名字符串。 
    + 获取**构造方法**中传递的路径.
3. String getName() ：返回由此抽象路径名表示的文件或目录的名称。 
    + 获取的是构造方法传递路径的结尾部分（文件 / 文件夹）。
4. long length() ：返回由此抽象路径名表示的文件的长度。 
    + 获取的是构造方法中指定文件的大小，以字节为单位。
    + **注意**：
        + 文件夹是没有大小概念的，不能获取文件夹的大小。
        + 如果构造方法中给出的路径不存在，那么length方法返回0。

### 判断功能的方法

1. boolean exists()：测试此抽象路径名表示的文件或目录是否存在。 
    + 用于判断构造方法中的路径是否存在
        + 存在：true
        + 不存在：false
2. boolean isDirectory() ：测试此抽象路径名表示的文件是否是一个目录。 
    + 用于判断构造方法中的路径是否以文件夹结尾
        + 是：true
        + 不是：false
3. boolean isFile() ：测试此抽象路径名表示的文件是否是一个标准文件。 
    + 用于判断构造方法中的路径是否以文件夹结尾
        + 是：false
        + 不是：true

> 注意事项：
> 电脑的硬盘中只有 文件 / 文件夹，两个方法是互斥。
> 这两个方法使用的前提：路径必须是存在的，否则都返回 false
> 

### 创建和删除功能的方法

1. boolean createNewFile() ：当且仅当不存在具有此抽象路径名指定名称的文件时，不可分地创建一个新的空文件。 
    + 创建文件的路径和名称在构造方法中给出（构造方法的参数）。
    + 返回值：布尔值
        + true：文件不存在，创建文件。
        + false：文件已存在，不会创建文件。
    + **注意：**
        1. 此方法只能创建文件，不能创建文件夹。
        2. 创建文件的路径必须存在，否则会抛出异常。(IOException)
2. boolean mkdir() ：创建此抽象路径名指定的目录。 
    + 创建单级空文件夹。
3. boolean mkdirs() ：创建此抽象路径名指定的目录，包括所有必需但不存在的父目录。 
    + 既可以创建单级空文件夹，又可以创建多级空文件夹。
    + 创建文件夹的路径和名称在构造方法中给出（构造方法的参数）。
    + 返回值：布尔值
        + true：文件夹不存在，创建文件夹。
        + false：文件夹已存在，不会创建文件夹。
    + **注意：**
        1. 此方法只能创建文件夹，不能创建文件。
        2. 创建文件夹的路径如果不存在，会自动创建文件夹（不要被文件的名称迷惑，要看文件类型）。

4. boolean delete() ：删除此抽象路径名表示的文件或目录。 
    + 删除构造方法中给出的文件 / 文件夹
    + 返回值：布尔值
        + true：文件 / 文件夹删除成功。
        + false：***文件夹*** 中有内容 或者 **构造方法中的路径不存在**，不会删除，返回 false。
        + **注意：**
            + delete 方法是直接在硬盘上删除文件 / 文件夹，不走回收站，删除需谨慎。


## 5. 目录的遍历

1. String[ ] list() ：返回一个String数组，表示该 File目录中的所有子文件或目录。遍历构造方法中给出的目录，会获取目录中所有文件或文件夹的名称，把获取到的多个名称存储到一个 String类型的数组中。 
2. File[ ] listFiles() ：返回一个File数组，表示该 File目录中的所有子文件或目录。遍历构造方法中给出的目录，会获取目录中所有文件或文件夹，把文件或文件夹封装为 File对象，多个 File对象存储到 File 数组中。 

> 注意事项：
> 1. list 方法和 listFiles 方法遍历的是构造方法中给出的目录
> 2. 如果构造方法中给出的目录的路径不存在，会抛出空指针异常。
> 3. 如果构造方法中给出的路径不是一个目录，也会抛出空指针异常。


## 6. 递归

### 概述

+ 递归：指在方法内调用自己的这种现象。
+ 递归的分类：
    + 递归分为两种：直接递归，间接递归。
    + 直接递归称为方法自身调用自己。
    + 间接递归可以A方法调用B方法，B方法调用C方法，C方法调用A方法。
+ 注意事项：
    + 递归一定要有**条件限定**，保证递归能够停下来，否则会发生**栈内存溢出**。
    + 在递归中虽然有限定条件，但是**递归次数不能太多**，否则会发生**栈内存溢出**。
    + **构造方法，禁止递归（编译是就报错）**。
+ 递归的使用前提：当调用方法的时候，方法的主体不变，每次调用方法的参数的不同，可以使用递归。


### 练习一：递归累加求和

```java
/*
练习：
    使用递归计算 1~n 之间的和
 */
public class Demo01DiGui {
    public static void main(String[] args) {
        int sum = accSum(100);
        System.out.println(sum);
    }

    /*
    定义一个方法，使用递归计算 1~n 之间的和
    1+2+3+...+n
    n+(n-1)+(n-2)+...+1
    已知：
        最大值：n
        最小值：1
    使用递归必须明确：
        1. 递归的结束条件：获取到1的时候结束
        2. 递归的目的：获取下一个被加的数字（n-1）
     */
    public static int accSum(int n) {
        // 获取到1的时候结束
        if (n == 1) {
            return 1;
        }
        // 获取下一个被加的数字
        return n + accSum(n-1);
    }

}
```

### 练习二：递归求阶乘

```java
public class Demo02DiGui {
    public static void main(String[] args) {
        int num = jc(10);
        System.out.println(num);
    }
    public static int jc(int n){
        if (n == 1){
            return 1;
        }
        return n * jc(n-1);
    }
}

```

### 练习三：递归打印多级目录

```java
import java.io.File;

/*
练习：
    递归打印多级目录
 */
public class Demo03DiGui {
    public static void main(String[] args) {
        File file = new File("D:\\messages");
        getAllFile(file);
    }

    /*
    定义一个方法，参数传递File类型的目录
    方法中对目录进行遍历
     */
    public static void getAllFile(File dir) {
        // 打印被遍历的目录名称
        System.out.println(dir);
        File[] files = dir.listFiles();
        for (File f : files) {
            // 对遍历得到的File对象f进行判断，判断是否为文件夹
            if (f.isDirectory()) {
                // f是一个文件夹，则继续遍历这个文件夹
                // 我们发现getAllFile方法就是传递文件夹，遍历文件夹的方法
                // 所以直接调用getAllFile方法即可：递归（自己调用自己）
                getAllFile(f);
            } else {
                // f是文件，直接打印即可
                System.out.println(f);
            }
        }
    }
}
```

## 7. 综合案例

### 文件搜索

```java
import java.io.File;

/*
练习：
    文件搜索
 */
public class Demo03DiGui {
    public static void main(String[] args) {
        File file = new File("D:\\messages");
        getAllFile(file);
    }

    /*
    定义一个方法，参数传递File类型的目录
    方法中对目录进行遍历
     */
    public static void getAllFile(File dir) {
        // 打印被遍历的目录名称
        //System.out.println(dir);

        File[] files = dir.listFiles();
        for (File f : files) {
            // 对遍历得到的File对象f进行判断，判断是否为文件夹
            if (f.isDirectory()) {
                // f是一个文件夹，则继续遍历这个文件夹
                // 我们发现getAllFile方法就是传递文件夹，遍历文件夹的方法
                // 所以直接调用getAllFile方法即可：递归（自己调用自己）
                getAllFile(f);
            } else {
                /*
                // 把File对象装换成String
                String s = f.toString();
                // 把结尾全转换成小写
                s = s.toLowerCase();
                // 调用String类中的endswith方法，判断是以什么结尾
                boolean b = s.endsWith(".dll");
                if (b == true) {
                    System.out.println(f);
                }
                */
                
                // 链式编程
                if (f.getName().toLowerCase().endsWith(".dll")) {
                    System.out.println(f);
                }
            }
        }
    }
}
```


### fileFilter的原理和使用（文件过滤器）

在File类中有两个和**listFiles**重载的方法，方法的参数传递的就是**过滤器**。

+ File[] listFiles(FileFilter filter) 
    + java.io.FileFilter接口：用于抽象路径名（File对象）的过滤器。
    + 作用：用来过滤文件（File对象）
    + 抽象方法：用来过滤文件的方法
        + boolean accept(File pathname)：测试指定抽象路径名是否应该在某个路径名列表中。
            + 参数：File pathname：使用listFiles方法遍历目录，得到的每一个File对象。

+ File[] listFiles(FilenameFilter filter) 
    + java.io.FilenameFilter接口：实现此接口的实例过滤器文件名。
    + 作用：用来过滤文件名称。
    + 抽象方法：用来过滤文件的方法。
        + boolean accept(File dir, String name) ：测试指定文件是否应该包含在某一文件列表中。 
            + File dir：构造方法中传递的被遍历的目录。
            + String name：使用listFiles方法遍历目录，获取的每一个文件 / 文件夹的名称。

> 注意：
> 两个过滤器接口都是没有实现类的，需要我们自己写实现类，重写过滤的方法 accept ，在方法中自己定义过滤的规则。


```java
import java.io.File;
/*
必须明确的两件事：
1. 过滤器中的accept方法是谁调用的？
2. accept方法的参数pathname是什么？
 */

public class DemoFilter {
    public static void main(String[] args) {
        File file = new File("D:\\ideaproject\\StudentManager");
        getAllFile(file);
    }
    /*
    定义一个方法，用来递归文件夹，查找所需要的文件
     */
    public static void getAllFile(File f) {
        File[] files = f.listFiles(new FileFilterImpl());// 传递过滤器对象
  /*
listFiles方法一共做了三件事：
    1. listFiles方法会对构造方法中传递的目录进行遍历,获取目录中的每一个文件夹/文件 --> 封装为File对象
    2. listFiles方法会调用参数传递的过滤器中的方法accept
    3. listFiles方法会把遍历得到的每一个File对象，传递给accept方法的参数pathname

accept方法的返回值是一个布尔值
    true：就会把传递的File对象保存到File数组中
    false：就不会把传递的File对象保存到File数组中
  */
        for (File file : files) {
            // 如果是文件夹，则继续递归
            if (file.isDirectory()) {
                getAllFile(file);
            } else { // 否则输出
                System.out.println(file);
            }
        }
    }
}
// =============

/*
创建过滤器FileFilter的实现类，重写accept方法，定义过滤规则
 */
import java.io.File;
import java.io.FileFilter;

public class FileFilterImpl implements FileFilter {
    @Override
    public boolean accept(File pathname) {
        /*
        过滤的规则：
        在accpet方法中，判断File对象是否是以.java结尾的
        是就返回true
        不是返回false
         */
        // 如果pathname是一个文件夹，返回true，继续遍历这个文件夹
        if (pathname.isDirectory()) {
            return true;
        }
        // 判断文件是否是以.java结尾的
        return pathname.getName().toLowerCase().endsWith(".java");
    }
}
```

### 使用lambda表达式优化

```java
import java.io.File;
import java.io.FileFilter;
import java.io.FilenameFilter;

public class Demo02Filter {
    public static void main(String[] args) {
        // 创建一个File对象
        File file = new File("D:\\aaa");
        getAllFile(file);
    }

    /*
    定义一个方法，用来递归查找指定的文件夹
     */
    public static void getAllFile(File f) {
        // 使用匿名内部类
        // 使用listFiles方法遍历目录
        /*File[] files = f.listFiles(new FileFilter() {
            // 如果是pathname文件夹或者是以.java结尾的文件，则返回true
            @Override
            public boolean accept(File pathname) {
                return pathname.isDirectory() || pathname.getName().toLowerCase().endsWith(".java");
            }
        });*/

        // 使用lambda表达式优化
        // File[] files = f.listFiles(pathname -> pathname.isDirectory() || pathname.getName().toLowerCase().endsWith("java"));

        // 传递FilenameFilter
        /*File[] files = f.listFiles(new FilenameFilter() {
            @Override
            public boolean accept(File dir, String name) {
            // 将目录和文件名称组成一个文件对象
               return new File(dir,name).isDirectory()|| name.toLowerCase().endsWith(".java");
            }
        });*/

        // 使用lambda优化
        File[] files = f.listFiles((dir, name) -> new File(dir, name).isDirectory() || name.toLowerCase().endsWith(".java"));
        for (File file : files) {
            if (file.isDirectory()) {
                getAllFile(file);
            } else {
                System.out.println(file);
            }
        }
    }
}
```

