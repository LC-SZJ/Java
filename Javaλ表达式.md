[TOC]


#  Java Lambda表达式 — λ

在 JDK 1.8 中加入了Lambda表达式

## 1. 函数式编程思想

+ 面向对象思想：
  做一件事情，找一个能解决这个事情的对象，调用对象的方法，完成事情。
+ 函数式编程思想：
  只要能获取到结果，谁去做的，怎么做的都不重要，重视的是结果，不是过程。

## 2. 体验Lambda的更优写法

+ 匿名内部类的方式

```java
new Thread(new Runnable() {
            @Override
            public void run() {
              System.out.println(Thread.currentThread().getName()+"线程3号");
            }
        }).start();
```

+ Lambda 表达式的方式

```java
new Thread(() -> {
            System.out.println(Thread.currentThread().getName()+"λ");
        }).start();
```

## 3. Lambda表达式的标准格式
1. 一些参数
2. 一个箭头
3. 一段代码

+ 格式：

```java
(参数列表) -> {一些重写方法的代码};
```

+ 解释说明格式：
    + ()：接口中抽象方法的参数列表，没有参数就空着；有参数就写出参数，多个参数使用逗号分隔。
    + ->：传递的意思，把参数传递给方法体 { }
    + {}：重写接口的抽象方法的方法体


## 4. 练习：使用Lambda表达式（无参无返回）

```java
/*
题目：
给定一个厨子Cook接口，内含唯一的抽象方法 makeFood，且无参数，无返回值
使用 Lambda表达式的标准格式调用 invokeCook方法，打印输出“吃饭啦！”字样
 */
public class Demo01Cook {
    public static void main(String[] args) {
        // 使用匿名内部类
        invokeCook(new Cook() {
            @Override
            public void makeFood() {
                System.out.println("吃饭啦！");
            }
        });
        // 使用 Lambda表达式
        invokeCook(() -> {
            System.out.println("吃饭啦！");
        });
    }

    public static void invokeCook(Cook cook) {
       cook.makeFood();
    }

}

// ====================
public interface Cook {
    public abstract void makeFood();
}

```

## 5. 练习：使用Lambda表达式（有参有返回）

```java
import java.util.Arrays;
import java.util.Comparator;

/*
需求：
使用数组存储多个Person对象，对数组中的多个Person对象
按照年龄进行升序排序，并打印输出
 */
public class Demo01Arrays {
    public static void main(String[] args) {
        Person[] arr = {
                new Person("柳岩", 38),
                new Person("迪丽热巴", 18),
                new Person("马儿扎哈", 19)
        };

        /*Arrays.sort(arr, new Comparator<Person>() {
            @Override
            public int compare(Person o1, Person o2) {
                return o1.getAge() - o2.getAge();
            }
        });*/

        // 使用 Lambda表达式
        Arrays.sort(arr, (Person o1, Person o2) -> {
            return o1.getAge() - o2.getAge();
        });

        for (Person person : arr) {
            System.out.println(person);
        }

    }
}
```


## 6. 练习：使用Lambda表达式（有参有返回）

```java
/*
需求：
给定一个 Calculator计算器接口，内含抽象方法 calc，可以将两个 int型数字相加
返回和，使用Lambda表达式的标准格式调用 invokeCalc方法，完成120和130的计算
 */
public class DemoCalculator {
    public static void main(String[] args) {
        // 使用匿名内部类调用
        /*invokeCalc(120, 130, new Calculator() {
            @Override
            public int calc(int a, int b) {
               return a + b;
            }
        });*/
        // 使用Lambda表达式调用
        invokeCalc(120, 130, (int a, int b) -> {
            return a + b;
        });

    }

    public static void invokeCalc(int a, int b, Calculator c) {
        int result = c.calc(a, b);
        System.out.println("结果是：" + result);
    }
}
// ================

public interface Calculator {
    public abstract int calc(int a, int b);
}
```

## 7. Lambda的省略格式

+ Lambda表达式：是可推导，可省略的。
+ 凡是根据上下文推导出来的内容，都可以省略书写。
+ 可以省略的内容：
    1. （参数列表）：括号中参数列表的**数据类型**，可以省略不写。
    2. （参数列表）：括号中的参数如果只有一个，那么**类型和()**都可以省略。
    3. （一些代码）：如果**{ }**中的代码只有一行，**无论是否有返回值**，都可以省略（**{ }，return，分号**）。
    
> 注意：要省略**{ }，return，分号**必须一起省略。
> 

## 8. Lambda的使用前提

1. 使用 Lambda必须具有接口，且要求**接口中有且仅有一个抽象方法**。
    无论是 JDK内置的`Runnable`、`Comparator`接口还是自定义的接口，只有当接口中的抽象方法存在且唯一时，才可以使用 Lambda。
2. 使用 Lambda必须具有**上下文的推断**。
    也就是方法的参数或者是局部变量类型必须为 Lambda对应的接口类型，才能使用 Lambda作为该接口的实例。
> 备注：有且仅有一个抽象方法的接口，称为“ **函数式接口** ”。
