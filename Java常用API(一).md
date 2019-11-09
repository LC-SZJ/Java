[TOC]
# Java 常用API（一）

查看API文档的三个步骤：
1. 看位于哪个包下。
2. 查看构造方法。
3. 查看普通方法。

## 1. Scanner类

+ Scanner类的功能：可以实现键盘输入数据，到程序中。
### 引用类型的一般使用步骤
1. 导包
    import 包路径.类名称;
    如果需要使用目标类，和当前类位于同一包下，则可以省略导包语句
    只有Java.lang包下的内容不需要导包，其他的包都需要import语句
2. 创建
    类名称 对象名 = new 类名称();
3. 使用
    对象名.成员方法名();
### Scanner的使用步骤
```java
//导包
import java.util.Scanner;

public class DemoScanner {
    public static void main(String[] args) {
    //创建
    //备注：System.in 从键盘上获取输入
        Scanner sc = new Scanner(System.in);
    //使用
        int num = sc.nextInt();
        System.out.println("输入int整数为：" + num);
        String str = sc.next();
        System.out.println("输入的字符串为：" + str);
    }

}
```

### 例题

```java
/*题目：
从键盘输入三个整数，输出最大值*/
import java.util.Scanner;

public class DemoScanner {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        System.out.println("请输入第一个整数：");
        int a = sc.nextInt();
        System.out.println("请输入第二个整数：");
        int b = sc.nextInt();
        System.out.println("请输入第三个整数：");
        int c = sc.nextInt();
        
        int temp = a > b ? a : b;
        int max = temp > c ? temp : c;
        System.out.println("最大值为：" + max);
    }

}
```

## 2. 匿名对象

### 概述

+ 创建对象的标准格式：类名称 对象名 = new 类名称();
+ 匿名对象就是只有右边的对象，没有左边的对象名和赋值运算符。new 类名称();
+ 注意事项：匿名对象只能使用唯一的一次，下次再使用不得不再创建一个新的对象。
+ 使用建议：如果确定有一个对象只需要使用唯一的一次就可以使用匿名对象。
+ 使用方式

```java
import java.util.Scanner;
public class DemoAnonymous {
    public static void main(String[] args) {
        //普通使用方式
//        Scanner sc = new Scanner(System.in);
//        int num = sc.nextInt();
        //匿名对象使用方式
//        int num = new Scanner(System.in).nextInt();
        
    }
}
```

### 匿名对象作为方法的参数

```java
import java.util.Scanner;
public class DemoAnonymous {
    public static void main(String[] args) {
      //使用一般写法传入参数
//        Scanner sc = new Scanner(System.in);
//        methodParam(sc);
        //使用匿名对象来进行传参
          methodParam(new Scanner(System.in));
    }
    public static void methodParam(Scanner sc){
        int num = sc.nextInt();
        System.out.println("输入的是：" + num);
    }
}
```

### 匿名对象作为方法的返回值

```java
import java.util.Scanner;
public class DemoAnonymous {
    public static void main(String[] args) {
        Scanner sc = methodReturn();
        int num = sc.nextInt();
        System.out.println("输入的值为：" + num);

    }
    public static Scanner methodReturn(){
//        Scanner sc = new Scanner(System.in);
//        return sc;
          return new Scanner(System.in);
    }
}
```

## 3. Random类
+ Random类的功能：用来生成随机数字

### 使用步骤
1. 导包
    import java.util.Random;
2. 创建
    Random r = new Random();(小括号留空即可)
3. 使用
    获取一个随机的int数字(范围是int所有范围，有正负两种)，int num = r.nextInt();
    获取一个随机的int数字(参数代表了范围，左闭右开区间), int num = r.nexInt(3);
    实际上代表的含义是[0, 3)，也就是0~2
```java
import java.util.Random;

public class DemoRandom {
    public static void main(String[] args) {
        Random r = new Random();
        int num = r.nextInt();
        System.out.println("随机数字是：" + num);
    }
}
```

```java
import java.util.Random;

public class DemoRandom {
    public static void main(String[] args) {
        Random r = new Random();
        for (int i = 0; i < 100; i++) {
            int num = r.nextInt(10);
            System.out.println(num);
        }
    }
}
```

### 生成[1, n]之间的随机数
```java
/*
题目：
根据int变量n的值，来获取随机数字，范围是[1,n]，可以取到1，也可以取到n
 */
import java.util.Random;

public class DemoRandom {
    public static void main(String[] args) {
        Random r = new Random();
        int n = 5;
        for (int i = 0; i < 100; i++) {
     //本来的范围是[0.n),只需整体+1即可变成[1,n+1),即[1,n]
            int result = r.nextInt(n) + 1;
            System.out.println(result);
        }
    }
}
```

### 猜数字小游戏

```java
/*
题目：
用代码模拟猜数字的小游戏。
思路：
1. 首先需要产生一个随机数字，并且一旦产生就不再变化，用Random的nextInt()方法
2. 需要键盘输入，所以用到了Scanner类
3. 获取键盘输入的数字，用到了Scanner类的nextInt()方法
4. 将已经得到的数字，判断一下（if）
    如果太大了，提示太大，并且重试
    如果太小了，提示太小，并且重试
    如果猜中了，游戏结束！
5. 重试就是再来一次，如果循环次数不确定，用while(true)
 */
import java.util.Random;
import java.util.Scanner;
public class DemoRandomGame {
    public static void main(String[] args) {
        Random r = new Random();
        Scanner sc  = new Scanner(System.in);
        int randomNum = r.nextInt(100) + 1 ;// [1,100]
        while(true){
            System.out.println("请输入你猜测的数字：");
            int guessNum = sc.nextInt();
            if (guessNum > randomNum){
                System.out.println("太大了，请重试。");
            }else if (guessNum < randomNum){
                System.out.println("太小了，请重试。");
            }else{
                System.out.println("恭喜你，猜中了！");
                break;//如果猜中了，循环结束
            }
        }
        System.out.println("游戏结束。");

    }
}
```

## 4. ArrayList`<E>`类

+ 数组的长度不可以发生改变，但是ArrayList集合的长度是可以随意变化的。
+ 对于ArrayList来说，有一个`<E>`代表***泛型***。
+ 泛型，也就是装在集合当中的所有元素，全都是统一的什么类型。
+ 注意事项：泛型只能是引用类型，不能是基本类型。
+ 注意事项：对于ArrayList集合来说，直接打印得到的不是地址值，而是内容，如果内容是空，得到的是空的中括号, [ ]。
+ 示例

```java
import java.util.ArrayList;
public class DemoArrayList {
    public static void main(String[] args) {
        //创建了一个ArrayList集合，集合的名称是list，里面装的全都是String字符串类型的数据
        //备注：从JDK1.7+开始，右侧的尖括号内部可以不写内容，但是<>本身还是要写的
        ArrayList<String> list = new ArrayList<>();
        System.out.println(list);  //  []
        //向集合当中添加一些数据，需要用到add方法
        list.add("迪丽热巴");
        System.out.println(list);  //  [迪丽热巴];
        list.add("古力娜扎");
        list.add("赵丽颖");
        System.out.println(list);  //  [迪丽热巴, 古力娜扎, 赵丽颖]
    }
}
```

###  ArrayList当中常用的方法
+ public boolean add(E e)：向集合当中添加元素，参数的类型和泛型一致，返回值代表添加是否成功。
备注：对于ArrayList集合来说，add添加动作一定是成功的，所以返回值可用不可用，但是对于其他集合来说，add添加动作不一定成功。

+ public E get(int index)：从集合当中获取元素，参数是索引编号，返回值就是对应位置的元素。
+ public E remove(int index)：从集合当中删除元素，参数是索引编号，返回值就是被删除的元素。
+ public int size()：获取集合的尺寸长度，返回值是集和当中包含元素的个数。
+ 示例

```java
import java.util.ArrayList;
public class DemoArrayListMethod {
    public static void main(String[] args) {
        ArrayList<String> list = new ArrayList<>();
        System.out.println(list);

        // 向集合当中添加元素：add
       boolean success =  list.add("迪丽热巴");
        System.out.println(list); // 迪丽热巴
        System.out.println("添加动作是否成功：" + success); // true

        list.add("李小璐");
        list.add("赵又廷");
        list.add("贾乃亮");
        System.out.println(list); //[迪丽热巴, 李小璐, 赵又廷, 贾乃亮]

        //从集合当中获取元素：get，索引从0开始
        String name = list.get(1);
        System.out.println("第一号索引位置：" + name);

        //从集合当中删除元素：remove，索引从0开始
        String whoRemoved = list.remove(1);
        System.out.println("被删除的人是：" + whoRemoved); // 李小璐
        System.out.println(list);

        //获取集合的长度尺寸，也就是其中元素的个数
        System.out.println("集合的长度是：" + list.size());

    }
}  
```

+ 遍历集合

```java
import java.util.ArrayList;
public class DemoArraylistEach {
    public static void main(String[] args) {
        ArrayList<String> list = new ArrayList<>();
        list.add("迪丽热巴");
        list.add("古力娜扎");
        list.add("马尔扎布");

        // 遍历集合
        for (int i = 0; i < list.size(); i++) {
            System.out.println(list.get(i));
        }
    }
}

```

### ArrayList存储基本数据类型
+ 如果希望向集合ArrayList中存储基本数据类型，必须使用基本类型对应的“包装类”。
+ ArrayList中不能存储基本数据类型，集合中实际上存储的是地址值
+ 备注：从JDK 1.5+ 开始，支持自动装箱，自动拆箱。
    自动装箱：基本类型 --> 包装类型
    自动拆箱：包装类型 --> 基本类型

|基本数据类型	|  包装类（引用类型，包装类都位于java.lang包下）	  |
| --- | --- |
|byte	|Byte	|
|short	|Short	|
|int	|Integer       【特殊】	|
|long	|Long	|
|float	|Float	|
|double	|Double	|
|char	|Character   【特殊】	|
|boolean	|Boolean	|

+ 包装类使用示例

```java
import java.util.ArrayList;
public class DemoArrayLlistBasic {
    public static void main(String[] args) {
        ArrayList<Integer> listC = new ArrayList<>();
        listC.add(100);
        listC.add(200);
        System.out.println(listC); // [100, 200]
        int num = listC.get(1);
        System.out.println("索引为1的元素是：" + num); // 200
    }
}

```

### 题目一

```java
import java.util.ArrayList;
import java.util.Random;
/*
题目：
产生6个1~33的随机数，添加到集合当中，并遍历集合
 */
public class DemoArrayListRandom {
    public static void main(String[] args) {
        ArrayList<Integer> list = new ArrayList<>();
        Random r = new Random();
        for (int i = 0; i < 6; i++) {
            int num = r.nextInt(33) + 1;
            list.add(num);
        }
        for (int i = 0; i < list.size(); i++) {
            System.out.println(list.get(i));
        }
    }
}
```

### 题目二

```java
/*
在ArrayList中存储自定义的对象
 */
import java.util.ArrayList;
public class DemoArrayListStudent {
    public static void main(String[] args) {
        ArrayList<Student> list = new ArrayList<>();

        Student one = new Student("洪七公", 20);
        Student two = new Student("欧阳锋", 21);
        Student three = new Student("黄药师", 22);
        Student four = new Student("段智兴", 23);

        list.add(one);
        list.add(two);
        list.add(three);
        list.add(four);

        // 遍历集合
        for (int i = 0; i < list.size(); i++) {
            Student stu = list.get(i);
            System.out.println("姓名：" + stu.getName() + "，年龄：" + stu.getAge());

        }
    }
}
```

### 题目三

```java
/*
按指定格式遍历集合字符串
 */
import java.util.ArrayList;
public class DemoArrayListPrint {
    public static void main(String[] args) {
        ArrayList<String> list = new ArrayList<>();

        list.add("张三丰");
        list.add("张无忌");
        list.add("张翠山");
        list.add("宋远桥");

        System.out.println(list); // [张三丰, 张无忌, 张翠山, 宋远桥]
        printArrayList(list);	// {张三丰@张无忌@张翠山@宋远桥}
    }
// 打印指定格式的集合，集合可以当作方法的参数
    public static void printArrayList(ArrayList<String> list){
        System.out.print("{");
        for (int i = 0; i < list.size(); i++) {
            String name = list.get(i);
            if (list.size() -1 == i){
                System.out.println(name + "}");
            }else{
                System.out.print(name + "@");
            }
        }
    }
}
```

### 题目四

```java
import java.util.ArrayList;
import java.util.Random;
/*
题目：
创建一个装20个随机数的大集合，将其中的偶数保存在小集合中。
要求自定义方法实现筛选
 */
public class DemoArrayListReturn {
    public static void main(String[] args) {
        ArrayList<Integer> bigList = new ArrayList<>();
        Random r = new Random();
        for (int i = 0; i < 20; i++) {
            int num = r.nextInt(100) + 1;
            bigList.add(num);
        }

        ArrayList<Integer> smallList = getSmallList(bigList);
        System.out.println("偶数共" + smallList.size() + "个");
        for (int i = 0; i < smallList.size(); i++) {
            System.out.println(smallList.get(i));
        }
    }
    // 自定义方法，参数是大集合，返回值是满足条件的小集合
    public static ArrayList<Integer> getSmallList(ArrayList<Integer> bigList){
        ArrayList<Integer> smallList = new ArrayList<>();

        for (int i = 0; i < bigList.size(); i++) {
            int num = bigList.get(i);
            if (num % 2 == 0) {
               smallList.add(num);
            }
        }
        return smallList;
    }
}
```

## 5. String类
### 概述
+ java.lang.String代表字符串
+ API当中说，Java 程序中的所有字符串字面值（如 "abc" ）都作为此类的实例实现。其实就是说，程序当中所有的双引号字符串，都是String类的对象。（就算没有new，也照样是。）

### 字符串的特点
1. 字符串的内容永不可变。【重点】
2. 正因为字符串不可改变，所以字符串是可以共享使用的。
3. 字符串效果上相当于char[ ]字符数组，但是底层原理是byte[ ]字节数组。

### 创建字符串的常见 3+1 种方式
三种构造方法 ：
1. public String()：创建一个空白字符串，不含有任何内容。
2. public String(char [ ] array)：根据字符数组的内容，来创建对应的字符串。
3. public String(byte [ ] array)：根据字节数组的内容，来创建对应的字符串。

一种直接创建：
String str = "Hello" // 右边直接用双引号
注意：直接写上双引号，就是字符串对象。

### 示例
```java
public class DemoString {
    public static void main(String[] args) {
        // 空参构造
        String str1 = new String();
        System.out.println("第一个字符串：" + str1);

        //根据字符数组创建字符串
        char[] charArray = { 'A', 'B', 'C' };
        String str2 = new String(charArray);
        System.out.println("第二个字符串：" + str2);

        //根据字节数组创建字符串
        byte[] byteArray = { 97, 98, 99 };
        String str3 = new String(byteArray);
        System.out.println("第三个字符串：" + str3);
    }
}
```

### 字符串常量池

+ 字符串常量池：程序当中直接写上的双引号字符串，就在字符串常量池中。
+ 对于基本类型来说，==进行的是数值的比较。
+ 对于引用类型来说，==进行的是【地址值】的比较。
+ 双引号直接写的字符串在常量池当中，new的不在池当中。
+ 字符串常量池内存图
![字符串常量池内存图](https://ae01.alicdn.com/kf/Ha9f5aa30d166458f9b6fc2cd0d70523eO.jpg)

### 字符串的比较相关方法
> ==对引用类型来说，进行的是地址值的比较，如果确实需要进行对字符串内容的比较，可以使用两个方法
+ public boolean equals(Object obj)：参数可以是任何对象，只有参数是一个字符串并且内容相同的才给true，否则返回false.

+ 注意事项：
    1. 任何对象都能用Object接收。
    2. equals方法具有对称性，也就是a.equals(b) 和 b.equals(a)效果一样。
    3. 如果比较双方一个常量，一个变量，推荐把常量字符串写在前面。
        推荐："abc".equals(str)	不推荐：str.equals("abc")
        因为变量字符串一旦是null，程序就会出现NullPointerException异常
    
+ public boolean equalsIgnoreCase(String str)：忽略大小写，进行内容比较

### String当中与获取相关的常用方法

+ public int length()：获取字符串当中的字符的字符个数，拿到字符串的长度。
+ public String concat(String str)：将当前字符串和参数字符串拼接成为返回值，新的字符串。
+ public char charAt(int index)：获取指定索引位置的单个字符。
+ public int indexOf(String str)：查找参数字符串在本字符串中首次出现索引位置，如果没有返回-1值。

```java
public class DemoStringGet {
    public static void main(String[] args) {
        // 获取字符串的长度
        int length = "jkdjfahgjgakjouerkjflksda".length();
        System.out.println("字符串的长度是：" + length);

        // 拼接字符串
        String str1 = "Hello";
        String str2 = " World!";
        String str3 = str1.concat(str2);
        System.out.println(str3);

        // 查找单个字符
        String str4 = "Hello World!";
        char ch = str4.charAt(2);
        System.out.println("2号索引位置的字符是：" + ch);

        // 查找参数字符串在本字符串中首次出现的索引位置
        String str5= "Hello World!";
        int index = str5.indexOf("llo");
        System.out.println("索引位置是：" + index);


    }
}
```


### 字符串的截取方法

+ public String substring(int index)：截取从参数位置一直到字符串末尾，返回新的字符串。
+ public String substring(int begin, int end)：截取从begin开始，到end结束，中间的字符串。
    备注：[begin, end)，包含左边，不包含右边。
```java
public class DemoSubstring {
    public static void main(String[] args) {
        String str1 = "HelloWorld";
        String str2 = str1.substring(5);
        System.out.println(str2); // World

        String str3 = str1.substring(4,  7); 
        System.out.println(str3); //  oWo
        System.out.println("=============");
        
 // 字符串的内容没有发生改变，只是strA中保存的地址值发生了改变
        String strA = "Hello";
        System.out.println(strA); // Hello
        strA = "Java";
        System.out.println(strA); // Java
    }
}
```

### String当中与转换相关的常用方法

+ public char[ ] toCharArray()：将当前字符串拆分成字符数组作为返回值。
+ public byte[ ] getBytes()：获得当前字符串底层的字节数组。
+ public String replace(CharSequence oldString, CharSequence newString)：
将所有出现的老字符串替换成为新的字符串，返回替换之后的结果新的字符串。
    备注：CharSequence意思就是说可以接受字符串类型。

```java
public class DemolStringConvert {
    public static void main(String[] args) {
        // 转换成为字符数组
        char[] chars = "Hello".toCharArray();
        System.out.println(chars[0]); // H
        System.out.println(chars.length); // 5
        System.out.println("===========");

        // 装换成为字节数组
        byte[] bytes = "abc".getBytes();
        for (int i = 0; i < bytes.length; i++) {
            System.out.println(bytes[i]); // 97 98 99
        }
        // 字符串替换
        System.out.println("===========");
        String str1 = "How do you do ?";
        String str2 = str1.replace("o", "*");
        System.out.println(str1); // How do you do ?
        System.out.println(str2); // H*w d* y*u d* ?
    }
}
```

### 分割字符串的方法
+ public String [ ] split(String regex)：按照参数的规则，将字符串切分成若干部分。
> 注意事项：split方法的参数其实是一个"正则表达式"。
> 如果按照英文句点 "."进去切分，必须写 "\\."(两个反斜杠)。

```java
public class DemoStringSplit {
    public static void main(String[] args) {
        String str1 = "aaa,bbb,ccc";
        String[] array1 = str1.split(",");
        for (int i = 0; i < array1.length; i++) {
            System.out.println(array1[i]);
        }
        System.out.println("=============");

        String str2 = "XXX.YYY.ZZZ";
        String[] array2 = str2.split("\\.");
        System.out.println(array2.length);
        for (int i = 0; i < array2.length; i++) {
            System.out.println(array2[i]);
        }

    }
}
```

### 题目一
```java
/*
题目：
定义一个方法，把数组{1，2，3}按照指定格式拼接成一个字符串。
格式参照如下：[word1#word2#word3#]
分析：
1. 首先准备一个int数组，内容是1，2，3
2. 定义一个方法，将数组变成字符串
三要素：
返回值类型：String
方法名称：formatArrayToString
参数列表：int[]
3. 格式;[word1#word2#word3]
用到：for循环、字符串拼接、每个数组元素之前有一个word字样，分隔使用#，区分一下是不是最后一个
4. 调用方法，得到返回值，并打印结果字符串

 */
public class DemoSringPractise {
    public static void main(String[] args) {
        int[] array= {1, 2, 3};

        String result = formatArrayToString(array);
        System.out.println(result);

    }
    public static String formatArrayToString(int [] array) {
        String str = "[";
        for (int i = 0; i < array.length; i++) {
            if ( i == array.length -1 ){
                str += "word" + array[i] + "]";
            }else{
               str += "word" + array[i] + "#" ;
            }
        }
        return str;
    }
}
```

### 题目二
```java
import java.util.Scanner;
/*
题目：
键盘输入一个字符串，并且统计其中各种字符出现的次数
种类有：大写字母，小写字母，数字，其他
分析：
1. 既然用到了键盘输入，那肯定是Scanner
2. 键盘输入的是字符串，那么，String str = sc.next();
3. 定义四个变量，分别统计其中四种字符出现的次数
4. 需要对字符串一个字，一个字的检查，String --> char []
 */
public class DemoStringCount {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.println("请输入一个字符串：");
        String input = sc.next();

        int countUpper = 0;
        int countLower = 0;
        int countNumber = 0;
        int countOther = 0;

        char[] chars = input.toCharArray();
        for (int i = 0; i < chars.length; i++) {
            char ch = chars[i];
            if (ch >= 'a' && ch <= 'z'){
                countLower++;
            }else if (ch >= 'A' && ch <= 'Z'){
                countUpper++;
            }else if (ch >= '0' && ch <= '9'){
                countNumber++;
            }else{
                countOther++;
            }
        }
        System.out.println("大写字母有：" + countUpper);
        System.out.println("小写字母有：" + countLower);
        System.out.println("数字有：" + countNumber);
        System.out.println("其他字符有：" + countOther);


    }
}
```

## 6. 静态static

### 概述

+ 一旦使用static修饰成员方法，那么这就成为了静态方法。静态方法不属于对象，而是属于类的。
+ 如果没有static关键字，那么必须首先创建对象，然后通过对象才能使用它。
+ 如果有了static关键字，那么不需要创建对象，直接通过类名称就能使用它。
+ 无论成员变量，还是成员方法，如果有了static，都推荐使用类名称进行调用。
    静态变量：类名称.静态变量
    静态方法：类名称.静态方法
    
+ 对于静态方法来说，也可以使用对象名进行调用，也可以直接通过类名称进行调用，推荐使用后者进行调用，使用前者调用后，被编译之后会被javac翻译成为“ 类名称.静态方法名”
+ 对于本类当中的静态方法，可以省略类名称


### 注意事项：

1. 静态不能直接访问非静态
    原因：因为在内存当中是【先】有的静态内容，【后】有的非静态内容。
    “先人不知道后人，但是后人知道先人”
2. 静态方法当中不能使用this关键字
    原因：this代表当前对象，通过谁调用的方法，谁就是当前对象。
    
### 静态static的内存图

![静态static的内存图](https://ae01.alicdn.com/kf/H17a117f2f596495ebd988e7d02d7cd3aw.jpg)

> 就算你是用对象名访问的静态变量，编译之后被javac翻译成“类名称.静态变量”。

### 静态代码块

+ 格式：

```java
    public class 类名称{
       static{
       // 静态代码块的内容
       }
    }
```

+ 特点：
    1. 当第一次用到本类的时，静态代码块执行唯一的一次。
    2. 静态内容总是优于非静态内容，所以静态代码块比构造方法先执行。
+ 静态代码块的典型用途
    用来一次性的对静态成员变量进行赋值。


## 7. Arrays工具类

+ java.util.Arrays是一个与数组相关的工具类，里面提供了大量的静态方法，用来实现数组常见的操作。
+ public static String toString(数组)：将参数数组变成字符串（按照默认格式：[元素1，元素2，元素3，...]
+ public static void sort(数组)：按照默认升序（从小到大）对数组的元素进行排序。
> 备注：
> 1. 如果是数值，sort默认按照升序从小到大。
> 2. 如果是字符串，sort默认按照字母升序。
> 3. 如果是自定义的类型，那么这个自定义的类需要有Comparable或者Comparator接口的支持（今后学习）。


```java
import java.util.Arrays;
public class DemoArrays {
    public static void main(String[] args) {
        int[] intArray = {3, 2, 1};
        Arrays.sort(intArray);
        // 将int[]数组按照默认格式变成字符串
        String intStr = Arrays.toString(intArray);
        System.out.println(intStr); // [1, 2, 3]

        String[] array2 = {"aaa", "ccc", "bbb"};
        Arrays.sort(array2);
        System.out.println(Arrays.toString(array2));
        // [aaa, bbb, ccc]
    }
}
```

```java
import java.util.Arrays;
/*
题目：
请使用Arrays相关的API，将一个随机字符串中所有的字符升序排列，并倒序打印。
 */
public class DemoArraysPractise {
    public static void main(String[] args) {
        String str = "hkjlfdsklour3243dkfsaaa";
        // 首先将字符串装换为字符数组
        char[] chars = str.toCharArray();
        // 对数组进行升序排列
        Arrays.sort(chars);
        // 倒序打印
        for (int i = chars.length - 1; i >= 0; i--) {
            System.out.println(chars[i]);
        }
    }
}
```

## 8. Math类

+ java.util.Math类是数学相关的工具类，里面提供了大量的静态方法，完成与数学运算相关的操作。
+ public static double abs(double num)：获取绝对值，有多种重载形式。
+ public static double cell(double num)：向上取整。
+ public static double floor(double num)：向下取整。
+ public static double round(double num)：四舍五入。
+ Math.PI：代表近似的圆周率常量（double)

```java
/*
题目：
计算在-10.8到5.9之间，绝对值大于6或者小于2.1的整数有多少个？
分析：
将-10.8转换为-10，两种办法：
1. Math.cell()方法，向上（向正方向）取整
2. 强转成为int，自动舍弃所有小数位
备注：如果使用Math.cell()，-10.8会变成-10.0，注意：double也是可以++的
 */
public class DemoMathPractise {
    public static void main(String[] args) {
        int count = 0; // 统计个数
        double max = 5.9;
        double min = -10.8;

/*
        for (double i = Math.ceil(min); i < max; i++)
               double abs = Math.abs(i);
*/
        for (int i = (int)min; i < max; i++){
            int abs = Math.abs(i);
            if (abs > 6 || abs < 2.1){
                count++;
                System.out.println(i);
            }
        }
        System.out.println("总共有：" + count);

    }
}
```