[TOC]

# Java 内部类

## 1. 概述与分类

如果一个事物的内部包含另一个事物，那么这是一个类的内部包含另一个类。例如：身体和心脏的关系，又如汽车和发动机的关系。
分类：
1. 成员内部类。
2. 局部内部类（匿名内部类）。

## 2. 成员内部类的定义
格式：
```java
修饰符 class 外部类名称{
	修饰符 class 成员内部类名称{
		// ...
	}
	// ...
}
```

> 注意：内用外，随意访问；外用内，需要内部类对象。
> 

## 3. 成员内部类的使用
两种方式
1. 间接方式：在外部类的方法当中，使用内部类，然后main只是调用外部类的方法。
2. 直接方式，公式：
***外部类名称.内部类名称 对象名 = new 外部类名称().new 内部类名称();***

## 4. 成员内部类的同名变量访问
如果出现了重名现象，那么格式是：***外部类名称.this.外部类成员变量***

```java
public class Outer {
     int num = 10;
    public class Inner{
        int num = 20;

       public void methodInner(){
            int num = 30;
           System.out.println(num); // 30
           System.out.println(this.num); // 20
           System.out.println(Outer.this.num); // 10
       }
    }
}
```

## 5. 局部内部类的定义 
如果一个类定义在一个方法的内部，那么这就是一个局部内部类。
***“局部”：只有当前所属的方法才能使用它，出了这个方法外面就不能用了。***

+ 格式：
```java
修饰符 class 外部类名称{
	修饰符 返回值类型 方法名称(参数列表){
		class 局部内部类名称{
			// ...
		}
	}
```

+ 示例

```java
// Outer.java
public class Outer {
    public void methodOuter(){
        class Inner{
            int num = 10;
            public void methodInner(){
                System.out.println(num); // 10
            }
        }
        Inner inner = new Inner();
        inner.methodInner();
    }
}

// InnerClass.java
public class InnerClass {
    public static void main(String[] args) {
        Outer obj = new Outer();
        obj.methodOuter();
    }
}

```

+ **小结一下类的权限修饰符：**
public > protected > (default) > private
定义一个类的时候，权限修饰符的规则：

1. 外部类：public / (default)
2. 成员内部类：public / protected / (default) / private
3. 局部内部类：什么都不能写，效果和(default)不一样。

## 6. 局部内部类的final问题
局部内部类，如果希望访问所在方法的局部变量，那么这个局部变量必须是***有效final***的。
> 备注：从JDK 8+ 开始，只要局部变量事实不变，那么final关键字可以省略。
> 

原因：
1. new出来的对象在堆内存当中。
2. 局部变量是跟这方法走的，在栈内存当中。
3. 方法运行结束之后，立即出栈，局部变量就会立即消失。
4. 但是new出来的对象会在堆当中持续存在，直到垃圾回收消失。


说白了，是new出来的对象和局部变量的生命周期问题，一旦局部变量消失，但是如果局部内部类想要使用局部变量，但这时局部变量已经消失了，所以这个局部变量必须是***有效final的***。

```java
public class Outer {
    public void methodOuter(){
//        final int num = 10;
        int num = 10;
        class Inner{
            public void methodInner(){
                System.out.println(num); // 10
            }
        }
        Inner inner = new Inner();
        inner.methodInner();
    }
}
```

## 7. 匿名内部类

+ 如果接口的实现类（或者父类的子类）只需要使用唯一的一次。
那么这种情况下就可以省略该类的定义，而改为使用***匿名内部类***。
+ 格式：
```java
接口名称 对象名 = new 接口名称(){
		// 覆盖重写所有抽象方法
};
```

+ 示例：

```java
public class InnerClass {
    public static void main(String[] args) {
//        MyInterfaceImpl impl = new MyInterfaceImpl();
//        impl.method();
        MyInterface obj = new MyInterface() {
            @Override
            public void method() {
                System.out.println("匿名内部类实现覆盖重写");
            }
        };
        obj.method();
    }
}
```

对格式“new 接口名称() {...}”进行解析：
1. new代表创建对象的动作
2. 接口名称就是匿名内部类需要实现哪个接口
3. {...}这才是匿名内部类的内容

另外还需要注意几点问题：
1. 匿名内部类，在***创建对象***的时候，只能使用唯一的一次。如果希望多次创建对象，而且类的内容一样的话，那么就必须使用单独定义的实现类了。
2. 匿名对象，在***调用方法***的时候，只能调用唯一的一次。如果希望同一个对象，调用多次方法，那么必须给对象起个名字。
3. 匿名内部类是省略了***实现类 / 子类名称***，但是匿名对象是省略了***对象名称***。
***强调：匿名内部类和匿名对象不是一回事！！！***


## 8. 类作为成员变量类型，接口作为成员变量类型 案例

```java
// DemoMain.java
public class DemoMain {
    public static void main(String[] args) {
        Hero hero = new Hero();

        hero.setName("盖伦");
        hero.setAge(20);

        Weapon weapon = new Weapon("AK-47");

        hero.setWeapon(weapon);
        // 调用接口当中的抽象方法。
//        hero.setSkill(new SkillImpl());

        // 使用匿名内部类
//        Skill skill = new Skill() {
//            @Override
//            public void use() {
//                System.out.println("Pia~pia~pia");
//            }
//        };
//        hero.setSkill(skill);

        // 使用匿名内部类和匿名对象
        hero.setSkill(new Skill() {
            @Override
            public void use() {
                System.out.println("Biu~pia~biu~pia");
            }
        });
        hero.attack();



    }
}


// Hero.java
public class Hero {
    private String name;
    private int age;
    private Skill skill;
    private Weapon weapon;

    public Hero(String name, int age, Skill skill, Weapon weapon) {
        this.name = name;
        this.age = age;
        this.skill = skill;
        this.weapon = weapon;
    }

    public Skill getSkill() {
        return skill;
    }

    public void setSkill(Skill skill) {
        this.skill = skill;
    }

    public Hero() {
    }


    public void attack(){
        System.out.println(name + "正在用" + weapon.getCode()+ "打怪。");
        System.out.println(name + "释放技能");
        skill.use();
        System.out.println("技能释放完毕");
    }
    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public Weapon getWeapon() {
        return weapon;
    }

    public void setWeapon(Weapon weapon) {
        this.weapon = weapon;
    }
}


// Skill.java
public interface Skill {

    public abstract void use();

}

// SkillImpl.java
public class SkillImpl implements Skill{
    @Override
    public void use() {
        System.out.println("Biu~biu~biu");
    }
}


// Weapon.java
public class Weapon {
    private String code; // 武器代号

    public Weapon() {
    }

    public Weapon(String code) {
        this.code = code;
    }

    public String getCode() {
        return code;
    }

    public void setCode(String code) {
        this.code = code;
    }
}

```
## 9. 接口作为方法的参数和返回值 案例

```java
// DemoInterface.java
import java.util.ArrayList;
import java.util.List;

public class DemoInterface {
    public static void main(String[] args) {
        // 左边是接口名称，右边是实现类名称，这就是多态写法
        List<String> list = new ArrayList<>();

        List<String> result = addNames(list);
        for (int i = 0; i < result.size(); i++) {
            System.out.println(result.get(i));
        }

    }
    public static List<String> addNames(List<String> list){
        list.add("迪丽热巴");
        list.add("马儿扎哈");
        list.add("沙扬娜拉");

        return list;
    }
}

```






