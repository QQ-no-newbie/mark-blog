@[TOC](目录)
# 1. 继承
## 1.1继承的概述
如果多个类有多个相同的属性和方法，就可以定义一个父类，将这些相同的属性和方法整合到一起，然后可以将多个子类继承于父类，这些子类就可以拥有这些相同的属性和方法，可以提升代码的复用性。




![在这里插入图片描述](https://img-blog.csdnimg.cn/4c72f314afb94f8aa3663f71b232bf47.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5Lq65Zyo5LuW5Lmh6LSx5ZG9,size_17,color_FFFFFF,t_70,g_se,x_16)

（图片来自黑马程序员视频）
## 1.2继承的好处和弊端
### 1.2.1继承的好处
**1.2.1.1** 提高代码的复用性（多个类的相同成员可以放在同一类中）
**1.2.1.2** 提高代码的维护性（如果方法的代码需要修改，修改一处即可）
### 1.2.2继承的弊端
**1.2.2.1** 继承让类与类之间产生了关系，类的耦合性增加了，当父类发生变化的时候，子类也不得不变化，失去了独立性。
## 1.3继承的变量访问特点
访问子类的一个变量，按照一下顺序访问。

1.子类方法局部范围内查找
2.子类成员范围内查找
3.父类成员范围内查找
4.一直找到最后一个没有父类的类中的成员变量
5.都没有就报错

## 1.4关键字super
super关键字的用法和this用法类似 
**this** 代表本类对象的引用
**super** 代表父类储存空间的标识（可以理解为父类的引用）
## 1.5继承中构造方法的访问特点
**1.5.1** 子类的所有构造方法都会默认访问父类的构造方法
原因：
**1.5.1.1** 子类会继承父类中的数据，还可能会使用父类的数据，所以在子类初始化之前，一定要完成父类的构造方法。
**1.5.1.2** 每一个子类的构造方法的第一条语句默认都是super

**1.5.2**  若父类中只有带参的构造方法，没有带参的构造的方法
可以使用super关键字去调用父类的带参构造方法
或者在父类提供一个无参的构造方法
## 1.6继承中成员方法的访问特点
通过子类对象访问一个方法，按照一下顺序：
1. 子类成员范围查找
2. 父类成员范围查找
3. 一直找到最后一个没有父类的类
4. 都没有就报错
## 1.7方法重写
子类中出现了和父类中一样的方法声明，则叫做**方法重写。**
通常应用在子类需要父类的功能，又需要又自己的内容的时候。这样既可以沿袭父类方法，又可以有子类特定内容.
### 1.7.1@Override关键字
一个注解，帮助检查重写方法声明的正确性
### 1.7.2方法重写的注意事项
**1.7.2.1** 私有（private修饰）方法不能被重写
**1.7.2.1** 子类方法权限不能更低（public>默认>private）
## 1.8继承的注意事项
 **Java中类的继承可以多层继承，不能一个子类继承多个父类**
 （可以有爸爸的爸爸的爸爸……不能有多个爸爸）
# 2.多态
## 2.1 多态的概述
多态就是同一个对象，在不同时刻表现的出来的形态。	
比如说 一个运动员，我们可以说运动员是运动员，也可以说运动员是个人。
这个例子中运动员在不同时刻表现出不同的形态，这就是多态。
（这些看起来是废话，但是有一定的好处）
### 2.1.1多态的前提
多态的前提：
1.有继承/实现的关系（实现后面讲）
2.有方法重写
3.有父类引用指向子类
### 2.1.2实现的例子

```java
public class player {
    private int age;
    private String name;
}
```

```java
public class basketballplayer extends player{
    public void basketball() {
        System.out.println("play basketball!");
    }
}
```

```java
public class text {
    public static void main(String[] args) {
        player p1 = new basketballplayer();//多态的实现
    }
}
```
## 2.2多态的访问特点
### 2.2.1成员变量的访问特点
1.在编译时候看的是被子类引用的父类
2.在运行时候调用的也是被子类引用的父类
### 2.2.2成员方法的访问特点
1.在编译时看的是被子类引用的父类
2.在运行时候调用的是子类
### 2.2.3口诀总结

```java
player p1 = new basketballplayer();
```

成员变量 ：编译左，运行左
成员方法： 编译左，运行右
（左指等号左边，右同理）
## 2.3多态的好处与弊端
### 2.3.1多态的好处
提高了程序的扩展性
例子：当定义多个类继承于同一个父类

```java
public class basketballplayer extends player{
	@Override
    public void play() {
        System.out.println("play basketball!");
    }
}

public class swimplayer extends player{
	@Override
    public void play() {
        System.out.println("swim");
    }
}

public class footballplayer extends player {
	@Override
    public void play() {
        System.out.println("play football!");
    }
}
```
若要调用同一个父类有，却被子类重写的方法，在不用多态情况下

```java
public class text {
    public static void main(String[] args) {
        
    }
    public void usePlayer (basketballplayer p1) {
        p1.play();
    }
    public void usePlayer (swimplayer p2) {
        p2.play();
    }
    public void usePlayer (footballplayer p3) {
        p3.play();
    }
}
```
可以发现，如果有很多的子类继承同一个父类，并且调用被重写过的父类方法，那不是写死了此时我们就可以用多态

```java
public class text {
    public static void main(String[] args) {
        basketballplayer p1 = new basketballplayer();
        swimplayer p2 = new swimplayer();
        footballplayer p3 = new footballplayer();
        usePlayer(p1);
        usePlayer(p2);
        usePlayer(p3);
    }
    public static void usePlayer (player p1) {
        p1.play();
    }
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/000116807b034e54aa3bf1b653ca8f14.png)
### 2.3.2多态的弊端
不能使用子类型的特有功能，只能使用父类中定义的变量及方法
## 2.4多态的转型
### 2.4.1多态的向上转型
从子到父（父类引用指向子类）

```java
player p1 = new basketballplayer();//向上转型例子
```
### 2.4.2多态的向下转型
从父到子（父类引用转为子类）
若想使用子类的中定义的方法，可以进行向下转型使用
向下转型模板：
```java
player p1 = new basketballplayer();
basketballplayer p = (basketballplayer) p1;
```
# 3.抽象类
## 3.1 抽象类概述
一个没有方法体的方法应该被定义为抽象方法
而类中有抽象方法，该类就应该被定义为抽象类。
（听起来就挺抽象）
其实就是下面那个东西，只定义方法体就是不在方法里写东西，而抽象方法必须要用abstract来关键字修饰，而有抽象方法的类必须被定义为抽象类。
```java
public abstract class Animal {
    public abstract void eat();
}
```
另外，抽象类不是一个具体的类，不能直接被实现。
## 3.2抽象类的特点
1.抽象类和抽象方法必须使用**abstract**关键字来修饰
例如 ：
public abstract void class 类名{}
public abstract void eat (); 
2.抽象类中不一定有抽象方法，有抽象方法一定是抽象类
3.抽象类不能实例化，若想实例化，参照多态的方式，通过子类对象实例化
4.抽象的子类，要么重写所有抽象方法，要么是抽象类
## 3.3抽象类的成员特点
1.成员变量既可以是变量，也可以是常量
2.有构造方法，但不能实例化。构造方法用于子类访问父类数据的初始化
3.成员方法既可以有抽象方法，也可以有非抽象的方法，抽象方法可以来限定子类完成某些操作，而非抽象方法可以提高代码复用性
# 4.接口
## 4.1接口的概述
接口是一种公共的规范标准，只要符合标准规范，大家都可以通用
Java中的接口更多的体现在对行为的抽象。
## 4.2接口的特点
1.接口的关键字用**interface**修饰
例如：public interface 接口名{}
2.类实现接口用**implements**表示
例如：public class implements 接口名{}
3.接口不能实例化
接口实例化可以参照多态的方式，利用实现类实例化，叫做接口多态
多态形式：具体类多态，抽象类多态，接口多态。
多态的前提：有继承/实现关系;有方法重写；有父（类/接口）引用指向（子/实例）类对象
4.接口的实现类
要么重写接口中所有抽象方法，要么是抽象类
## 4.3接口的成员特点
1.成员变量只能是常量
默认修饰符 **public static final**
2.接口没有构造方法，接口主要是对行为进行抽象，没有具体的存在，一个类如果没有父类，默认继承自Objct类
3.成员方法只能是抽象方法
默认修饰符 **public abstract**
## 4.4类与接口关系
类与类的关系：
继承，只能单继承，但是可以多继承
类与接口关系：
实现关系，可以单实现，也可以多实现，还可以在继承一类的同时可以实现多接口
接口和接口的关系
继承关系，可以单继承，也可以多继承
# 5 抽象类和接口的关系
## 5.1	成员区别
抽象类：有变量，有常量；有构造方法；有抽象方法也有非抽象方法
接口：只有常量和抽象方法
## 5.2 设计理念区别
抽象类：对类抽象，包括属性，行为
接口：对行为抽象，主要是行为
## 6 形参与返回值
## 6.1类名作为形参和返回值时
1.方法的形参是类名，其实需要的该类的对象
2.方法的返回值是类名，其实返回的是该类的对象
（其实是和c的结构体一样）
## 6.2 抽象类名作为形参和返回值时
1.方法的形参是抽象类名，其实需要的是该抽象类的子类对象``
2.方法的返回值是抽象类名，其实返回的是该抽象类的子类对象
	

```java
public abstract class Animal {
    public abstract void eat();
}



//Animal的继承类
public class Cat extends Animal{
    @Override
    public void eat() {
        System.out.println("cat eat fish");
    }
}


//操作类
public class Animaloperator {
    public void animalop(Animal A) {
        A.eat();
    }
    public Animal animalget() {
        Animal c = new Cat();
        return c;
    }
}


//测试类
public class text {
    public static void main(String[] args) {
        Animaloperator A = new Animaloperator();
        Animal C = new Cat();
        A.animalop(C);

        Animal c = A.animalget();
        c.eat();
    }
}

```
输出结果  
![在这里插入图片描述](https://img-blog.csdnimg.cn/b05178153f54438389e835d252f7f366.png)
## 6.2 接口名作为形参和返回值
1.方法的形参是接口名，其实需要的是该接口的实现对象``
2.方法的返回值是接口名，其实返回的是该接口的实现对象
（和抽象类类似，都是使用多态实现的）

# 7.内部类
内部类的概述不多说了：就是在类中定义一个类
定义格式就是

```java
public class 类名 {
	//修饰符可以有public和private
    修饰符 class 类名 {
        
    }
}
```

## 7.1 内部类的访问特点
1.内部类可以直接访问外部类的成员，包括私有
2.外部类要访问内部类的成员，必须创建对象


## 7.2 类的在不同位置
1.在类的成员位置：成员内部类
2.在类的局部位置：局部内部类
### 7.2.1 成员内部类	
定义格式：
外部类名.内部类名 对象名 = 外部类名.内部类对象；

```java
	Outer.Inner oi = new Outer.new Inner();
	(内部类的类型必须是public修饰符修饰，private修饰符修饰的只能用外部类的方法调用)
```
### 7.2.2 局部内部类
局部内部类就是在方法中定义类，外界是无法使用的，需要在方法内创建对象并使用。
该类可以直接访问外部类的成员，也可以访问局部变量。
## 7.3 匿名内部类
### 7.3.1 概述
前提：存在一个类（可以是具体类也可以是抽象类）或者接口
匿名内部类是一种特殊的局部内部类	

```java
	new 类名或者接口名 {
			重写方法；
	}；
```
若想要调用匿名内部类的方法可以

```java
	new 类名或者接口名 {
			重写方法；
	}.方法名（）；
```
或者用多态的方式将子类或者实现类的对象赋值给一个父类或者接口再执行

```java
//范例
public class Outer {
    Inter i = new Inter() {
        @Override
        public void show() {
            System.out.println("xxxxxxx");
        }
    };
    i.show();
}
```
### 7.3.2 匿名内部类在开发中的使用
直接在代码中讲解

```java
public class text {
    public static void main(String[] args) {
        //创建操作类对象，并且调用method方法
        operator jo = new operator();
        jumpping j = new Cat();
        jo.method(j);

        jumpping d = new Dog();
        jo.method(d);

        //若只使用一次的话，每次使用一种jumpping的实现类都挺要创建一种类，十分麻烦。
        // （父类子类也是如此）
        //可以直接使用匿名内部类，范例：
        jo.method(new jumpping() {
            @Override
            public void jump() {
                System.out.println("Cat is jummping");
            }
        });
        
        jo.method(new jumpping() {
            @Override
            public void jump() {
                System.out.println("Dog is jumpping");
            }
        });
    }
}
```
输出如下：  
![在这里插入图片描述](https://img-blog.csdnimg.cn/d78b5c7a5cc0484392122114b690e3c9.png)  
一样的效果，但是缺少定义了两个类，提高了效率（像我这种懒比最喜欢的方式）



