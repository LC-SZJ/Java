
[TOC]
# Java 数组（一）


## 1.一维数组的创建和使用

### 1.1 创建一维数组
+ 先声明，再用new运算符进行内存分配
```java
//第一种声明方式
type arrayname[];
//第二种声明方式 
type [] arrayname;
//内存分配
arrayname = new type[arraylength];
```

> 用new运算符为数组分配内存时，整型数组每个元素的初始值为0；浮点型数组每个元素的初始值为0.0；String型为null

+ 声明的同时为数组分配内存
```java
type arrayname[] = new type[arraylength];
```
### 1.2 初始化一维数组
```java
//第一种初始化方式
type arrayname[] = new type[]{elem1, elem2, elem3,...};
//第二种初始化方式
type arrayname[] = {elem1, elem2, elem3, ...};
```
### 1.3 使用一维数组
```java
//使用一维数组输出1~12月的天数
public class GetDay {
    public static void main(String [] args){
        int [] day = new int[]{31, 28, 31, 30, 31, 30, 31, 30, 31, 30, 31};
        //使用for循环
        for (int i = 0; i < day.length; i++){
            System.out.println((i+1)+"月有"+day[i]+"天");
        }
        //使用foreach语句
        int i = 0;
        for (int e: day){
            System.out.println((i+1)+"月有"+day[i]+"天");
            i++;
        }
    }
}
```

## 2. 二维数组的创建和使用

### 2.1 二维数组的创建
+ 先声明，再用new运算符进行内存分配
```java
//第一种声明方式
type arrayname[][];
//第二种声明方式
type [][] arrayname;
//(1)直接为每一维分配内存空间
arrayname = new type[row][line];
//(2)分别为每一维分配内存空间
arrayname = new type[row][];
arrayname[0] = new type[line1];
arrayname[1] = new type[line2];
……
```

+ 声明的同时为数组分配内存
```java
type arrayname[][] = new type[row][line];
```

### 2.2 二维数组的初始化
```java
type arrayname[][] = {{value1, ...}, {value2, ..}, ...};
type arrayname[][] = new type[][]{{value1, ...}, {value2, ..}, ...};
```

## 3. 数组基本操作的方法

### 3.1 遍历数组
e.g：
```java
/**
 * 遍历二维数组
 */
int c[][] = new int[][]{{1}, {2, 3}, {4, 5, 6}};
		//使用双重for循环
        for (int i = 0; i < c.length; i++){
            for (int j = 0; j < c[i].length; j++){
                System.out.print(c[i][j]+" ");
            }
            System.out.println();
        }
        System.out.println();
        //使用foreach语句
        for (int x[]: c){
            for (int e: x){
                System.out.print(e+" ");
            }
            System.out.println();
        }
    }
```

### 3.2 填充替换数组元素
数组中的元素定义完成后，可通过**Arrays**类的静态方法**fill()**来对数组中的元素进行替换。该方法通过各种**重载**形式可完成对任意类型的数组元素的替换。fill()方法有两种参数类型。

#### fill(int [] a, int value)
  + 作用：将指定的int值分配给int型数组的每个元素。
  + e.g：
```java
import java.util.Arrays;	//导入java.util.Arrays类
public class Swap {
    public static void main(String [] args){
        int arr[] = new int[]{1, 2, 3, 4, 5, 6};
        int j = 0;
        //原样输出
        for (int x : arr){
            System.out.println(j+":"+x);
            j++;
        }
        System.out.println();
        //使用fill(int []a, int value)进行替换
        Arrays.fill(arr, 6);
        //替换后输出
        for (int i = 0; i < arr.length; i++){
            System.out.println(i+":"+arr[i]);
        }
    }
}
```

#### fill(int [] a, int fromIndex, int toIndex, int value)
+ 作用：从数组a的**[formIndex, toIndex)**，是个左闭右开区间，填充数值value。

### 3.3 对数组进行排序

#### Arrays.sort(Object[ ] a)
+ 作用：通过Arrays类的静态方法**sort()**方法可以实现对数组的升序排序。
+ e.g:
```java
import java.util.Arrays;
public class Taxis {
    public static void main(String [] args){
        int[] arr = new int[]{1, 9, 8, 7, 5, 3,2};
        String [] s = new String[]{"1", "A", "a", "B"};
        //对int型数组排序
        Arrays.sort(arr);
        //对String型数组排序
        Arrays.sort(s);
        System.out.println();
        for (int e : arr){
            System.out.print(e+" ");
        }
        System.out.println();
        for (String e : s){
            System.out.print(e+" ");
        }
    }
}
```
> Java中的String类型数组的排序算法是根据字典编排顺序排序的，因此数字排在字母前面，大写字母排在小写字母前面。

### 3.4 复制数组

#### copyOf(arr, int newlength)
+ 作用：复制数组至指定长度
> arr：要进行复制的数组
> newlenght：int型常量，指复制后的新数组的长度。
> 如果新数组的长度大于数组arr的长度，则整型数组用0填充，char型数组用null来填充；
> 如果新数组的长度小于数组arr的长度，则会从数组arr的第一个元素开始截取至满足新数组长度为止；
+ e.g:
```java
import java.util.Arrays;
public class Copy {
    public static void main(String [] args){
        int arr[] = new int[]{1, 2, 3, 4, 5};
        int newarr1[] = Arrays.copyOf(arr, 6);
        for (int e : newarr1){
            System.out.print(e+" ");
        } 
    }
}
//结果：1,2,3,4,5,0
```

#### copyOfRange(arr, int fromIndex, int toIndex)
+ 作用：将指定数组的指定长度复制到一个新数组
> 从arr中复制的范围 **[ fromIndex,  toIndex )**
+ e.g:
```java
import java.util.Arrays;
public class Copy {
    public static void main(String [] args){
        int arr[] = new int[]{1, 2, 3, 4, 5};
        int arr3[] = Arrays.copyOfRange(arr, 0, 4);
        for (int i = 0; i < arr3.length; i++){
            System.out.print(arr3[i]+" ");
        }
    }
}
//结果：1，2，3，4
```

### 3.5 数组查询
>在进行数组查询之前，必须将数组排好序(sort()方法)；
>没有排序进行查找，结果是不确定的；
>如果数组中包含多个指定值的元素，则无法保证找到的是哪一个； 

#### binarySearch(Object [] a, Object key)
+ 作用：返回在指定数组中搜索值的索引，这里的返回值是指对数组进行排序后的元素的索引。
+ 如果搜索值不在数组中，返回-1或”-“(插入点)。插入点是搜索值将要插入数组的那一点，即第一个大于此值的索引。

#### binarySearch(Object [] a, int fromIndex, int toIndex, Object key)
+ 作用：返回在指定数组指定范围内搜索值的索引，同样是排序后的索引。
+ e.g:
```java
import java.util.Arrays;
public class BinarySerach {
    public static void main(String [] args){
        String [] str = new String[]{"ab", "cd", "ef", "gh"};
        Arrays.sort(str);
        int index1 = Arrays.binarySearch(str, 0, 2, "cd");
        System.out.println("cd的索引位置是："+index1);//1
    }
}
```

## 4. 数组排序算法

### 4.1 冒泡排序
1. 基本思想
    冒泡排序的基本思想是对比相邻的元素值。如果满足条件就交换元素值，把较小的元素移动到数组前面，大的元素移动到数组后面，像气泡一样往上升。
2. 冒泡算法 
    由双层循环实现，外层循环控制排序次数，排序次数为数组长度-1，因为最后一次循环只剩下一个数组元素，不需要对比;
    内层循环用于对比数组中每个临近元素的大小，以确定是否交换位置，对比和交换次数随着排序轮数而减少;
3. 算法实现
```java
public class BubbleSort {
    public void sort(int [] a){
        for (int i = 1; i < a.length; i++){
            for (int j = 0; j < a.length-i; j++){
                if (a[j] > a[j+1]){
                    int temp = a[j];
                    a[j] = a[j+1];
                    a[j+1] = temp;
                }
            }
        }
        showArray(a);
    }
    public void showArray(int [] a){
        for (int i = 0; i < a.length; i++){
            System.out.print(a[i]+" ");
        }
    }
    public static void main(String [] args){
        BubbleSort soter = new BubbleSort();
        int [] arr = new int[]{1, 3, 2, 4, 5, 6, 7, 8, 9};
        soter.sort(arr);
    }
}
```

### 4.2 选择排序
1. 基本思想
    选择排序的基本思想是将指定排序位置与其他数组元素分别对比，如果满足条件就交换元素值
2. 选择算法
    外层循环控制排序次数，同样为数组长度-1；
    内层循环用与查找最大值或最小值。从未排序的数组中选取第一个元素作为基准元素，与其他未排序元素进行对比，如果大于或小于，就替换为新基准元素，直到选出最大或最小的元素值；
    选出最大值或最小值后，与当前元素的下标进行比较，如果不同就交换，否则不交换。
3. 算法实现
```java
public class SelectSort{
    //从后往前排序
    public void sort(int [] a){
        for (int i = 1; i < a.length; i++){
            int minid = 0;
            for (int j = 1; j <= a.length-i; j++){
                if (a[minid] > a[j]){
                    minid = j;
                }
            }
                int temp = a[minid];
                a[minid] = a[a.length-i];
                a[a.length-i] = temp;
        }
        showArray(a);
    }
    //从前往后排序
    public void sort_2(int [] a){
        for (int i = 0; i < a.length-1; i++){
            int minid = i;
            for (int j = i+1; j < a.length; j++){
                if (a[minid] > a[j]){
                    minid = j;
                }
            }
            if (minid != i){
               int temp = a[minid];
               a[minid] = a[i];
               a[i] = temp;
            }
        }
    showArray(a);
    }

    public void showArray(int [] a){
        for (int e: a){
            System.out.print(e+" ");
        }
    }
    public static void main(String[] args) {
        SelectSort sorter = new SelectSort();
        int [] arr = {1, 2, 5, 6, 7, 8, 4, 9, 7};
        sorter.sort_2(arr);
    }
}
```

### 4.3 反转排序
1. 基本思想
    数组最后一个元素与第一个元素交换，倒数第二个元素与第二个元素交换，以此类推，直到所有数组元素反转替换；
2. 反转算法
    单层循环，循环次数为数组长度/2;
3. 算法实现
```java
public class ReverseSort {
    public static void main(String[] args) {
        int [] arr = {10, 20, 30, 40, 50, 60, 70, 80};
        ReverseSort sorter = new ReverseSort();
        sorter.showArray(arr);
        sorter.sort(arr);
        sorter.showArray(arr);
    }
    /**
     * 反转排序
     * @param array
     * 要排序的数组
     */
    public void sort(int [] array){
        int len = array.length;
        for (int i = 0; i < len/2; i++){
            int temp = array[i];
            array[i] = array[len-i-1];
            array[len-i-1] = temp;
        }
    }
    /**
     * 显示数组中的元素
     * @param array
     * 要显示的数组
     */
    public void showArray(int [] array){
       for (int e: array){
           System.out.print(e+" ");
       }
       System.out.println();
    }
}
```

