[TOC]

# Java 集合

## 1. Collection集合（单列集合）

### 集合概述

+ 集合：集合是Java提供的一种容器，可以用来存储多个数据。
+ 数组的长度是固定的，而集合的长度是可变的。
+ 数组中存储的是同一类型的元素，可以存储基本类型的值。***集合存储的都是对象，是引用类型***，而且对象的类型可以不一样。在开发中一般当对象多的时候，使用集合进行存储。

### 集合的架构



![集合的架构](https://ae01.alicdn.com/kf/H4395eaf3c6b94da4a4a966a98bce5ebbu.jpg)


### Collection常用的方法

Collection是所有单列集合的父接口，因此在Collection中定义了单列集合（list和set）通用的一些方法，这些方法可用于操作所有的单列集合，共性方法如下：

+ public boolean add(E e)：把给定的对象添加到当前集合当中。
+ public void clear()：清空集合当中所有的元素。
+ public boolean remove(E e)：把给定的对象从当前集合当中删除。
+ public boolean contains (E e)：判断当前集合中是否包含给定的对象。
+ public boolean isEmpty(E e)：判断当前集合是否为空。
+ public int size()：返回集合中元素的个数。
+ public Object[] toArray()：把集合中的元素，存储到数组中。

```java
import java.util.ArrayList;
import java.util.Collection;
/*
public boolean add(E e);
public void clear();
public boolean remove(E e);
public int size();
public boolean isEmpty();
public Object[] toArray();
public boolean contains(E e);
 */

public class DemoCollection {
    public static void main(String[] args) {
        Collection<String> coll = new ArrayList<>();
        System.out.println(coll);

        // add 添加元素
        coll.add("迪丽热巴");
        coll.add("古力娜扎");
        coll.add("马尔扎哈");
        coll.add("鹿晗");
        coll.add("赵丽颖");

        System.out.println(coll);

        // isEmpty 判断是否为空
        System.out.println(coll.isEmpty());

        // size 当前集合的大小
        System.out.println(coll.size());

        // remove 清除一个元素
        coll.remove("马尔扎哈");

        // Object[] toArray
        Object[] objects = coll.toArray();
        for (int i = 0; i < objects.length; i++) {
            System.out.println(objects[i]);
        }
        
        // contains 判断当前集合是否包含给定的对象
        boolean b1 = coll.contains("赵丽颖");
        System.out.println(b1);

        // clear 清空集合中的元素，但集合仍然存在，只不过为空
        coll.clear();
        System.out.println(coll.isEmpty());
    }
}
```


### Iterator

+ 迭代：即Collection集合元素的通用获取方式。在取元素之前先判断集合中有没有元素，如果有，就把这个元素取出来，再继续判断，如果还有就再取出来。一直把集合中所有元素取出。这种取出的方式专业术语称为迭代。
+ java.util.Iterator接口：迭代器（对集合进行遍历）

有两个常用的方法

1. public boolean hasNext()：判断集合当中还有没有下一个元素，有就返回true，没有就返回false。
2. public E next()：取出集合当中的下一个元素。

Iterator迭代器，是一个接口，我们无法直接使用，需要使用Iterator接口的实现类对象，获得实现类的方式比较特殊，***Collection接口***中有一个方法，叫***iterator()***，这个方法返回的就是迭代器的实现对象。

+ Iterator`<E>`iterator()：返回在此 Collection 的元素上进行迭代的迭代器。


Iterator迭代器的使用步骤：

1. 使用集合当中的方法***iterator()***获取迭代器的实现类对象，使用Iterator接口接受（多态）。
2. 使用Iterator接口当中的方法***hasNext()***判断还有没有下一个元素。
3. 使用Iterator接口当中的方法***next()***取出集合当中的下一个元素。

> 注意事项：
> Iterator`<E>`接口也是有泛型的，迭代器的泛型跟着集合走，集合是什么泛型，迭代器就是什么泛型。
> 


迭代器的实现原理

```java
public class DemoCollection {
    public static void main(String[] args) {
        Collection<String> coll = new ArrayList<>();

        coll.add("迪丽热巴");
        coll.add("马儿扎哈");
        coll.add("赵丽颖");
        coll.add("鹿晗");
        
        Iterator<String> it = coll.iterator();

        while (it.hasNext()){
            String str = it.next();
            System.out.println(str);
        }
    }
}
```

+ `coll.iterator()`：获取集合的实现类对象，并且会把指针（索引）指向集合的**-1**索引。
+ `it.hasNext()`：判断集合还有没有下一个元素。
+ `it.next()`：做了两件事
    1. 取出下一个元素
    2. 会把指针向后移动一位


## 2. 增强for（for each）

+ 增强for循环（也称 for each循环）是**JDK 1.5**以后出来的一个高级for循环，专门用来遍历数组和集合的。它的内部原理其实是个**Iterator迭代器**，所以在遍历的过程中，不能对集合中的元素进行增删操作。
+ public interface Collection`<E>`extends Iterable`<E>`：所有的***单列*** 集合都可以使用增强for。
+ public interface Iterable`<T>`实现这个接口允许对象成为 "foreach" 语句的目标。 

+ 格式：

```java
for (元素的数据类型 变量名称 : Collection集合 or 数组){
		// 写操作代码
}
```

> 注意事项：
> 1. 不要在遍历的过程中对集合元素进行增删操作。
> 2. 增强 for循环必须有被遍历的对象，目标只能是Collection集合或者数组，新式for仅仅作为遍历操作出现。
> 


## 3. 泛型(Generic)

### 泛型概念

泛型是一种未知的数据类型，当我们不知道使用什么数据类型的时候，就可以使用泛型。
泛型也可以看作是一个变量，用来接受数据类型。

E e：Element 元素
T t：Type 类型

创建集合对象的时候，就会确定泛型的数据类型。

### 使用泛型的好处

创建集合对象，不使用泛型

+ 好处：集合不使用泛型，默认的类型就是Object类型，可以存储任意类型的数据。
+ 弊端：不安全，容易引发异常。

创建集合，使用泛型

+ 好处 ：
    1. 避免了类型转换的麻烦，存储的是什么类型，取出的就是什么类型。
    2. 把运行期异常（代码运行之后会抛出的异常），提升到了编译期（写代码的时候会报错）。

+ 弊端：
    泛型是什么类型，只能存储什么类型。

### 定义含有泛型的类

+ 格式：
```java
修饰符 class 类名<泛型>{
		// ...
}
```

+ 示例：

```java
public class GenericClass <E> {
    private E name;

    public void setName(E name){
        this.name = name;
    }

    public E getName(){
        return this.name;
    }
}
```

### 定义含有泛型的方法

泛型定义在方法的修饰符和返回值类型之间

+ 格式：

```java
修饰符 <泛型> 返回值类型 方法名(参数列表(使用泛型)){
		// ...
}
```

+ 示例：

```java
public class GenericMethod {
	// 定义泛型普通方法
    public <E> void methodNormal(E e){
        System.out.println(e);
    }
	// 定义泛型静态方法
    public static <E> void methodStatic(E e){
        System.out.println(e);
    }
}
```

### 定义含有泛型的接口

+ 格式：

```java
修饰符 interface 接口名称<泛型>{
		// ...
}
```

1. 第一种使用方式：定义接口的实现类，实现接口，指定接口的泛型。

```java
public interface Iterator`<E>`{
	E next();
}
```

Scanner类实现了Iterator接口，并指定接口的泛型为String，所以重写的next方法泛型默认就是String。

```java
public final class Scanner implements Iterator<String>{
	public String next();
}
```

2. 第二种使用方式：接口使用什么泛型，实现类就使用什么泛型，类跟着接口走，就相当于定义了一个含有泛型的类，创建对象的时候确定泛型的类型。


```java
public class ArrayList<E>implements List<E>{
	public boolean add(E e);
	public E get(int index);
}

public interface List<E> extends Collection<E> {
	boolean add(E e);
	E get(int index);
}
```

### 泛型的通配符

+ ？：代表任意的数据类型
+ 使用方式：不能创建对象使用，只能作为方法的参数使用。
+ ***不知道使用什么类型来接收的时候，此时可以使用 ？ ，？表示未知通配符***。
+ 但是一旦使用泛型的通配符后，只能使用Object类中的共性方法，集合中元素自身的方法无法使用。
+ 练习：

```java
/*
题目要求：
定义一个方法，能遍历所有类型的ArrayList集合
分析：
这时候我们不知道ArrayList集合使用什么数据类型，
可以使用泛型的通配符 ? 来接受数据类型
 */
public class DemoGeneric01 {
    public static void main(String[] args) {
        ArrayList<Integer> listA = new ArrayList<>();
        listA.add(10);
        listA.add(20);

        ArrayList<String> listB = new ArrayList<>();
        listB.add("马尔扎哈");
        listB.add("格利纳扎");

        printArray(listA);
        printArray(listB);
    }
    public static void printArray(ArrayList<?> list){
        Iterator<?> it = list.iterator();
        while (it.hasNext()){
            Object o = it.next();
            System.out.println(o);
        }
    }
}
```

> 注意事项：
> 泛型不存在继承关系。
> Collection`<Object>` list = new ArrayList`<String>`(); 这种是错误的！
> 

### 通配符的高级使用—泛型受限

泛型的上限：

+ 格式：类型名称 < ? extends 类 > 对象名称
+ 意义：只能接受该类本身及其子类


泛型的下限：

+ 格式：类型名称 < ? super 类 > 对象名称
+ 意义：只能接收该类本身及其父类


## 4. 综合案例：斗地主（单列集合）

```java
/*
题目：
使用单列集合实现斗地主
分析：
1. 准备牌
创建一个集合poker用来存储54张牌
准备两个数组，分别存储花色和编号
两个for循环放入集合poker当中
2. 洗牌
调用Collections工具类中的shuffle()方法随机打乱牌
3. 发牌
用四个集合分别存储玩家A、B、C和底牌hand
使用索引 i % 3来进行对玩家分牌，当i >= 51时，给底牌
4. 看牌
输出四个集合存储的牌
 */
import java.util.ArrayList;
import java.util.Collections;

public class PokerGame {
    public static void main(String[] args) {
        // 准备牌
        ArrayList<String> poker = new ArrayList<>();

        String[] colors = {"♥", "♠", "♣", "♦"};
        String[] numbers = {"2","A","K","Q","J","10","9",
                "8","7","6","5","4", "3"};

        poker.add("大王");
        poker.add("小王");

        for (String color : colors) {
            for (String number : numbers) {
                poker.add(color + number);
            }
        }
//        System.out.println(poker);
//        System.out.println(poker.size());

        // 洗牌
        Collections.shuffle(poker);

        // 准备存储玩家的牌
        ArrayList<String> playA = new ArrayList<>();
        ArrayList<String> playB = new ArrayList<>();
        ArrayList<String> playC = new ArrayList<>();
        ArrayList<String> hand = new ArrayList<>();


        // 发牌
        for (int i = 0; i < poker.size(); i++) {
            String o = poker.get(i);
            int j = i % 3;
            if (i >= 51){
               hand.add(o);
            }else if (j == 0){
                playA.add(o);
            }else if (j == 1){
                playB.add(o);
            }else if (j == 2){
                playC.add(o);
            }
        }

        // 看牌
        System.out.println("选手A：" + playA);
        System.out.println("选手B：" + playB);
        System.out.println("选手C：" + playC);
        System.out.println("底牌：" + hand);


    }
}
```

## 5. List集合

+ java.util.List 接口 extends Collection 接口

### List接口的特点：

1. 有序的集合：存储和取出元素的顺序是一样的。
2. 有索引：包含一些带索引的方法。
3. 允许存储重复的元素。

### List接口中带索引的方法（特有）：

1. public void add(int index, E element)：将指定的元素，添加到该集合的指定位置。
2. public E get(int index)：返回集合中指定位置的元素。
3. public E remove(int index)：移除列表中指定位置的元素，返回值是被移除的元素。
4. public E set(int index, E element)：用指定元素替换集合中指定位置的元素，返回值是被替换的元素。

> 注意事项：
> 1. 操作索引的时候，一定防止索引越界异常。
> 2. IndexOutOfBoundsException：索引越界异常，集合会报。
> 3. ArrayIndexOutOfBoundsException：数组索引越界异常。
> 4. StringIndexOutOfBoundsException：字符串索引越界异常。 	
> 


## 6. List的实现类

### 1. ArrayList集合

+ 底层是一个数组。
+ 查询快，增删慢。
+ List 接口的大小可变数组的实现。
+ 注意，此实现不是同步的。（多线程）

### 2. LinkedList集合

+ 底层是一个链表结构。
+ 查询慢，增删快。
+ List 接口的链接列表实现。
+ **注意，此实现不是同步的。（多线程）**
+ 里面包含了大量操作首尾元素的方法。

> 注意事项：使用 LinkedList特有的方法，不能使用多态，多态的弊端就是不能使用子类特有的方法。
> 

添加方法：

+ public void addFirst(E e)：将指定元素插入到列表的开头。
+ public void addLast(E e)：将指定元素插入到列表的结尾。
+ public void push(E e)：将元素推入此列表所表示的堆栈，此方法等效于addFirst。

得到方法：

+ public E getFirst()：返回此列表的第一个元素。
+ public E getLast()：返回此列表的最后一个元素。

删除方法：

+ public E removeFirst()：移除并返回此列表的第一个元素。
+ public E removeLast()：移除并返回此列表的最后一个元素。
+ public E pop()：从此列表所表示的堆栈处弹出一个元素。此方法等效于removeFirst。

判断列表是否为空：

+ public boolean isEmpty()：如果此列表不包含元素，则返回true。


### 3. Vector集合

+ Vector 类可以实现可增长的对象数组。
+ 与新 collection 实现不同，Vector 是同步的。


## 7. Set集合

+ java.util.Set接口 extends Collection接口

Set接口的特点：

1. 不允许存储重复的元素。
2. 没有索引，没有带索引的方法，也不能使用普通的for循环遍历。

+ java.util.HashSet集合 implements Set接口

HashSet集合的特点：

3. 是一个无序集合，存储元素和取出元素的顺序有可能不一样。
4. 底层是一个哈希表结构（查询的速度非常的快）

```java
public class DemoSet {
    public static void main(String[] args) {
        Set<Integer> set = new HashSet<>();

        set.add(10);
        set.add(20);
        set.add(30);
        set.add(20); 
        System.out.println(set); // [20, 10, 30]

        Iterator<Integer> it = set.iterator();
        while (it.hasNext()){
            Integer next = it.next();
            System.out.println(next);
        }
        System.out.println("============");
        for (Integer i : set) {
            System.out.println(i);
        }
    }
}
```

### 哈希值

+ 哈希值：是一个十进制的整数，由系统随机给出（就是对象的地址值，是一个**逻辑地址**，是模拟出来得到的地址，不是数据实际存储的**物理地址**。
+ 在Object类中有个一个方法，可以获取对象的哈希值。
    int hashCode()：返回该对象的哈希码值。
    hashCode方法的源码：public native int hashCode();
    native：代表该方法调用的是本地操作系统的方法。
+ Object类中toString方法的源码：  return getClass().getName() + "@" + Integer.toHexString(hashCode());


```java
// 巧了，"通话"和"重地"的哈希码值一样
System.out.println("重地".hashCode()); // 1179395
System.out.println("通话".hashCode()); // 1179395
```

### HashSet存储数据的结构（哈希表）

+ 什么是哈希表呢？

在**JDK 1.8**之前，哈希表底层采用***数组 + 链表***实现，即使用链表处理冲突，同一hash值的链表都存储在一个链表里。但是当一个桶中的元素较多，即hash值相等的元素较多时，通过key值依次查找的效率较低。而JDK 1.8中，哈希表存储采用 ***数组 + 链表 + 红黑树***实现，当链表长度超过阈值（8）时，将链表***装换为红黑树***，这样大大减少了查找时间。

+ 简单来说，哈希表是由 数组 + 链表 + 红黑树（JDK 1.8增加的红黑树部分）实现的。


### Set集合存储不重复元素的原理

使用**add**方法添加元素的时候，**add**方法会调用元素的**hashCode**方法和**equals**方法，判断元素是否重复。

> 注意事项：
> 存储的元素必须重写**hashCode方法**和**equals方法**。
> 


### HashSet存储自定义类型的元素

+ 前提：需要重写类中的hashCode方法和equals方法。才能使集合中的元素唯一。
+ **注意，此实现不是同步的。**
+ 示例：

```java
 @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        // 使用反射技术判断 o 是否是Person类型 等效于 o instanceof Person
        if (o == null || getClass() != o.getClass()) return false;
        Person person = (Person) o;
        return age == person.age &&
                Objects.equals(name, person.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(name, age);
    }
    
// =============================

HashSet<Person> set = new HashSet<>();

        Person one = new Person("小美女", 19);
        Person two = new Person("小美女", 19);
        Person three = new Person("大美女", 20);

        System.out.println(one == two); // false

        System.out.println("one hashCoe:" + one.hashCode()); // one hashCoe:734175840
        System.out.println("two hashCode:" + two.hashCode()); // two hashCode:734175840

        set.add(one);
        set.add(three);
        set.add(two);
        System.out.println(set); // [Person{name='小美女', age=19}, Person{name='大美女', age=20}]
```

### LinkedHashSet集合

+ java.util.LinkedHashSet集合 extends HashSet集合
+ 特点：底层是一个哈希表（数组+链表 / 数组+红黑树）+链表，多了一条链表（记录元素的存储顺序），保证元素有序。
+ 以上特点JDK1.6中是这样说的：具有可预知迭代顺序的 Set 接口的哈希表和链接列表实现。
+ **注意，此实现不是同步的。**

### 可变参数

在**JDK 1.5**之后，如果我们定义一个方法需要接受多个参数，，并且参数的类型一致，我们可以使用可变参数。=

+ 格式：

```java
修饰符 返回值类型 函数名称(参数类型...参数名称){
		// 函数体
}
```

其实这个写法完全等价于

```java
修饰符 返回值类型 函数名称(参数类型[]参数名称){
		// 函数体
}
```

只是后面这种定义，在调用时必须传递数组，而前者可以直接传递数据即可。
**JDK 1.5**以后，出现了简化操作。**...**用在参数上，称之为可变参数。

> 注意事项：
> 1. 一个方法的参数列表，只能有一个可变参数。
> 2. 如果方法的参数有多个，那么可变参数必须写在参数列表的末尾。


## 8. Collections工具类

+ java.util.Collections是集合工具类。


1. public static `<T>` boolean addAll(Collection`<T>`c, T ... elements)：往集合当中添加一些元素。
2. public static void shuffle(List`<?>`, list)：打乱集合顺序。
3. public static `<T>` void sort(List`<T>` list)：将集合中的元素按照默认规则排序。


> 注意事项：sort(List`<T>` list)：使用前提：
> 被排序的集合里面元素，必须**实现Comparable接口**，重写接口中的方法：**compartTo**定义排序的规则。
> Comparable接口的排序规则：***自己（this）- 参数：升序*** 


4. public static `<T>` void sort(List`<T>` list, Comparator<? super T>)：将集合中的元素按照指定规则排序。

Comparable与Comparator的区别：
Comparable：自己（this）和别人（参数）比较，自己需要是实现Comparable接口，重写compareTo方法；
Comparator：相当于找一个第三方裁判，比较两个。
Comparator比较规则：o1 - o2 ：升序。

```java
Collections.sort(listB, new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
                return o1 - o2;
            }
        });
```

## 9. Map集合（双列集合）

+ `Collection`中的集合，元素是孤立存在的（理解为单身），向集合中存储元素采用一个一个元素的方式完成存储。
+ `Map`中的集合，元素是成对存在的（理解为夫妻），每个元素由键和值两部分组成，通过键可以找到对应的值。
+ `Collection`中的集合称为单列集合，`Map`中的集合称为双列集合。

### 概述
+ java.util.Map<k, v>集合

Map集合的特点：

1. Map集合是一个双列集合，一个元素包含两个值（一个key，一个value）。
2. Map集合中的元素，key和value的数据类型可以相同，也可以不同。
3. Map集合中的元素，key是不允许重复的，value是可以重复的。
4. Map集合中的元素，key和value是一一对应的。

### Map常用子类


HashMap集合的特点：

+ java.util.HashMap<k, v>集合 implements Map<k, v>接口

1. HashMap集合底层是***哈希表***：查询的速度特别快。
    + JDK 1.8之前：数组+链表
    + JDK 1.8之后：数组+链表 / 红黑树
2. HashMap集合是一个无序的集合，存储元素和取出元素的顺序有可能不一致。


LinkedHashMap集合的特点：

+ java.util.LinkedHashMap<k, v>集合 extends HashMap<k, v>集合

1. LinkedHashMap集合的底层是***哈希表+链表***（保证迭代的顺序）。
2. LinkedHashMap集合是一个有序的集合，存储元素和取出元素的顺序是一致的。

### Map接口中的常用方法

1. public V put(K key, V value)：把指定的键和指定的值添加到Map集合当中去。
	+ 返回值：V
	+ 存储键值对的时候，key不重复，返回值V是：null
	+ 存储键值对的时候，key重复，会使用新的value替换Map集合中重复的value，返回被替换的value值。
	
2. public V remove(Object key)：把指定的键，所对应的键值对元素，在Map集合中删除，返回被删除元素的值。
    + 返回值：V
    + key存在，V返回被删除的值
    + key不存在，V返回null
    
3. public V get(Object key)：根据指定的键，在Map集合中获取指定的值。
    + 返回值：V
    + key存在，返回对应的value值
    + key不存在，返回null

4. boolean containsKey(Object key)：判断集合中是否包含指定的键。
    + 包含返回：true
    + 不包含返回：false

5.  Set<K> keySet() ：返回此映射中包含的键的 Set 视图。 

#### 1. Map集合的第一种遍历方式：通过键找值的方式。

```java
import java.util.HashMap;
import java.util.Iterator;
import java.util.Map;
import java.util.Set;

/*
1. public V put(K key, V value)
2. public V remove(Object key)
3. public V get(Object key)
4. public boolean containsKey(Object key)
 */
public class DemoMap {
    public static void main(String[] args) {
        Map<String, String> map = new HashMap<>();

        // put(K key, V value)
        String put1 = map.put("萧炎", "彩鳞");
        map.put("林动", "欢欢");
        map.put("牧尘", "洛璃");
        
        // remove(Object key)
        String put2 = map.remove("牧尘");
        
        // containsKey(Object key)
        boolean b1 = map.containsKey("萧炎");
        
        // V get(Object key)
        String put4 = map.get("林动");

        // Map集合的第一种遍历方式：通过键找值
        Set<String> set = map.keySet();
        
        // 1. 使用迭代器
        Iterator<String> it = set.iterator();
        while (it.hasNext()){
            String key = it.next();
            String value = map.get(key);
            System.out.println(key + "=" + value);
        }
        System.out.println("=================");
        
        // 2. 使用增强for
        for (String key : map.keySet()) {
            String value = map.get(key);
            System.out.println(key + "=" + value);
        }
    }
}
```

#### 2. Map集合的第二种遍历方式：Entry对象。

static interface Map.Entry<K,V> ：映射项（键-值对）。 

Map.Entry<K,V>：在Map接口中有一个内部接口Entry。
作用：当Map集合一创建，就会在Map集合中创建一个Entry对象，用来记录键与值（键值对对象，键与值的映射关系） --> ***结婚证***

Map集合中的方法：
+ Set<Map<K, V>> entrySet()：返回此映射中包含的映射关系的 Set 视图。

使用步骤：

1. 使用Map集合当中的方法entrySet()，把Map集合当中的多个entry对象取出来，存放到一个Set集合中。
2. 遍历Set集合，获取每一个Entry对象。
3. 使用Entry对象中的方法，getKey()和getValue()获取键与值。


```java
import java.util.HashMap;
import java.util.Iterator;
import java.util.Map;
import java.util.Set;

public class DemoMapEntry {
    public static void main(String[] args) {
        Map<String, Integer> map = new HashMap<>();

        map.put("赵丽颖", 168);
        map.put("冯宝宝", 173);
        map.put("高圆圆", 178);
        map.put("林志玲", 180);

        System.out.println(map);
        // 1. 将每一个entry对象存放到set集合中
        Set<Map.Entry<String, Integer>> set = map.entrySet();
        // 2. 遍历entry对象，获取每一个entry对象
        // 首先获得set集合的迭代器
        Iterator<Map.Entry<String, Integer>> it = set.iterator();
        while (it.hasNext()) {
            Map.Entry<String, Integer> entry = it.next();
            // 3. getKey()和getValue()
            String key = entry.getKey();
            Integer value = entry.getValue();
            System.out.println(key+ "=" + value);
        }
        System.out.println("============");
        // 通过增强for实现
        for (Map.Entry<String, Integer> entry : set) {
            String key = entry.getKey();
            Integer value = entry.getValue();
            System.out.println(key+"="+value);
        }
    }
}
```


### HashMap存储自定义类型键值

Map集合保证key是唯一的，作为key的元素，必须重写hashCode和equals方法，以保证key是唯一的。

```java
import java.util.HashMap;
import java.util.Iterator;
import java.util.Map;
import java.util.Set;

public class DemoHashMapSavePerson {
    public static void main(String[] args) {
        // key：String
        // value：Person
        HashMap<String, Person> mapA = new HashMap<>();

        mapA.put("北京", new Person("聂离", 19));
        mapA.put("上海", new Person("陆飘", 19));
        mapA.put("广州", new Person("杜泽", 20));
        mapA.put("上海", new Person("叶紫芸", 18));

        // 使用keySet+增强for遍历
        Set<String> setA = mapA.keySet();
        for (String key : setA) {
            Person value = mapA.get(key);
            System.out.println(key + "-->" + value);
        }
        System.out.println("==================");
        
        // key：Person
        // value：String
        HashMap<Person, String> mapB = new HashMap<>();

        mapB.put(new Person("海波东",30), "冰皇");
        mapB.put(new Person("雅妃", 25), "金之女皇");
        mapB.put(new Person("萧炎", 20), "炎帝");
        mapB.put(new Person("古河", 30), "丹王");
        mapB.put(new Person("雅妃", 25), "女皇");

        // 使用entrySet+迭代器遍历
        Set<Map.Entry<Person, String>> setB = mapB.entrySet();
        // 获取迭代器
        Iterator<Map.Entry<Person, String>> it = setB.iterator();
        while (it.hasNext()) {
            Map.Entry<Person, String> entry = it.next();
            Person key = entry.getKey();
            String value = entry.getValue();
            System.out.println(key+"-->"+value);
        }
        System.out.println("====================");
        // 使用增强for + enteySet
        for (Map.Entry<Person, String> entry : setB) {
            Person key = entry.getKey();
            String value = entry.getValue();
            System.out.println(key+"-->"+value);
        }
    }
}
```

### LinkedHashMap集合

+ java.util.LinkedHashMap<K, V> extends HashMap<K, V>
+ Map 接口的哈希表和链接列表实现，具有可预知的迭代顺序。
+ 底层原理：哈希表 + 链表（记录元素的顺序）
+ **注意，此实现不是同步的。**

### Hashtable集合

+ java.util.Hashtable<K, V>集合 implements Map<K, V>接口。
+ ***任何非 null 对象都可以用作键或值。***
+ 不像新的 collection 实现，Hashtable 是同步的

Hashtable：底层也是一个哈希表，是一个线程安全的集合，是单线程集合，访问速度慢。
HashMap：底层是一个哈希表，是一个线程不安全的集合，是多线程集合，访问速度快。

HashMap集合（之前学的所有集合）：可以存储null值，null键
Hashtable集合：不可以存储null值，null键

Hashtable和Vector集合一样，在***JDK1.2***之后被更先进的集合（HashMap、ArrayList）替代。
但是，Hashtable的子类***Properties***依然在使用，***Properties集合是唯一一个和 IO流相结合的集合***。


### 练习

```java
import java.util.HashMap;
import java.util.Scanner;

/*
题目要求：
输入一个字符串，判断其中每个字符出现的次数？
分析：
1. 使用Scanner类中的next方法，接受键盘输入的字符串
2. 创建Map集合， 其中字符作为key，字符出现的次数作为value
3. 遍历字符串，取出每一个字符
4. 使用获取到的字符，去Map集合判断key是否存在
    key存在
            通过key（字符）获取value（次数）
            value++
            put(key, value)，更新value的值
    key不存在
            put(key, 1)
5. 遍历Map集合，输出结果

遍历字符串的两种方式：
1. 使用String的toCharArray()方法，将字符串变成字符数组，循环遍历

2. 使用String的length()方法和charAt(int index)：获取每个索引处的单个字符，来遍历
 */
public class DempMapPractise {
    public static void main(String[] args) {
        System.out.println("请输入一个字符串：");
        // 使用Scanner类中的next方法
        Scanner sc = new Scanner(System.in);
        String str = sc.next();
        // 创建HashMap集合，存储字符和相应的次数
        HashMap<Character, Integer> map = new HashMap<>();
        // 遍历字符串，方法1：
        /*for (char c : str.toCharArray()){
            // 如果map中包含c
            if (map.containsKey(c)){
                Integer value = map.get(c);
                value++;
                map.put(c, value);
            }else{ // 如果不包含c
                map.put(c, 1);
            }
        }*/

        // 遍历字符串，方法2：
        for (int i = 0; i < str.length(); i++){
            char c = str.charAt(i);
            if (map.containsKey(c)){
                Integer value = map.get(c);
                value++;
                map.put(c, value);
            }else {
                map.put(c, 1);
            }
        }

        // 遍历map集合，打印结果
        for (Character key : map.keySet()) {
            Integer value = map.get(key);
            System.out.println(key + "=" + value);
        }
        // System.out.println(map);
    }
}
```

### JDK9对集合添加的优化

JDK9的新特性：
	List接口，Set接口，Map接口，里边增加了一个静态方法***of***，可以给集合一次性添加多个元素。
	static `<E>` List`<E>` of(E ... elements)
	使用前提：当集合中存储的元素个数是确定的，不再改变时使用。
	
> 注意事项：
> 1. of 方法只适用于：List接口、Set接口、Map接口，不适用于接口的实现类。
> 2. of 方法的返回值是一个不能改变的集合，集合不能再使用add、put方法添加元素，会抛出异常：UnsupportedOperationException：不支持操作异常。
> 3. Set接口和Map接口在调用 of 方法的时候，不能有重复的元素，否则后抛出异常：IllegalArgumentException：非法参数异常，有重复的元素。
> 

示例：

```java
import java.util.List;
import java.util.Map;
import java.util.Set;

public class DemoStaticMehodof {
    public static void main(String[] args) {
        List<String> list = List.of("a", "b", "c", "d");
        System.out.println(list);
//        list.add("r"); // UnsupportedOperationException：不支持操作异常
//        System.out.println(list.remove("a"));// UnsupportedOperationException

       // Set<String> set = Set.of("a", "b", "a", "c");
      // System.out.println(set); // IllegalArgumentException：非法参数异常

     // IllegalArgumentException：非法参数异常，有重复元素
//        Map<String, Integer> map = Map.of("聂离", 20, "聂离", 21, "杜泽", 22);
//        System.out.println(map);

    }
}
```


## 10. 综合案例：斗地主（双列集合）

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.HashMap;

/*
1. 准备牌
2. 洗牌
3. 发牌
4. 排序
5. 看牌
 */
public class PokerGamePlus {
    public static void main(String[] args) {
        // 用来存储扑克的编号和 54张扑克
        HashMap<Integer, String> poker = new HashMap<>();
        // 扑克的编号
        ArrayList<Integer> pokerIndex = new ArrayList<>();
        // 先把大王小王及其编号存储进扑克
        poker.put(0, "大王");
        pokerIndex.add(0);
        poker.put(1, "小王");
        pokerIndex.add(1);

        // 两个集合分别存储花色和编号
        String[] colors = {"♥", "♦", "♣", "♠"};
        String[] numbers = {"2", "A", "K", "Q", "J", "10", "9", "8", "7", "6", "5", "4", "3"};
        int index = 2;
        for (String number : numbers) {
            for (String color : colors) {
                poker.put(index, color + number);
                pokerIndex.add(index);
                index++;
            }
        }
//        System.out.println(poker);

        // 2. 洗牌
        Collections.shuffle(pokerIndex);
        // System.out.println(pokerIndex);

        // 3. 发牌
        // 3个玩家 + 1个底牌
        ArrayList<Integer> playerA = new ArrayList<>();
        ArrayList<Integer> playerB = new ArrayList<>();
        ArrayList<Integer> playerC = new ArrayList<>();
        ArrayList<Integer> hand = new ArrayList<>();

        for (int i = 0; i < pokerIndex.size(); i++) {
            Integer in = pokerIndex.get(i);
            int j = i % 3;
            if (i >= 51) {
                hand.add(in);
            } else if (j == 0) {
                playerA.add(in);
            } else if (j == 1) {
                playerB.add(in);
            } else if (j == 2) {
                playerC.add(in);
            }
        }
        // 4. 排序，默认为从小到大
        Collections.sort(playerA);
        Collections.sort(playerB);
        Collections.sort(playerC);
        Collections.sort(hand);

        // 5. 发牌
        seePoker("周润发", playerA, poker);
        seePoker("刘德华", playerB, poker);
        seePoker("周星驰", playerC, poker);
        seePoker("底牌", hand, poker);


    }

    public static void seePoker(String name, ArrayList<Integer> keys, HashMap<Integer, String> poker) {
        System.out.print(name + ":");
        for (Integer key : keys) {
            String value = poker.get(key);
            System.out.print(value + " ");
        }
        System.out.println();
    }
}
```
