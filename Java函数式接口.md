[TOC]

# Java 函数式接口

## 1. 函数式接口

### 1.1 概念

- 函数式接口：有且只有一个抽象方法的接口，称之为函数式接口。
- 当然接口中还可以包含其他的方法(静态，默认，私有)
- @FunctionalInterface注解
    - 作用：可以检测接口是否是一个函数式接口
        - 是：编译成功
        - 否：编译失败(接口中没有抽象方法，或抽象方法的个数多余1个)

> 备注：“**语法糖**”，是指使用更加方便，但是原理不变的代码语法。例如在遍历集合时使用的 for-each语法，其实底层的实现原理仍然是迭代器，这便是"**语法糖** "。从应用层面来讲，Java中的 Lambda可以被当作是匿名内部类的的 "语法糖"，但是二者在原理上是不同的。
> 

### 1.2 格式

只要确保接口中有且仅有一个抽象方法即可：

```java
	修饰符 interface 接口名称 {
	public static abstract 返回值类型 函数名称(参数列表);
	
	// 其他非抽象方法内容
	}
```

### 1.3 函数式接口的使用

```java
@FunctionalInterface
public interface MyFunctionalInterface {
    public abstract void method();

}

// ===============
public class MyFunctionalInterfaceImpl implements MyFunctionalInterface {
    @Override
    public void method() {
        System.out.println("使用实现类重写接口中的抽象方法");

    }
}

// ============

/*
    函数式接口的使用：一般可以作为方法的参数和返回值类型
 */
public class Demo01MyFunctionalInterface {
    public static void main(String[] args) {
        // 调用show方法，方法的参数是一个函数式接口，所以可以传递接口的实现类对象
        show(new MyFunctionalInterfaceImpl());
        // 调用show方法，方法的参数是一个函数式接口，所以可以传递接口的匿名内部类
        show(new MyFunctionalInterface() {
            @Override
            public void method() {
                System.out.println("使用匿名内部类重写接口中的抽象方法");
            }
        });

        // 使用lambda表达式
        show(() -> {
            System.out.println("使用lambda表达式重写接口中的抽象方法");
        });
        // 简化lambda表达式
        show(() -> System.out.println("简化之后"));

    }

    /*
    定义一个方法，方法的参数是一个函数式接口MyFunctionalInterface
     */
    public static void show(MyFunctionalInterface myinter) {
        myinter.method();
    }
}
```



## 2. 函数式编程

### 2.1 Lambda的延迟执行

#### 性能浪费的日志案例

> 备注：日志可以帮助我们快速定位问题，记录程序运行过程中的情况，以便项目的监控和优化。

```java
/*
    日志案例
    问题：
        发现以下代码存在一些性能浪费的问题
        调用showLog方法，方法的第二个参数是一个拼接的字符串
        先把字符串拼接好，然后调用showLog方法
        showLog方法中如果传递的日志等级不是1级
        那么就会觉得拼接好的字符串没有使用，存在了浪费
 */
public class Demo01Logger {
    /*
        定义一个根据日志级别，显示日志信息的方法
     */
    public static void showLog(int level, String message) {
        // 对日志级别进行判断，如果是1级别，那么输出信息
        if (1 == level) {
            System.out.println(message);
        }
    }
    public static void main(String[] args) {
        // 定义三个日志信息
        String msgA = "Hello";
        String msgB = "World";
        String msgC = "Java";

        showLog(2, msgA+msgB+msgC);

    }
}
```

#### 使用Lambda表达式的优化

```java
public interface Message {
    // 定义一个返回拼接字符串的抽象方法
    public abstract String builderMessage();
}

// =========
/*
    使用Lambda表达式优化日志案例
    Lambda的特点：延迟加载
    Lambda的使用前提：必须存在函数式接口

    使用Lambda表达式作为参数传递，仅仅是把参数传递到showLog方法中
    只有满足条件，日志的等级是1级
        才会调用接口Message中的builderMessage方法
        才会进行字符串的拼接
    如果不满足条件，日志的等级不是一级
    那么Message接口中的方法builderMessage不会执行
    所以拼接字符串的代码也不会执行，不会存在性能的浪费

 */
public class Demo02Logger {

    public static void main(String[] args) {
        // 定义三个日志信息
        String msgA = "Hello";
        String msgB = "World";
        String msgC = "Java";
        // 调用showLog方法，参数msg是一个函数式接口，所以可传递Lambda表达式
        showLog(2, () -> {
            System.out.println("不满足条件不执行");
            return msgA+msgB+msgC;
        });

    }
    /*
        定义一个方法，用来显示日志信息
        日志等级和函数式接口Message作为方法的参数
     */
    public static void showLog(int level, Message msg) {
        if (level == 1) {
            System.out.println(msg.builderMessage());
        }
    }
}
```

### 2.2 使用Lambda作为方法的参数&返回值

  例如 java.lang.Runnable接口是一个函数式接口。假设有一个startThrea方法使用该接口作为方法的参数，那么就可以使用Lambda进行传参。这种情况其实和Thread类的构造方法参数为Runnable没有本质区别

```java
public class Demo01Runnable {
    // 定义一个方法startThread，方法的参数使用函数式接口Runnable
    public static void startThrea(Runnable run) {
        // 开启多线程
        new Thread(run).start();
    }
    public static void main(String[] args) {
        // 调用startThread方法，方法的参数是一个函数式接口，所以可以使用匿名内部类
        startThrea(new Runnable() {
            @Override
            public void run() {
                System.out.println(Thread.currentThread().getName()+"--> 多线程启动了");
            }
        });

        // 使用Lambda表达式
        startThrea(() -> {
            System.out.println(Thread.currentThread().getName()+"--> 多线程开启了");
        });


    }
}
```

  如果一个方法的返回值类型是一个函数式接口，那么就可以直接返回一个Lambda表达式。当需要通过一个方法，来获取一个 `java.util.Comparator`接口类型的对象作为排序器时，就可以调用该方法获取。

```jaVa
import java.util.Arrays;
import java.util.Comparator;

public class Demo02Comparator {
    // 定义一个方法，方法的返回值类型使用函数式接口Comparator
    public static Comparator<String> getComparator() {
        // 方法的返回值是一个接口，我们可以返回这个接口的匿名内部类
       /*return new Comparator<String>() {
           // 按照字符串的降序排序
           @Override
           public int compare(String o1, String o2) {
              return o2.length() - o1.length();
           }
       };*/
       // 方法的返回值是一个接口，所以我们可以使用Lambda表达式
        /*return (String o1, String o2) -> {
            return o2.length() - o1.length();
        };*/
        // 进一步优化
        return (o1,o2) -> o2.length() - o1.length();
    }
    public static void main(String[] args) {
        String[] arr = {"111", "2222", "333333333", "44444"};
        System.out.println("排序前:");
        System.out.println(Arrays.toString(arr));
        System.out.println("排序后:");
        Arrays.sort(arr, getComparator());
        System.out.println(Arrays.toString(arr));

    }
}
```


## 3. 常用的函数式接口

### 3.1 Supplier`<T>`接口

- java.util.function.Supplier`<T>`接口仅包含一个无参的方法：
    - `T get()`：用来获取一个泛型参数指定类型的对象数据。

- Supplier`<T>`接口被称之为生产型接口，指定接口的泛型是什么类型，那么接口中的 get方法就会产生什么类型的数据。

- 示例


```java
/*
Supplier<T>接口的使用
 */
import java.util.function.Supplier;

public class DemoSupplier {
    // 定义一个方法，方法的参数传递Supplier<T>接口，泛型指定为String，get方法就返回一个String
    public static String getString(Supplier<String> sup) {
        return sup.get();
    }
    public static void main(String[] args) {
        // 调用getString方法，方法的参数是一个Supplier<T>接口,即函数式接口，可以使用Lambda表达式
        // 重写其中的 get方法
        String s1 = getString(() -> {
            return "胡歌";
        });
        System.out.println(s1);

        // 优化Lambda表达式
        String s2 = getString(() -> "胡歌");
        System.out.println(s2);
    }
}
```

#### 练习：求数组元素的最大值

```java
import java.util.function.Supplier;

/*
    练习：求数组元素的最大值
        使用Supplier接口作为方法参数类型，通过Lambda表达式找出int型数组的最大值
        提示：接口的泛型使用 java.lang.Interger类
 */
 


public class DemoSupplierTest {
    /*
        定义一个方法，用于获取 int型数组的最大值，方法的参数传递Supplier<T>接口
        泛型使用 Integer
     */
    public static int getMax(Supplier<Integer> sup) {
        return sup.get();
    }
    public static void main(String[] args) {
        // 定义一个数组
        int[] arr = {12, 21, 100, 999, 8080, 1321, 10000};
        // 调用 getMax方法，方法的参数传递 Lambda表达式
        int maxValue = getMax(() -> {
          int max = arr[0];
            for (int i = 1; i < arr.length; i++) {
               if (max < arr[i]) {
                   max = arr[i];
               }
            }
            return max;
        });
        System.out.println("数组中元素的最大值为："+maxValue);
    }
}
```

### 3.2 Consumer`<T>`接口

- java.util.function.Consumer`<T>`接口则正好与 Supplier`<T>`接口相反，它不是生产一个数据，而是消费一个数据，其数据类型由泛型决定。
- Comsumer接口中包含抽象方法：`void accept(T t)`，意为消费一个指定泛型的数据。
- Comsumer接口是一个消费型接口，泛型指定什么类型，就可以使用 accept方法消费什么类型的数据，至于具体怎么消费（使用），需要自定义（输出，计算……）

- 示例

```java
import java.util.function.Consumer;

public class DemoComsumer {
    /*
        定义一个方法，参数：字符串和 Consumer<T>接口，泛型指定 String
        用来使用 Consumer接口消费字符串
     */
    public static void consume(String name, Consumer<String> con) {
        con.accept(name);
    }

    public static void main(String[] args) {
        consume("赵丽颖", (String name) -> {
            // 消费方式 打印输出
            // System.out.println(name);
            // 消费方法 反转
            /*
                StringBuilder是一个字符串缓冲区，里面有一个方法
                reverse方法可以将此字符序列反转，再使用 toString方法转换成String类
             */
            String rename = new StringBuilder(name).reverse().toString();
            System.out.println(rename);
        });
    }
}
```

#### 默认方法：andThen
  如果一个方法参数和返回值都是`Consumer`类型，那么就可以实现效果：消费数据的时候，首先做一个操作，然后再做一个操作，实现组合（连接），而这个方法就是`Consumer`接口中的 default方法`andThen`方法，下面式 JDK源码：

```java
	default Consumer<T> andThen(Consumer<? super T> after) {
        Objects.requireNonNull(after);
        return (T t) -> { accept(t); after.accept(t); };
    }
```

> 备注：
> `java.util.Objects`的`requireNonNull`静态方法将会在参数为 null 时主动抛出 `NullPointerException`异常。这省去了重复编写 if语句和抛出空指针异常的麻烦。

- 示例：


```java
import java.util.function.Consumer;

/*
    Consumer接口的默认方法：andThen
    作用：需要两个 Consumer接口，可以把两个Consumer接口组合到一起,对数据进行消费

    例如：
        Consumer<String> con1;
        Consumer<String> con2;
        String s= "Hello";

        con1.accept(s);
        con2.accpet(s);
        这两行等价于
        con1.andThen(con2).accept(s);
    注：谁写前边，谁先消费

 */
public class DemoAndThen {
    /*
        定义一个方法，参数传递一个字符串和两个 Consumer接口，泛型都使用 String
     */
    public static void method(String s, Consumer<String> con1, Consumer<String> con2) {
//        con1.accept(s);
//        con2.accept(s);
        con1.andThen(con2).accept(s);

    }

    public static void main(String[] args) {
        String s = "Hello";
        method(s,
                (t) -> {
                    // 消费方式：把字符串变成大写
                    System.out.println(s.toUpperCase());
                },
                (t) -> {
                    // 消费方式：把字符串变成小写
                    System.out.println(s.toLowerCase());
                }
        );
    }
}
```

#### 练习：格式化打印信息


```java
import java.util.function.Consumer;

/*
    练习：
        字符串数组中有多条信息，请按照格式“姓名：xx。性别：xx。"的格式将信息打印出来
        要求将打印姓名的动作作为第一个Consumer接口的实例
        将打印性别的动作作为第二个 Consumer接口的实例
 */
public class AndThenTest {
    /*
        定义一个方法，参数为一个字符串数组，两个Consumer接口，泛型都使用String
     */
    public static void printInfo(String[] arr, Consumer<String> con1, Consumer<String> con2) {
        for (String message : arr) {
            con1.andThen(con2).accept(message);
        }
    }

    public static void main(String[] args) {
        String[] arr = {"迪丽热巴,女", "古力娜扎,女", "马儿扎哈,男"};
        printInfo(arr,
                (message) -> {
                    // 对字符串进行切割,获取姓名
                    String name = message.split(",")[0];
                    System.out.print("姓名：" + name + "。");
                },
                (message) -> {
                    // 对字符串进行切割,获取性别
                    String sex = message.split(",")[1];
                    System.out.println("性别：" + sex + "。");
                });
    }
}
```

### 3.3 Predicate`<T>`接口

- java.util.function.Predicate`<T>`接口
- 作用：对某种数据类型的数据进行判断，结果返回一个 boolean值
- Predicate接口中包含一个抽象方法：
    - `boolean test(T t)`：用来对指定的数据类型进行判断
    - 结果：
        - 符合条件，返回 true
        - 不符合条件，返回 false

- 示例：

```java
import java.util.function.Predicate;

public class DemoPredicate {
    /*
        定义一个方法，参数传递一个字符串
        和一个Predicate接口，泛型使用String
        使用Predicate中的方法 test对字符串进行判断，并把判断的结果返回
     */
    public static boolean checkString(String s, Predicate<String> pre) {
        return pre.test(s);
    }

    public static void main(String[] args) {
        String s = new String("abcdef");
        // 调用checkString方法，函数式接口，使用Lambda表达式
        /*boolean b = checkString(s, (str) -> {
            return s.length() > 5;
        });*/

        // 对Lambda表达式进行优化
        boolean b = checkString(s, str -> str.length() > 5);
        System.out.println(b);

    }
}
```

#### 默认方法：and

  - **JDK源码:**

```java
default Predicate<T> and(Predicate<? super T> other) {
        Objects.requireNonNull(other);
        return (t) -> test(t) && other.test(t);
    }
```

- 示例：

```java
import java.util.function.Predicate;

/*
    需求：
        判断一个字符串，有两个判断条件
        1. 字符串的长度是否大于5
        2. 字符串中是否包含a
    两个条件必须同时满足，我们就可以使用 && 运算符连接
 */
public class DemoPredicate_And {
    public static boolean checkString(String s, Predicate<String> pre1, Predicate<String> pre2) {
        return pre1.and(pre2).test(s);
        // 等价于 return pre1.test(s) && pre2.test(s);
    }

    public static void main(String[] args) {
        String s = "I Love Java!";
        boolean b = checkString(s, (String str) -> {
            // 对字符串长度是否大于5进行判断
            return str.length() > 5;
        }, (String str) -> {
            // 对字符串中是否包含a进行判断
            return str.contains("a");
        });
        System.out.println(b);
    }

}
```

#### 默认方法：or

- **JDK源码：**

```java
default Predicate<T> or(Predicate<? super T> other) {
        Objects.requireNonNull(other);
        return (t) -> test(t) || other.test(t);
    }
```


#### 默认方法：negate

- **JDK源码：**

```java
 default Predicate<T> negate() {
        return (t) -> !test(t);
    }
```

#### 练习：集合信息的筛选

```java
import java.util.ArrayList;
import java.util.function.Predicate;

/*
题目：
    数组当中有多条"姓名+性别"的信息如下：
    String[] array = {"迪丽热巴,女", "古力娜扎,女", "马儿扎哈,男", "赵丽颖, 女"}; 请通过Predicate接口的拼装，将符合要求的字符串筛选到ArrayList集合中
    筛选条件须同时满足：
    1. 必须为女生
    2，名字为4个子
分析:
    1.有两个判断条件，所以需要使用两个Predicate接口
    2.必须同时满足两个条件，所以可以使用 and方法

 */
public class PredicateTest {
    public static ArrayList<String> filter(String[] arr, Predicate<String> pre1, Predicate<String> pre2) {
        // 创建一个ArrayList集合，存储过滤之后的信息
        ArrayList<String> list = new ArrayList<>();
        for (String s : arr) {
            // 使用and进行与操作
            boolean b = pre1.and(pre2).test(s);
            // 如果同时满足条件
            if (b) {
                list.add(s);
            }
        }
        // 返回过滤之后的集合
        return list;
    }

    public static void main(String[] args) {
        String[] array = {"迪丽热巴,女", "古力娜扎,女", "马儿扎哈,男", "赵丽颖, 女"};

        // 调用filter方法，使用Lambda表达式
        ArrayList<String> list = filter(array, (String s) -> {
            // 对是否是女生进行判断
            return s.split(",")[1].equals("女");

        }, (String s) -> {
            // 对名字是否是四个字进行判断
            return s.split(",")[0].length() == 4;

        });
        // 对过滤之后的集合进行输出
        for (String s : list) {
            System.out.println(s);
        }
    }
}
```

### 3.4 Function`<T, R>`接口

- java.util.function.Function`<T,R>`接口用来根据一个类型的数据，得到另一个类型的数据。前者称为前置条件，后者称为后置条件。
- Function接口中最主要的抽象方法为：`R apply(T t)`，根据类型T的参数获取类型R的结果。
- 使用场景例如：将String类型转换为Integer类型。

> 注意事项：
> Function的前置条件泛型和后置条件泛型可以相同。

- 示例：

```java
import java.util.function.Function;
public class DemoFunction {
    /*
        定义一个方法，
        方法的参数传递一个字符串类型的整数
        方法的参数传递一个Function<String, Integer>类型的接口
        使用Function接口中的方法apply，把字符串类型的整数转换为Integer类型

     */
    public static Integer change(String s, Function<String, Integer> fun) {
        Integer in = fun.apply(s);
        // int in = fun.apply(S); // 自动拆箱 Integer --> int
        return in;
    }

    public static void main(String[] args) {
       int in =  change("123", (String s) -> {
           return Integer.parseInt(s);
        });
        System.out.println(in + 1);
    }
}
```

#### 默认方法：andThen

- Function接口中的默认方法`andThen`：用来进行组合操作。


- 示例

```java
import java.util.function.Function;

/*
    需求：
        把String类型的"123"转换为Integer类型，再加上10
        把Integer类型转换为String类型
    分析：
        转换了两次
        1. String --> Integer
        Function<String, Integer> fun1
        Integer in = fun1.apply("123") + 10;
        2. Integer --> String
        Function<Integer, String> fun2
        String s = fun2.apply(in);
    使用andThen方法，把两次转换组合在一起使用
 */
public class Function_AndThen {
    public static void change(String s, Function<String, Integer> fun1, Function<Integer, String> fun2) {
        String str = fun1.andThen(fun2).apply(s);
        System.out.println(str);

    }

    public static void main(String[] args) {
/*
        change("123", (String s) -> {
            // String --> Integer
             return Integer.parseInt(s) + 10;
        }, (Integer in) -> {
            // Integer --> String
             return in + " ";
        });
*/
        // 优化Lambda表达式
        change("1234", s -> Integer.parseInt(s) + 100, i -> i + " ");
    }

}
```

#### 练习：自定义函数模型的拼接

```java
import java.util.function.Function;

/*
题目:
    String str = "赵丽颖,20";
    1.将字符串截取数字年龄部分，得到字符串
    2.将上一步的字符串转换为int类型的数字
    3.将上一步的int数字累加100，得到结果int数字

分析：
    1."赵丽颖,20" -> "20"
    2."20" -> 20
    3. 20 + 100 -> 120
 */
public class DemoFunctionTest {
    /*
        定义一个方法，参数传递字符串和三个Function接口
        Function<String, String>fun1
        Function<String, Integer>fun2
        Function<Integer, Integer>fun3
     */
    public static int change(String s, Function<String, String> fun1,
                             Function<String, Integer> fun2, Function<Integer, Integer> fun3) {
        return fun1.andThen(fun2).andThen(fun3).apply(s);
    }

    public static void main(String[] args) {
        String str = "赵丽颖,20";

        int num = change(str, (String s) -> {
            return s.split(",")[1];
        }, (String s) -> {
            return Integer.parseInt(s);
        }, (Integer in) -> {
            return in + 100;
        });
        System.out.println(num);
    }
}
```