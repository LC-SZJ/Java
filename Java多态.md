[TOC]

# Java 多态

一个对象拥有多种形态，这就是：***对象的多态性***。

## 1. 多态的格式与使用

+ 代码当中体现多态性，其实就是一句话：***父类引用指向子类对象***。
+ 格式：

```java
父类名称 对象名 = new 子类名称();
或者
接口名称 对象名 = new 实现类名称();
```

## 2. 多态中成员变量的访问特点

+ 和继承中一样，没有任何变化。
+ 成员变量不能覆盖重写，成员方法能覆盖重写。
+ 子类没有覆盖重写，就是父。
+ 子类有覆盖重写，就是子。

## 3. 多态中成员方法的使用特点

+ 口诀：***编译看左边，运行看右边****

> 对比一下：
> 成员变量：编译看左边，运行还看左边。
> 成员方法：编译看左边，运行看右边。
> 

## 4. 多态的好处
无论右边***new***的时候换成了哪个子类对象，等号左边的调用方法都不会变化。


## 5. 对象的向上转型

对象的向上转型，其实就是多态的写法。
+ 格式：`父类名称 对象名 = new 子类名称();`
+ 含义：右侧创建一个子类对象，把它当做父类来看待使用。
+ 示例：
`Animal animal = new Cat();// 创建了一只猫，当作动物看，没问题`

> 注意事项：***向上转型一定是安全的***，从小范围转向了大范围。
> 类似于：
> double num = 100 ; // 正确，int --> double ，自动类型转化。
> 

## 6. 对象的向下转型

对象的向下转型，其实是一个***还原***的动作。向下类型装换的时候，一定要用instaceof关键字进行判断。
+ 格式：`子类名称 对象名 = (子类名称) 父类对象;`
+ 含义：将父类对象，***还原***成为本来的子类对象。
+ 示例：
`Animal animal = new Cat();`
`Cat cat = (Cat) animal; `

> 注意事项：
> 1. 必须保证对象本来创建的时候，就是猫，才能向下转型成为猫。
> 2. 如果对象创建的时候本来不是猫，现在非要向下转型成为猫，运行时就会报错。
> ***java.lang.ClassCastException***
> 类似于：
> 	int num = (int) 10.0; // 可以	int num = (int) 10.5 // 不可以，精度损失 
> 

## 7. 用instanceof关键字进行类型判断
如何才能知道一个父类引用的对象，本来是什么子类？

+ 格式：`对象 instanceof 类名称;
这将会得到一个boolean值结果，也就是判断前面的对象能不能当做后面类型的示例。

+ 示例

```java
// Cat.java
public class Cat extends Animal{

    @Override
    public void eat() {
        System.out.println("猫吃鱼");
    }
    public void catchMouse(){
        System.out.println("猫爪老鼠");
    }
}

// Dog.java
public class Dog extends Animal{
    @Override
    public void eat() {
        System.out.println("狗吃骨头");
    }

    public void watchHouse(){
        System.out.println("狗看房子");
    }
}

// DemoMain.java
public class DemoMain {
    public static void main(String[] args) {
        Animal animal = new Cat();
        giveMeAPet(animal);
    }

    public static void giveMeAPet(Animal animal){
        if (animal instanceof Dog){
            Dog dog = (Dog)animal;
            dog.eat();
            dog.watchHouse();
        }
        if (animal instanceof Cat){
            Cat cat = (Cat)animal;
            cat.eat();
            cat.catchMouse();
        }
    }
}
```

## 8. 笔记本USB接口案例
重点：接口的基本使用、对象的上下转型以及使用接口作为方法的参数。
Computer ***使用*** USB接口，Mouse、KeyBoards ***实现*** USB接口

+ USB.java

```java
// USB接口类
public interface USB {
    public abstract void open();

    public abstract void close();
}
```

+ Computer.java

```java
// 电脑类
public class Computer {

    public void powerOn(){
        System.out.println("电脑开机");
    }
    public void powerOff(){
        System.out.println("电脑关机");
    }
    // 接口作为方法的参数
    public void useDevice(USB usb){
        usb.open();
        if (usb instanceof Mouse){ // 一定要先判断
            Mouse mouse = (Mouse)usb; // 向下转型
            mouse.click();
        } else if (usb instanceof KeyBoard) {
            KeyBoard keyboard = (KeyBoard)usb;
            keyboard.type();
        }
        usb.close();
    }
}
```

+ Mouse.java

```java
// 鼠标类
public class Mouse implements USB{
    @Override
    public void open() {
        System.out.println("打开鼠标");
    }

    @Override
    public void close() {
        System.out.println("关闭鼠标");
    }

    public void click(){
        System.out.println("鼠标点击");
    }
}
```

+ KeyBoard.java

```java
// 键盘类
public class KeyBoard implements USB{
    @Override
    public void open() {
        System.out.println("打开键盘");
    }

    @Override
    public void close() {
        System.out.println("关闭键盘");
    }
    public void type(){
        System.out.println("键盘输入");
    }
}
```

+ Demo02Main.java

```java
public class Demo02Main {
    public static void main(String[] args) {
        Computer computer = new Computer();
        computer.powerOn();

        USB usbMouse = new Mouse();
        computer.useDevice(usbMouse);

//        USB usbKeyBoard = new KeyBoard();
        KeyBoard keyBoard = new KeyBoard();
        computer.useDevice(keyBoard);

        computer.powerOff();
    }
}
```