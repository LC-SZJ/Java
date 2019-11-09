[TOC]

# Java 基础语法

## 1. 常量（6种）

常量：在程序运行期间，不可改变的量。

1. 字符串常量：“abc"
2. 字符常量：'a'
3. 整型常量：123
4. 浮点型常量：3.14
5. 布尔常量：true、false
6. 空常量：null

> 注意事项：
> 1. 字符常量单引号中的字符有且仅有一个，没有不行，多个也不行。
> 2. 不能直接打印空常量。
> 

## 2. 基本数据类型（4类8种）

1. 整数型：byte、short、int、long
2. 浮点型：float、double
3. 字符型：char
4. 布尔型：boolean

> 注意事项：
> 1. 字符串不是基本类型，而是引用类型。
> 2. 浮点型可能只是一个近似值，并非精确的值。
> 3. 数据范围与字节数不一定相关，例如：float 数据范围比 long 更加广泛，但是 float 是4字节，long 是8字节。
> 4. 浮点数当中默认类型是 double，如果一定要是用 float 类型，需要加上一个后缀 F。
如果是整数，默认为 int 类型，如果该一定要使用 long 类型，需要加上一个后缀L。推荐使用大写字母。


## 3. 变量

+ 变量：在程序运行期间，内容可以发生改变的量。
+ 格式：
```java
数据类型 变量名称;
变量名称 = 数据值;
// 或者
数据类型 变量名称 = 数据值;
```

```java
// 右侧数值的范围不能超过左侧数据类型的取值范围，否则报错
// byte是：-128~127
byte num1 = 30; // 正确
byte num2 = 300; // 错误
// float后缀F
float num3 = 100F;
// long后缀L
long num4 = 200L;
```

> 注意事项：
>1. 如果创建多个变量，那么变量之间的名称不可以重复。
>2. 对于 float 和 long 类型来说，字母后缀 F和 L不要丢掉。
>3. 如果使用 byte 和 short 类型的变量，那么右侧的数字值不能超过左侧类型的取值范围。
>4. 没有进行赋值的局部变量，不能直接使用；一定要赋值之后，才能使用。
>5. 变量使用不能超过作用域范围。
>【作用域】：从定义变量的一行开始，一直到所属的大括号结束为止。
>6. 可以通过一个语句来创建多个变量，但是一般情况不推荐这么写。
>




## 4. 数据类型装换
当数据类型不一样的时候，将会发生数据类型装换。
+ 自动类型装换（隐式）
1. 特点：代码不需要进行特殊处理，自动完成。
2. 规则：***数据范围从小到大***。
```java
// long --> float,范围是float更大一些，符合从下到大的原则。
float num1 = 1.2L;
```

> 数据范围从小到大与它的字节数是不一定相关的。


+ 强制类型装换（显式）
1. 特点：代码需要进行特殊的格式处理，不能自动完成。
2. 格式：范围小的类型 范围小的变量名 = (范围小的类型) 原本范围大的数据;

```java
int num1 = (int) 100L;

```

> 注意事项：
> 1. 强制类型装换一般不推荐使用，因为有可能发生精度损失，数据溢出。
> 2. byte/short/char这三种类型都可以发生数学运算，例如：加法“+”。
> 3. byte/short/char这三种类型在运算的时候，都会被首先提升为int类型，然后再计算。
> 4. boolean类型不能发生数据类型装换。
> 

+ 示例

```java
public class DemoDataType{
	public static void main(String [] args){
		// long类型强制装换成int类型，数据溢出
		int num1 = (int)6000000000L;
		System.out.println(num1); // 1705032704
		
		// double --> int 强制类型装换，精度损失
		int num2 = (int)3.99;
		// 这并不是四舍五入，所有的小数位都会被舍弃掉
		System.out.println(num2); // 3
		
		// 计算机底层会用一个数字（二进制）来代表字符A，也就是65
		// 一旦char类型进行了数学运算，那么字符就会按照一定的规则翻译成一个数字
		char zifu1 = 'A';
		System.out.println(zifu1 + 1); // 66
		// 注意：右侧数值大小不能超过左侧的类型范围
		byte num3 = 30; 
		byte num4 = 40;
		// byte + byte --> int int --> int 
		// byte result1 = num3 + num4; // 错误写法！
		int result1 = num3 + num4; 	
		System.out.println(result1); // 70
			
		short num5 = 60;
		// byte + short --> int + int --> int
		// int 强制装换成为 short：
		// 注意必须保证逻辑上的真实大小本来就没有超过short范围，否则会发生数据溢出
		short result2 = (short)(num4 + num5);
		System.out.println(result2);
	}
}
```

## 5. 数字和字符的对照关系表（编码表）

+ ASCII码表：American Standard Code for Information Interchange，美国信息交换标准代码。
+ Unicode码表：万国码，也是数字和字符的对照关系，开头0~127部分和ASCII完全一样，但是从128开始包含有更多字符。
```java
48 - '0'
65 - 'A'
97 - 'a'
```

## 6. 运算符

### 1. 算术运算符

#### 四则运算符及取模

+ 对于一个整数表达式来说，除法用的是整数，整数除以整数，结果仍然是整数，只看商，不看余数。
+ 只有对于整数的除法来说，取模运算符才有余数的意义。
> 注意事项：一旦运算符当中有不同的数据类型，那么结果将会是数据类型范围大的那种。（谁大听谁的）。
> 

+ 四则运算当中的加号“+”有常见的三种用法：
    + 对于数值来说，那就是加法。
    + 对于字符char类型来说，在计算之前，char会被提升成为int，然后再计算。
    + 对于字符串String（首字母大写，不是关键字）来说，加号代表字符串***连接***操作。***任何数据类型和字符串进行连接的时候，结果都会变成字符串***。

#### 自增自减运算符

自增运算符：++
自减运算符：--
+ 基本含义：让一个变量涨一个数字1，或者让一个变量降一个数字1
+ 使用格式：写在变量名称之前，或者写在变量名称之后，例如：++num，也可以num++
+ 使用方式：
    1. 单独使用：不和其他任何操作混合，自己独立成为一个步骤。
    2. 混合使用：和其他操作混合，例如与赋值混合，或者与打印混合，等。
+ 使用区别：
    1. 在单独使用的时候，前++和后++没有任何区别，也就是：++num;和num++;是完全一样的。
    2. 在混合使用的时候，有【重大区别】
        1. 前++：先加后用
        2. 后++：先用后加

> 注意事项：只有***变量***才能使用自增、自减运算符。常量不可发生改变，所以不能用。
> 

### 2. 赋值运算符

+ 基本赋值运算符：就是一个等号 “=”。
+ 复合赋值运算符：+=、-=、*=、/=、%=。

> 注意事项：
> 1. 只有变量才能使用赋值运算符，常量不能进行赋值。
> 2. 复合赋值运算符其中隐含了一个强制类型转换。
> 

```java
byte num = 30;
// num = num + 30;
// num = byte + int
// num = int + int
// num = int
// num = (byte)int;
num += 20; 
```

### 3. 比较运算符

`>、<、==、>=、<=、!=`

> 注意事项：
>1. 比较运算符的结果一定是个boolean值，成立就是true，不成立就是false。
>2. 如果进行多次判断，不能连着写。
>

### 4. 逻辑运算符

与“&&”、或“||”，具有短路效果：如果根据左边已经可以判断得到的最终结果，那么右边的代码将不再执行，从而节省一定的性能。

> 注意事项：
> 1. 逻辑运算符只能用于boolean值。
> 2. 与、或需要左右各一个boolean值，但是取反只要有唯一的一个boolean值即可。
> 3. 与、或两种运算符，如果有多个条件，可以连续写。
> 

### 5. 三元运算符

格式：数据类型 变量名称 = 条件判断 ? 表达式A: 表达式B;

> 注意事项：
> 1. 必须同时保证表达式A和表达式B都符合左侧数据类型的要求。
> 2. 三元运算符的结果必须被使用。
> 


## 7. 普通方法

定义方法的格式：
```java
public static 返回值类型 方法名称(){
		 方法体
		 return 返回值;
}
```
方法名称命名规则和变量名称一样，使用小驼峰。

> 注意事项：
> 1. 方法定义的先后顺序无所谓。
> 2. 方法的定义不能产生嵌套包含关系。
> 3. 方法定义好了之后，不会执行。如果要想执行，一定要进行方法的***调用***。
> 

调用方法的三种格式：
1. 单独调用：方法名称(参数);
2. 打印调用：System.out.println(方法名称(参数));
3. 赋值调用：数据类型 变量名称 = 方法名称(参数);

> 注意事项：
> 对于有返回值的方法，可以使用单独调用、打印调用或者赋值调用。
> 但是对于无返回值的方法，只能使用单独调用，不能使用打印调用或者赋值调用。
> 


## 8. jshell
jshell时Java运行程序的时候的一个脚本执行程序，就像是剧本：你怎么写的，我就怎么演。

进入jshell：`jshell`
退出jshell：`/exit`

## 9. 编译器的两点优化

### 1. 编译器的隐含强制类型转换

对于***byte / short / char***三种类型来说，如果右侧赋值的数值没有超过范围，那么javac编译器将会自动隐含地为我们补上一个***(byte) (short) (char)***。这不是自动类型装换，而是强制类型装换，自动类型转换是：从小范围 --> 大范围

1. 如果没有超过左侧的范围，编译器补上强转。
2. 如果右侧超过了左侧范围，那么直接编译器报错。

```java
public class Demo01Notice{
	public static void main(String [] args){
		
		// int --> byte，不是自动类型转换
		byte num = /*(byte)*/30;
		System.out.println(num); // 30
		
		// int --> char，没有超过范围
		// 编译器将会自动补上一个隐含的(char)
		char zifu = /*(char)*/65;
		System.out.println(zifu); // A
	}
}
```



### 2. 编译器的常量优化

在给变量进行赋值的时候，如果右侧的表达式当中全都是常量，没有任何变量，那么编译器javac将会直接将若干个常量表达式计算得到结果。
```java
short result = 5 + 8; // 等号右边全都是常量，没有任何变量参与运算
```

编译之后，得到的.class字节码文件当中相当于【直接就是】：
```java
short result = 13; // 右侧的常量结果数值，没有超过左侧范围，所以正确。
```

这称为”编译器的常量优化“。
但是注意：一旦表达当中有变量参与，那么就不能进行这种优化了。

```java
public class Demo02Notice{
	public static void main(String [] args){
		short a = 5;
		short b = 8;
		
		// short result1 = a + b; // 错误写法！
		// System.out.println(result1);
		
		short result2 = 5 + 8; // 正确写法！
		System.out.println(result2); // 13
	}
}
```

## 10. 方法的重载（Overlaod）
+ 定义：方法名称一样，参数列表不一样。
+ 好处：只需要记住唯一一个方法名称，就可以实现类似的多个功能。

方法重载与下列因素有关：
1. 参数个数不同
2. 参数类型不同
3. ***参数的多类型顺序不用***

方法重载与下列因素无关：
1. 与参数的名称无关
2. 与方法的返回值类型无关

+ 题目一

```java
/*
题目要求：
用一个方法判断两个整数是否相等
*/
public class DemoMethodSame {
    public static void main(String[] args) {
        System.out.println(isSame(1,1));
    }

    public static boolean isSame(int a, int b){
        // 第一种
        /*boolean same;
        if (a == b){
            same = true;
        }else{
            same = false;
        }*/
        // 第二种
//        boolean same = a == b ? true : false;
        // 第三种
//        boolean same = a == b;
//        return same;
        // 第四种
        return a == b;
    }
}
```

+ 题目二
```java
/*
题目要求：
对byte、short、int、long四种数据类型实现判断两个数是否相等的重载
*/
public class DemoMethodOverloadSame {
    public static void main(String[] args) {
        byte a = 10;
        byte b = 20;
        System.out.println(isSame(a, b));
        System.out.println(isSame(10L, 10L));
        System.out.println(isSame((short)10, (short)20));
    }

    public static boolean isSame(byte a, byte b){
        if (a == b){
            return true;
        }else{
            return false;
        }
    }
    
    public static boolean isSame(int a, int b){
        boolean same;
        if (a == b){
            same = true;
        }else{
            same = false;
        }
        return same;
    }
    
    public static boolean isSame(short a, short b){
        boolean same = a == b ? true : false;
        return same;
    }
    
    public static boolean isSame(long a, long b){
        return a == b;
    }
}
```


## 11. switch语句

> **注意事项**
>
> 1. 多个case后面的数值不可以重复。
> 2. switch后面小括号当中只能是下列数据类型：
> **基本数据类型：byte / short / char / int
> 引用数据类型：(JDK1.7之后)String字符串、(JDK1.5之后)enum枚举**
> 3. switch语句格式可以很灵活：前后顺序可以颠倒，而且break语句还可以省略。
> “匹配哪一个case，就从哪一个位置向下执行，直到遇到了break或者整体结束为止”。
