[TOC]
# Java 继承

## 1. 概述

+ 面向对象三大特征：封装性，继承性，多态性。
+ 继承是多态的前提，没有继承既没有多态。
+ 继承好比师父和徒弟的关系。
+ 继承主要解决的问题是：***共性抽取***
+ 继承当中的特点：
    + 子类可以拥有父类的“内容”。
    + 子类还可以拥有自己专有的内容。

## 2. 继承的格式

+ 在继承的关系中，“子类就是一个父类”。也就是说，子类可以被当作父类看待。例如：父类是员工，子类是讲师，那么“讲师就是一个员工”。
+ 关系：is-a
+ 定义父类的格式和定义一个普通的类没有区别
+ 定义子类的格式：

```java
public class 子类名称 extends 父类名称{
      // ...
}
```

## 3. 继承中成员变量的访问特点

在父子类的继承关系中，如果成员变量重名，则创建子类对象时，访问有两种方式：

1. 直接通过子类对象访问成员变量：***等号左边是谁，就优先用谁，没有则向上找。***
2. 间接通过成员方法访问成员变量：***该方法属于谁，就优先用谁，没有则向上找。***


## 4. 区分子类方法中重名的三种变量

+ 局部变量：			直接写局部变量名
+ 本类的成员变量：***this.成员变量名***
+ 父类的成员变量：***super.成员变量名***


## 5. 继承中成员方法的访问特点

在父子类的继承关系当中，创建子类对象，访问成员方法的规则：***创建的对象是谁，就优先用谁，如果没有则向上找。***
> 注意事项：无论是成员方法还是成员变量，如果没有都是向上找父类，绝对不会向下找子类。
> 

## 6. 继承中方法的覆盖重写

+ 重写（override）：在继承关系当中，方法名称一样，参数列表***也一样***。覆盖，覆写。
+ 重载（overload）：方法名称一样，参数列表***不一样***。
+ 方法覆盖重写的特点：创建的是子类对象，则优先用子类方法。
> 注意事项：
> 1. 必须保证父子类之间方法的名称相同，参数列表也相同。
> ***@override***：写在方法前面，用来检测是不是有效的正确覆盖重写。
> 这个注解就算不写，只要满足要求，也算正确的方法覆盖重写。
> 2. 子类方法的返回值必须***小于等于***父类方法的返回值范围。
> 小扩展提示：java.long.Object类是所有类的公共最高父类（祖宗类），java.long.String就是Object类的子类。
> 3. 子类方法的权限必须***大于等于***父类方法的权限修饰符。
> 小扩展提示：public > protected > (default) > private
> 备注：(default)不是关键字default，而是什么都不写，留空。
> 90%以上的情况，方法的权限修饰符和返回值范围相同。

+ 设计原则：对于已经投入使用的类，尽量不要进行修改。推荐定义一个新的类，来重复利用其中共性内容，并且添加改动新内容。


## 7. 继承中构造方法的访问特点

1. 子类构造方法当中有一个默认隐含的“super();”调用，所以一定是先调用父类构造，后执行子类构造。
2. 子类构造可以通过super关键字来调用父类重载函数。
3. ***super的父类构造调用，必须是子类构造方法的第一个语句，不能一个子类构造调用多次super构造。***

总结：
子类必须调用父类的构造方法，不写则赠送super();，写了则用写的指定的super调用，super只能有一个，还必须是第一个。


## 8. super关键字的三种用法

1. 在子类的成员方法中，访问父类的成员变量。
2. 在子类的成员方法中，访问父类的成员方法。
3. 在子类的构造方法中，访问父类的构造方法。`super()`

## 9. this关键字的三种用法

1. 在本类的成员方法中，访问本类的成员变量。
2. 在本类的成员方法中，访问本类的另一个成员方法。
3. 在本类的构造方法中，访问本类的另一个构造方法。
> 注意事项：
> this(...)调用也必须是构造方法的第一个语句，唯一一个。
> super 和 this 两种构造调用，不能同时使用。
> 

```java
public class Zi extends Fu{
    int num = 20;
    public Zi(){
        this(123);
    }
    public Zi(int n){

    }
}
```

## 10. super和this关键字图解


![super和this关键字图解](https://ae01.alicdn.com/kf/Hb995e5c4e8674c7ba2f2d35f3ef0cf2fK.jpg)


## 11. Java继承的三个特点

1. Java语言是单继承的。
2. Java语言可以多级继承。
3. 一个子类的直接父类是唯一的，但是一个父类可以拥有很多个子类。


## 12. Java 抽象类

### 抽象方法和抽象类的格式

+ 抽象方法：就是在返回值类型前加上abstract关键字，然后去掉大括号，直接分号结束。
+ 抽象类：抽象方法所在的类，必须是抽象类才行，在class之前写上abstract即可。
+ 在抽象类中可以定义普通的成员方法。

```java
// 抽象类
public abstract class Animal {
    // 抽象方法
    public abstract void eat();
    // 普通成员方法
    public void normalMethod(){
        
    }
}
```

### 抽象类和抽象方法的使用

1. 不能直接new抽象类对象。
2. 必须用一个子类来继承抽象父类。
3. 子类必须覆盖重写抽象父类当中所有的抽象方法。覆盖重写（实现）：子类去掉抽象方法当中的abstract关键字，然后补上方法体大括号。
4. 创建子类对象进行使用。

### 注意事项
1. 抽象类不能直接创建对象。
2. 抽象类中，可以有构造方法，是供子类创建对象时，初始化父类成员使用的。
3. 抽象类中不一定不含抽象方法。但有抽象方法的类必定是抽象类。
4. 抽象类的子类，必须重写抽象父类中***所有***抽象方法，否则，编译无法通过而报错，除非改子类也是抽象类。

## 13. 发红包综合案例
+ User.java
```java
// 用户类
public class User {
    private String name; // 姓名
    private int money; // 余额

    public User() {
    }

    public User(String name, int money) {
        this.name = name;
        this.money = money;
    }
    // 展示用户姓名和金额
    public void show(){
        System.out.println("我叫：" + name + "，我有" + money + "元");
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getMoney() {
        return money;
    }

    public void setMoney(int money) {
        this.money = money;
    }
}
```
+ Manager.java

```java
import java.util.ArrayList;
// 群主类
public class Manager extends User{

    public Manager() {
    }

    public Manager(String name, int money) {
        super(name, money);
    }
    // 发红包
    public ArrayList<Integer> send(int totalMoney, int count){
       int leftMoney = super.getMoney();
       ArrayList<Integer> list = new ArrayList<>();
       // 判断一下，当前余额与发放的金额
       if (leftMoney < totalMoney){
           System.out.println("余额不足");
           return list; // 返回空集合
       }

       // 扣除当前发放的余额
        super.setMoney(leftMoney - totalMoney);

       // 发放金额
       int avg = totalMoney / count; // 平均金额
       int mod = totalMoney % count; // 零头

        for (int i = 0; i < count -1 ; i++) {
           list.add(avg) ;
        }
        int money = avg + mod;
        list.add(money);
        return list;
    }
}
```

+ Member.java

```java
import java.util.ArrayList;
import java.util.Random;
// 成员类
public class Member extends User{
    public Member() {
    }

    public Member(String name, int money) {
        super(name, money);
    }

    // 收红包
    public void receive(ArrayList<Integer> redList){

        // 获取在集合范围内的随机数
        int index = new Random().nextInt(redList.size());
        // 收到的红包金额，并删除
        int delta = redList.remove(index);
        // 当前金额
        int money = super.getMoney();
        // 重新设置金额
        super.setMoney(money + delta);
    }
}
```

+ MainRedPacket.java

```java
import java.util.ArrayList;
// 发红包主程序
public class MainRedPacket {
    public static void main(String[] args) {
        Manager manager = new Manager("群主", 100);

        Member one = new Member("成员A", 0);
        Member two = new Member("成员B", 0);
        Member three = new Member("成员C", 0);

        manager.show();
        one.show();
        two.show();
        three.show();
        System.out.println("=========");
        ArrayList<Integer> list = manager.send(20, 3);
        one.receive(list);
        two.receive(list);
        three.receive(list);

        manager.show();
        one.show();
        two.show();
        three.show();
    }
}
```


