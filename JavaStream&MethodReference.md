[TOC]

# Java Stream & Method Reference

## 1. Stream流

### 1.1 概述

#### 传统集合的多步遍历代码

```java
/*
    传统的方式遍历集合,过滤集合,打印输出满足条件的集合
 */
import java.util.ArrayList;
public class Demo01LIst {
    public static void main(String[] args) {
        // 创建list集合，存储姓名
        ArrayList<String> list = new ArrayList<>();
        list.add("张无忌");
        list.add("赵敏");
        list.add("周芷若");
        list.add("张强");
        list.add("张三丰");

        ArrayList<String> listA = new ArrayList<>();
        for (String s : list) {
            // 对list集合过滤，将张字开头的添加到listA集合中
            if (s.startsWith("张")) {
                listA.add(s);
            }
        }

        ArrayList<String> listB = new ArrayList<>();
        for (String s : listA) {
            // 对listA集合过滤，只要名字长度为3的
            if (s.length() == 3) {
                listB.add(s);
            }
        }

        // 遍历listB集合，打印输出
        for (String s : listB) {
            System.out.println(s);
        }
    }
}
```

#### Stream的更优写法

```java
import java.util.ArrayList;
/*
    使用Stream流的方式，遍历集合，对集合中的数据进行过滤
    Stream流式JDK1.8之后出现的
    关注的是做什么，而不是怎么做
 */
public class Demo02Stream {
    public static void main(String[] args) {
        // 创建list集合，存储姓名
        ArrayList<String> list = new ArrayList<>();
        list.add("张无忌");
        list.add("赵敏");
        list.add("周芷若");
        list.add("张强");
        list.add("张三丰");

        // 对list集合过滤，将张字开头的添加到listA集合中
        // 对listA集合过滤，只要名字长度为3的
        // 遍历listB集合，打印输出

        list.stream()
                .filter(name -> name.startsWith("张"))
                .filter(name -> name.length() == 3)
                .forEach(name -> System.out.println(name));

    }
}
```

### 1.2 流式思想的概述

**注意：请暂时忘记对传统IO流固有的印象！**

`filter` 、 `map` 、 `skip` 都是在对函数模型进行操作，集合元素并没有真正被处理。只有当终结方法 `count` 执行的时候，整个模型才会按照指定策略执行操作。而这得益于Lambda的延迟执行特性。

> 备注：“Stream流”其实是一个集合元素的函数模型，它并不是集合，也不是数据结构，其本身并不存储任何 元素（或其地址值）

Stream（流）是一个来自数据源的元素队列

- 元素是特定类型的对象，形成一个队列。 Java中的Stream并不会存储元素，而是按需计算。
- **数据源** 流的来源。 可以是集合，数组等。

和以前的Collection操作不同， Stream操作还有两个基础的特征：

- **Pipelining**: 中间操作都会返回流对象本身。 这样多个操作可以串联成一个管道， 如同流式风格（ﬂuent style）。 这样做可以对操作进行优化， 比如延迟执行(laziness)和短路( short-circuiting)。
- **内部迭代**： 以前对集合遍历都是通过Iterator或者增强for的方式, 显式的在集合外部进行迭代， 这叫做外部迭代。 Stream提供了内部迭代的方式，流可以直接调用遍历方法。

当使用一个流的时候，通常包括三个基本步骤：获取一个数据源（source）→ 数据转换→执行操作获取想要的结果，每次转换原有 Stream 对象不改变，返回一个新的 Stream 对象（可以有多次转换），这就允许对其操作可以像链条一样排列，变成一个管道。 


### 1.3 获取流

- java.util.stream.Stream`<T>`是 Java 8 新加入的最常用的流接口。（这并不是一个函数式接口。）

**获取一个流非常简单，有以下几种常用的方式：**

- 所有的`Collection`集合都可以通过`stream`默认方法获取流。
    - default Stream`<E>` stream()
- Stream接口的静态方法 of，可以获取数组对应的流。 
    - static `<T>` Stream`<T>` of(T... values) 
    - 参数是一个可变参数，那么我们就可以传递一个数组。

- 示例:

```java
/*
获取stream流的两种方式
1. 所有Collection集合都可以通过默认方法 stream获取流
2. 可以使用Stream接口中的静态方法，of，将数组转换成流对象
 */
import java.util.*;
import java.util.stream.Stream;

public class Demo03GetStream {
    public static void main(String[] args) {
        // 把集合转换为Stream流
        List<String> list = new ArrayList<>();
        Stream<String> stream1 = list.stream();

        Set<String> set = new HashSet<>();
        Stream<String> stream2 = set.stream();

        Map<String, String> map = new HashMap<>();
        // 获取键，存储到set集合中
        Set<String> keySet = map.keySet();
        Stream<String> stream3 = keySet.stream();
        // 获取值，存储到Collection集合
        Collection<String> values = map.values();
        Stream<String> stream4 = values.stream();

        // 获取键值对(键与值的映射关系 entrySet)
        Set<Map.Entry<String, String>> entries = map.entrySet();
        Stream<Map.Entry<String, String>> stream5 = entries.stream();

        // 把数组转换为Stream流
        Stream<Integer> stream6 = Stream.of(1, 2, 3, 4, 5);

        // 可变参数可以传递数组
        Integer[] arr = {1, 3, 4, 5};
        Stream<Integer> stream7 = Stream.of(arr);

        String[] arr2 = {"a", "b", "c", "d"};
        Stream<String> stream8 = Stream.of(arr2);



    }
}
```

### 1.4 常用方法


流模型的操作很丰富，这些方法可以被分成两种：

- **延迟方法**：返回值类型仍然是 `Stream`接口自身类型的方法，因此支持链式调用。（除终结方法外，其余方法均为延迟方法。）
- **终结方法**：返回值类型不再是 `Stream`接口自身类型的方法，因此不再支持类似 `StringBuilder`那样的链式调用。本小节中，终结方法包括 `count` 和 `forEach`方法。


#### ①逐一处理：forEach

虽然名字叫 forEach，但是与for循环中的 for-each不同。

- void forEach(Consumer<? super T> action);
-  该方法接收一个 Consumer接口函数，会将每一个流元素交给该函数进行处理。
-  Consumer 接口是一个消费型的函数式接口，可以传递 Lambda表达式，消费数据。

**简单记：**
	forEach方法，用来遍历流中的数据，是一个终结方法，遍历之后就不能继续调用Stream流中的其他方法。
	
- 示例：

```java
import java.util.stream.Stream;

public class Demo04ForEach {
    public static void main(String[] args) {
        // 获取一个Stream流
        Stream<String> stream = Stream.of("迪丽热巴", "古力娜扎", "马儿扎哈");
        // 使用forEach，参数传递Lambda表达式
        /*stream.forEach((String name) -> {
            System.out.println(name);
        });*/

        // 优化Lambda表达式
        stream.forEach(name -> System.out.println(name));
    }
}

/*
print result：
迪丽热巴
古力娜扎
马儿扎哈
*/
```

#### ②过滤：filter

- filter：用于对 Stream流中的数据进行过滤。
- Stream<T> filter(Predicate<? super T> predicate) ;
- filter方法的参数 Predicate是一个函数式接口，所以可以传递 Lambda表达式，对数据进行过滤。
- Predicate中的抽象方法：
    - `boolean test(T t);`

- 示例:

```java
import java.util.stream.Stream;
// Stream<T> filter(Predicate<? super T> predicate);
public class Demo05Filter {
    public static void main(String[] args) {
        // 获取Stream流
        Stream<String> stream = Stream.of("张三丰", "张三疯", "周芷若", "赵敏", "张翠山");
        // 使用 filter方法进行过滤
        // stream.filter((String name) -> {name.startsWith("张");});
        // 使用 filter之后，返回一个新的流
        Stream<String> newStream = stream.filter(name -> name.startsWith("张"));
        // 逐一输出
        newStream.forEach(name -> System.out.println(name));
        /*
            Stream流属于管道流，只能被消费(使用)一次
            第一个Stream流调用完毕，数据就会流到下一个Stream上
            而这时第一个Stream流已经使用完毕，就会关闭了
            所以第一个Stream流就不能再调用方法了
 IllegalStateException: stream has already been operated upon or closed
         */
        // 已经被使用一次了，再次使用会抛出异常
        // stream.forEach(name -> System.out.println(name));
    }
}

```

#### ③映射：map

- 如果需要将流中的元素映射到另一个流中，可以使用 map方法。
- `<R>` Stream`<R>` map(Function<? super T,? extends R> mapper) ;
- 该接口需要一个 Function函数式接口参数，可以将当前流中的 T类型数据转换为另一种 R类型的流。
- Function中的抽象方法：
    - `R apply(T t);`
- 示例：

```java
import java.util.stream.Stream;

public class Demo06Map {
    public static void main(String[] args) {
        // 获取一个String类的Stream流
        Stream<String> stream = Stream.of("1", "2", "3", "4");
        // 将String类型的整数，转换(映射)为 Integer类型的整数
        Stream<Integer> stream2 = stream.map((String s) -> {
            return Integer.parseInt(s);
        });
        // 遍历 stream2流
        stream2.forEach(num -> System.out.println(num));
    }
}
```

#### ④统计个数：count

  正如旧集合 `Collection`当中的 `size`方法一样，`Stream`提供 `count`方法来获取其中元素的个数。

- count：用于统计Stream流中元素的个数。
- `long count();`
- count方法是一个终结方法，返回值是一个 **long**类型的整数，所以不能再调用 Stream流中的其他方法了。

- 示例：

```java
import java.util.ArrayList;
import java.util.stream.Stream;

public class Demo07Count {
    public static void main(String[] args) {
        ArrayList<Integer> list = new ArrayList<>();
        list.add(1);
        list.add(2);
        list.add(3);
        list.add(4);
        list.add(5);

        Stream<Integer> stream = list.stream();
        long count = stream.count();
        System.out.println(count);
    }
}
```

#### ⑤取用前几个：limit

- limit：用于截取流中的元素。
- limit方法可以对流进行截取，只取用前 n 个。
- `Stream<T> limit(long maxSize);`
    - 参数是一个 **long**类型，如果集合当前长度大于参数，则进行截取；**否则不进行操作。**
- limit方法是一个 **延迟方法**，只是对流中的元素进行截取，返回的是一个新的流，所以可以继续调用 Stream流中的其他方法。

- 示例：

```java
import java.util.Arrays;
import java.util.stream.Stream;

public class Demo08Limit {
    public static void main(String[] args) {
        String[] arr = {"喜羊羊", "美羊羊", "暖羊羊", "灰太狼", "红太狼"};
        Stream<String> stream = Arrays.stream(arr);
        Stream<String> stream2 = stream.limit(3); // 截取前3个元素，并返回新的Stream流
        stream2.forEach(name -> System.out.println(name));
    }
}
```



#### ⑥跳过前几个：skip

- skip：用于跳过元素；
- 如果希望跳过前几个元素，可以使用 skip方法获取一个截取之后的新流。
- Stream`<T>` skip(long n);
    - 如果流的长度大于 n，则跳过前 n个；**否则将会得到一个长度为 0 的空流。**

```java
import java.util.stream.Stream;

public class Demo09Skip {
    public static void main(String[] args) {
        String[] arr = {"喜羊羊", "美羊羊", "暖羊羊", "灰太狼", "红太狼"};
        Stream<String> stream = Stream.of(arr);
        // 跳过前3个
        Stream<String> stream2 = stream.skip(3);
        stream2.forEach(name -> System.out.println(name));
    }
}
```

#### ⑦组合：concat

- concat：用于把流组合到一起。
- 如果有两个流，希望合并成一个流，那么可以使用 `Stream` 接口中的静态方法 `concat`
- public static <T> Stream<T> concat(Stream<? extends T> a, Stream<? extends T> b)
- 示例：

```java
import java.util.stream.Stream;

/*
public static <T> Stream<T> concat(Stream<? extends T> a, Stream<? extends T> b)
 */
public class Demo10Concat {
    public static void main(String[] args) {
        String[] arr = {"喜羊羊", "美羊羊", "暖羊羊", "灰太狼", "红太狼"};
        Stream<String> stream1 = Stream.of(arr);
        Stream<String> stream2 = Stream.of("张三丰", "张三疯", "周芷若", "赵敏", "张翠山");
        // 把以上两个流组合成为一个流
        Stream<String> concat = Stream.concat(stream1, stream2);
        // 遍历输出
        concat.forEach(name -> System.out.println(name));

    }

}
```

### 1.5 练习：集合元素处理(传统方式)

```java
	import java.util.ArrayList;

/*
题目: 
现在有两个 ArrayList 集合存储队伍当中的多个成员姓名，要求使用传统的for循环（或增强for循环）依次进行以下若干操作步骤： 
    练习:集合元素处理（传统方式）
        现在有两个ArrayList集合存储队伍当中的多个成员姓名，要求使用传统的for循环（或增强for循环）依次进行以下若干操作步骤：
        1. 第一个队伍只要名字为3个字的成员姓名；存储到一个新集合中。
        2. 第一个队伍筛选之后只要前3个人；存储到一个新集合中。
        3. 第二个队伍只要姓张的成员姓名；存储到一个新集合中。
        4. 第二个队伍筛选之后不要前2个人；存储到一个新集合中。
        5. 将两个队伍合并为一个队伍；存储到一个新集合中。
        6. 根据姓名创建Person对象；存储到一个新集合中。
        7. 打印整个队伍的Person对象信息。
 */
public class DemoStreamTest {
    public static void main(String[] args) {
        //第一支队伍
        ArrayList<String> one = new ArrayList<>();
        one.add("迪丽热巴");
        one.add("宋远桥");
        one.add("苏星河");
        one.add("石破天");
        one.add("石中玉");
        one.add("老子");
        one.add("庄子");
        one.add("洪七公");
        // 1. 第一个队伍只要名字为3个字的成员姓名；存储到一个新集合中。
        ArrayList<String> oneA = new ArrayList<>();
        for (String name : one) {
            if (name.length() == 3) {
                oneA.add(name);
            }
        }
        // 2. 第一个队伍筛选之后只要前3个人；存储到一个新集合中。
        ArrayList<String> oneB = new ArrayList<>();
        for (int i = 0; i < 3; i++) {
            oneB.add(oneA.get(i));
        }
        //第二支队伍
        ArrayList<String> two = new ArrayList<>();
        two.add("古力娜扎");
        two.add("张无忌");
        two.add("赵丽颖");
        two.add("张三丰");
        two.add("尼古拉斯赵四");
        two.add("张天爱");
        two.add("张二狗");
        // 3. 第二个队伍只要姓张的成员姓名；存储到一个新集合中。
        ArrayList<String> twoA = new ArrayList<>();
        for (String name : two) {
            if (name.startsWith("张")) {
                twoA.add(name);
            }
        }
        // 4. 第二个队伍筛选之后不要前2个人；存储到一个新集合中。
        ArrayList<String> twoB = new ArrayList<>();
        for (int i = 2; i < twoA.size(); i++) {
            twoB.add(twoA.get(i));
        }
        // 5. 将两个队伍合并为一个队伍；存储到一个新集合中。
        ArrayList<String> all = new ArrayList<>();
        all.addAll(twoB);
        all.addAll(oneB);
        // 6. 根据姓名创建Person对象；存储到一个新集合中。
        ArrayList<Person> list = new ArrayList<>();
        for (String s : all) {
            list.add(new Person(s));
        }
        for (Person person : list) {
            System.out.println(person);
        }
    }
}
```


### 1.6 练习：集合元素处理(Stream流方式)

```java
import java.util.ArrayList;
import java.util.stream.Stream;

/*
题目: 
现在有两个 ArrayList 集合存储队伍当中的多个成员姓名，要求使用传统的for循环（或增强for循环）依次进行以下若干操作步骤： 
    练习:集合元素处理（传统方式）
        现在有两个ArrayList集合存储队伍当中的多个成员姓名，要求使用传统的for循环（或增强for循环）依次进行以下若干操作步骤：
        1. 第一个队伍只要名字为3个字的成员姓名；存储到一个新集合中。
        2. 第一个队伍筛选之后只要前3个人；存储到一个新集合中。
        3. 第二个队伍只要姓张的成员姓名；存储到一个新集合中。
        4. 第二个队伍筛选之后不要前2个人；存储到一个新集合中。
        5. 将两个队伍合并为一个队伍；存储到一个新集合中。
        6. 根据姓名创建Person对象；存储到一个新集合中。
        7. 打印整个队伍的Person对象信息。
 */
public class DemoStreamTest2 {
    public static void main(String[] args) {
        //第一支队伍
        ArrayList<String> one = new ArrayList<>();
        one.add("迪丽热巴");
        one.add("宋远桥");
        one.add("苏星河");
        one.add("石破天");
        one.add("石中玉");
        one.add("老子");
        one.add("庄子");
        one.add("洪七公");
        // 1. 第一个队伍只要名字为3个字的成员姓名；存储到一个新集合中。
        // 2. 第一个队伍筛选之后只要前3个人；存储到一个新集合中。
        Stream<String> oneStream = one.stream().filter(name -> name.length() == 3).limit(3);
        //第二支队伍
        ArrayList<String> two = new ArrayList<>();
        two.add("古力娜扎");
        two.add("张无忌");
        two.add("赵丽颖");
        two.add("张三丰");
        two.add("尼古拉斯赵四");
        two.add("张天爱");
        two.add("张二狗");
        // 3. 第二个队伍只要姓张的成员姓名；存储到一个新集合中。
        // 4. 第二个队伍筛选之后不要前2个人；存储到一个新集合中。
        Stream<String> twoStream = two.stream().filter(name -> name.startsWith("张")).skip(2);
        // 5. 将两个队伍合并为一个队伍；存储到一个新集合中。
        // 6. 根据姓名创建Person对象；存储到一个新集合中。
        // 7. 打印整个队伍的Person对象信息。
        Stream.concat(oneStream, twoStream).map(name -> new Person(name)).forEach(name-> System.out.println(name));
    }
}
```

## 2. 方法引用

### 2.1 基本介绍

  双冒号`::`为引用运算符，而它所在的表达式被称为**方法引用**。如果Lambda要表达的函数方案已经存在于某个方法的实现中，那么则可以通过双冒号来引用该方法作为Lambda的替代者。

- Printable接口

```java
/*
    定义一个打印的函数式接口
 */
@FunctionalInterface
public interface Printable {
    // 对字符串的抽象方法
    public abstract void print(String s);
}
```

- Demo01Printable.java

```java
public class Demo01Printable {
    // 定义一个方法，参数传递Printable接口，对字符串进行打印
    public static void printString(Printable p) {
        p.print("HelloWorld");
    }

    public static void main(String[] args) {
        printString((s) -> {
            System.out.println(s);
        });
    /*
        分析：
            Lambda表达式的目的：打印参数传递的字符串
            把参数 s 传递给 System.out对象，调用 out对象中的方法println对字符串输出
            使用前提(注意):
                1.System.out对象已经存在。
                2.println方法也是存在的。
            所以我们可以直接使用方法引用来优化 Lambda表达式
            可以使用System.out方法直接引用(调用)println方法
     */
        printString(System.out::println);
    }
}
```

**语义分析**

例如上例中，`System.out`对象中已经有了一个重载`print(String x)`方法恰好就是我们需要的，那么对于`printString`方法的函数式接口参数，对比下面的两种写法，完全等效：

1. Lambda写法：s -> System.out.println(s);
2. 方法引用写法：System.out::println();

- 第一个种语义是指：拿到参数之后经Lambda之手，继而传递给`System.out.println()`方法去处理。
- 第二种等效的语义是指：直接让`System.out`中的`println`方法来取代Lambda。两种写法的执行效果完全一样，而第二种方法引用复用已有的方案，更加简洁。

> 注意事项：
> Lambda中传递的参数一定式方法引用中的方法可以接收的类型，否则会抛出异常。
> 

### 2.2 通过对象名引用【成员方法】

- Printable接口

```java
/*
    定义一个打印的函数式接口
 */
@FunctionalInterface
public interface Printable {
    // 对字符串的抽象方法
    public abstract void print(String s);
}
```

- MethodRefObject.java

```java
public class MethodRefObject {
    // 定义一个成员方法，参数传递字符串，将字符串大写输出
    public void printUpperCaseString(String s) {
        System.out.println(s.toUpperCase());
    }
}
```
- DemoObjectMethodReference.java

```java
public class DemoObjectMethodReference {
    // 定义一个方法，参数传递Printable接口
    public static void printString(Printable p) {
        p.print("Hello Java");
    }

    public static void main(String[] args) {
        printString((s) -> {
            MethodRefObject obj = new MethodRefObject();
            obj.printUpperCaseString(s);
        });
        /*
           使用方法引用对Lambda进行优化
           使用前提：对象和成员方法已经存在
         */
        // 创建对象
        MethodRefObject obj = new MethodRefObject();
        printString(obj::printUpperCaseString);
    }
}
```

### 2.3 通过类名称引用【静态方法】


- Calcable接口

```java
public interface Calcable {
    // 定义一个抽象方法，参数传递一个整数，进行取模运算
    public int methodAbs(int num);
}
```


- DemoStaticMethodRef.java

```java
public class DemoStaticMethodRef {
    // 定义一个方法，参数传递一个int型整数,一个Calcalbe接口
    public static int method(int num, Calcable c) {
        return c.methodAbs(num);
    }

    public static void main(String[] args) {
        // 调用method方法，传递一个整数，取模运算
        int num = method(-10, (n) -> {
            return Math.abs(n);
        });
        System.out.println(num);
        /*
        通过类名称引用静态成员方法的前提:
            1. 类存在
            2. 静态成员方法存在且已经实现了
         */
        int num2 = method(-100, Math::abs);
        System.out.println(num2);
    }
}
```

### 2.4 通过super引用父类的普通成员方法

- Greetable接口

```java
// 定义一个问候的接口
public interface Greetable {
   public abstract void greet();
}
```

- Fu.java

```java
public class Fu {
    // 定义一个sayHello方法
    public void sayHello() {
        System.out.println("我是师父");
    }

}
```

- Zi.java

```java
/*
通过super引用父类的普通成员方法
 */
public class Zi extends Fu {
    // 覆盖重写父类的 sayHello方法
    @Override
    public void sayHello() {
        System.out.println("我是徒弟");
    }

    // 定义一个方法，参数传递Greetable接口
    public void method(Greetable g) {
        g.greet();

    }

    // 展示输出的方法,参数传递一个Greetable接口
    public void show() {
        /*method(() -> {
            Fu fu = new Fu();
            fu.sayHello();
        });*/
        // method(() -> super.sayHello());
        method(super::sayHello);
    }
}
```

- main方法

```java
public class DemoMain {
    public static void main(String[] args) {
        new Zi().show();
    }
}
```

### 2.5 通过this引用本类的普通成员方法

- Richable接口

```java
public interface Richable {
    // 定义一个买东西的方法
    public abstract void buy();
}
```

- Husband.java

```java
public class Husband {
    // 定义买房子的方法
    public void buyHouse() {
        System.out.println("杭州一套小楼!");
    }

    // 定义结婚的方法, 参数传递Richable接口
    public void marry(Richable r) {
        r.buy();
    }

    // 高兴方法，因为要结婚
    public void soHappy() {
        //  this.marry(() -> {this.buyHouse();});
        /*
            this和调用的本类成员方法已经存在且实现
            可以使用this引用本类的成员方法
         */
        marry(this::buyHouse);
    }

    public static void main(String[] args) {
        new Husband().soHappy();
    }
}
```


### 2.6 类的构造器(构造方法)引用

- PersonBuilder接口

```java
@FunctionalInterface
public interface PersonBuilder {
    // 定义方法，用来根据传递的姓名，创建Person对象，并返回
    public abstract Person builderPerson(String name);
}
```

- DemoPerson.java

```java
public class DemoPerson {
    // 定义一个方法，参数传递姓名和PersonBuilder接口，打印对象姓名
    public static void printName(String name, PersonBuilder pb) {
        Person person = pb.builderPerson(name);
        System.out.println(person);
    }

    public static void main(String[] args) {
        // printName("迪丽热巴", (s) -> new Person(s));
        /*
            使用方法引用优化Lambda表达式
            构造方法：new Person(String name);已知
            创建对象：new 已知
            就可以使用Person引用new创建对象
         */
        printName("周元", Person::new); // 使用Person类的带参构造方法，创建对象
    }
}
```

### 2.7 数组的构造器引用

- ArrayBuilder接口

```java
/*
    数组创建接口
 */
@FunctionalInterface
public interface ArrayBuilder {
    // 根据参数传递的长度，创建int型指定长度的数组
    public abstract int[] builderArray(int length);
}
```

- DemoArray.java

```java
import java.util.Arrays;

public class DemoArray {
    /*
        定义一个方法，参数传递要创建数组的长度和ArrayBuilder接口
        方法内部传递的长度使用ArrayBuilder中的方法创建数组并返回
     */
    public static int[] creatArray(int length, ArrayBuilder ab) {
        return ab.builderArray(length);
    }

    public static void main(String[] args) {
        int[] arrA = creatArray(10, (len) -> new int[len]);
        System.out.println(Arrays.toString(arrA));
        System.out.println(arrA.length);
        /*
            创建的是 int[] 类型的数组
            数组的长度已知
            使用 int[]引用new，根据传递的长度创建数组
         */
        int[] arrB = creatArray(10, int[]::new);
        System.out.println(arrB.length);
    }
}
```