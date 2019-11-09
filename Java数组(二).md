[TOC]

# Java 数组（二）

## 1. 概述
+ 数组的概念：是一种容器，可以同时存放多个数据值。


## 2. 数组的特点：
1. 数组是一种引用数据类型。
2. 数组当中的多个数据，类型必须统一。
3. ***数组的长度在程序运行期间不可改变***。

## 3. 数组的初始化

定义：在内存当中创建一个数组，并且向其中赋予一些默认值。

+ 两种常见的初始化方式：
    1. 动态初始化（指定长度）
    2. 静态初始化（指定内容）
+ 动态初始化数组的格式：
数据类型[ ] 数组名称 = new 数据类型 [ 数组长度 ];

+ 静态初始化数组

    + 基本（标准）格式：
        数据类型[ ] 数组名称 = new 数据类型 [ ] { 元素1, 元素2, ... };

    + 省略格式：
        数据类型[ ] 数组名称 = { 元素1, 元素2, ... };

> 注意事项：
> 1. 虽然静态初始化没有直接告诉长度，但是根据大括号里面的元素具体内容，也可以自动推算出来长度。
> 2. 静态初始化的标准格式可以拆分成两个步骤。
> 3. 动态初始化也可以拆分成两个步骤。
> 4. 静态初始化一旦使用省略格式，就不能拆分成为两个步骤了。
> 

使用建议：
如果不确定数组当中的具体内容，用动态初始化；否则，已经确定了具体内容，用静态初始化。

```java
public class DemoArray {
    public static void main(String[] args) {
        // 动态初始化分成两个步骤
        int[] arrayA;
        arrayA = new int[10];

        // 静态初始化标准格式分成两个步骤
        int[] arrayB;
        arrayB = new int[] {5, 15, 25};

        // 静态初始化的省略格式不能拆分成两个步骤
        /*int[] arrayC;
        arrayC = {5, 10, 15};*/
    }
}
```

直接打印数组名称，得到的是数组对应的，***内存地址哈希值***

```java
System.out.println(arrayA); // [I@10f87f48
```

##  4. 一个数组的内存图

![一个数组的内存图](https://ae01.alicdn.com/kf/Hb0eaca0b382b4181a9eea24f3b0339b8l.jpg)


##  5. 两个数组的内存图

![两个数组的内存图](https://ae01.alicdn.com/kf/H55637815208d4b9bb9f3d5a8c321e2016.jpg)


## 6. 两个引用指向同一个数组的内存图

![两个引用指向同一个数组的内存图](https://ae01.alicdn.com/kf/H472189096be24cdcb488a0aea4170e17c.jpg)

## 7. 使用数组的常见问题

### 一、ArrayIndexOutOfBoundsException

数组的索引编号从0开始，一直到“数组长度 - 1” 为止。
如果访问数组元素的时候，索引编号不存在，那么将会发生***数组索引越界异常：ArrayIndexOutOfBoundsException***。

原因：索引编号写错了。
解决：修改成为正确的索引编号。


### 二、NullPointerException

数组必须进行new初始化，才能使用其中的元素。
如果只是赋值了一个null，没有进行new创建，那么将会发生***空指针异常：NullPointerException***。

原因：忘了new
解决：补上new


## 8. 获取数组的长度

格式：数组名称.length
返回一个int的数值


## 9. 数组反转
重点：对称位置、两个索引、三个变量、min < max

```java
import java.util.Arrays;

public class DemoArrayReverse {
    public static void main(String[] args) {
        int[] array = {5, 10, 20, 300, 10000};

        System.out.println(Arrays.toString(array));
        // 数组反转
        int len = array.length;
        for (int i = 0; i < len / 2; i++) {
            int left = i;
            int right = len - i - 1;
            int temp = array[left];
            array[left] = array[right];
            array[right] = temp;
        }
        System.out.println("============");

        System.out.println(Arrays.toString(array));

        System.out.println("=========");

        for (int min = 0, max = array.length - 1; min < max; min++, max--) {
            int temp = array[min];
            array[min] = array[max];
            array[max] = temp;
        }

        System.out.println(Arrays.toString(array));
    }
}
```

## 

