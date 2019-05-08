---
title: Java设计模式
date: 2019-05-05 20:36:58
tags:
- 设计模式
categories:
- Java
---
# 创建型模式
## 单例模式
核心作用：保证一个类只有一个实例，并且提供一个访问该实例的全局访问点。
常见的应用场景：
- Windows的任务管理器，回收站
- 网站的计数器
- 数据库连接池也采用单例模式，因为数据库连接是一种数据库资源
- 操作系统的文件系统，一个操作系统只能有一个文件系统
- 在Spring中，每个Bean默认就是单例的，这样做的优点是Spring容器可以管理

单例模式的优点：
- 由于单例模式只生成一个实例，减少了系统性能开销，当一个对象的产生需要比较多的资源时，如读取配置、产生其他依赖对象时，则可以通过在应用启动时直接产生一个单例对象，然后永久驻留内存的方式来解决
- 单例模式可以在系统设置全局的访问点，优化环共享资源访问，例如可以设计一个单例类，负责所有数据表的映射处理

单例模式实现方式
- 饿汉式（线程安全，调用效率高。不能延时加载）
- 懒汉式（线程安全，调用效率不高。可以延时加载）
- 双重检测锁式（由于JVM底层内部模型原因，偶尔会出问题，不建议使用）
- 静态内部类式（线程安全，调用效率高。可以延时加载）
- 枚举单例（线程安全，调用效率高。不能延时加载）

### 饿汉式
```Java
package inthebloodhorse.designpatter.singleton;

/*
    单例模式
    饿汉式
    线程安全，调用效率高。不能延时加载
 */
public class SingletonHunger {
    // 类初始化时，立即加载这个对象
    public static SingletonHunger instance = new SingletonHunger();

    private SingletonHunger() {

    }

    // 方法没有同步(synchronized),效率高
    public static SingletonHunger getInstance() {
        return instance;
    }

}

```
### 懒汉式
```Java
package inthebloodhorse.designpatter.singleton;

/*
    单例模式
    懒汉式
    线程安全，调用效率不高。可以延时加载
 */
public class SingletonLazy {
    // 真正用到的时候才去加载
    private static SingletonLazy instance;

    private SingletonLazy() {

    }

    // 资源利用率高。调用getInstance方法都要同步，并发效率低
    public synchronized static SingletonLazy getInstance() {
        if (instance == null) {
            instance = new SingletonLazy();
        }
        return instance;
    }
}
```
### 双重检测锁实现
```Java
package inthebloodhorse.designpatter.singleton;

/*
    单例模式
    双重检测锁实现
    这个模式将同步内容下放到if内部，提高执行效率
    不必每次获取对象时都进行同步，只有第一次才同步
    创建了以后就没必要了
 */

public class DoubleCheckLock {
    private static DoubleCheckLock instance;

    private DoubleCheckLock() {

    }

    public static DoubleCheckLock getInstance() {
        if (instance == null) {
            DoubleCheckLock temp;
            synchronized (DoubleCheckLock.class) {
                temp = instance;
                if (temp == null) {
                    synchronized (DoubleCheckLock.class) {
                        if (temp == null) {
                            temp = new DoubleCheckLock();
                        }
                    }
                    instance = temp;
                }
            }
        }
        return instance;
    }
}
```
### 静态内部类
```Java
package inthebloodhorse.designpatter.singleton;

/*
    单例模式
    静态内部类
    线程安全，调用效率高。可以延时加载
    因为 类的加载过程是线程安全的
 */

public class StaticInnerClass {

    // 延迟加载 当调用了getInstance()方法的时候 再加载此内部类
    private static class InnerClass {
        private static StaticInnerClass instance = new StaticInnerClass();
    }

    private StaticInnerClass() {

    }

    public static StaticInnerClass getInstance() {
        return InnerClass.instance;
    }
}
```
### 枚举实现
```Java
package inthebloodhorse.designpatter.singleton;

/*
    单例模式
    通过枚举来实现
    线程安全，调用效率高。不能延时加载
    因为枚举本身就是单例模式
    而且枚举可以阻止反射、序列化、反序列化漏洞，更加安全
 */
public enum EnumSingleton {
    // 枚举元素本身就是单例的
    INSTANCE;

    private Integer age = 1;

    // 添加自己需要的操作
    public void operation(Integer add) {
        age += add;
        System.out.println(age);
    }
}
```
## 工厂模式
- 实现了创建者和调用者的分离
- 分为 简单工厂模式，工厂方法模式，抽象工厂模式
- 用工厂方法代替new操作
- 将选择实现类、创建对象统一管理和控制。从而将调用者跟我们的实现类解耦。

### 简单工厂模式
```Java
package inthebloodhorse.designpatter.factory.simpleFactory;

public interface Car {
    void drive();
}
```
```Java
package inthebloodhorse.designpatter.factory.simpleFactory;

public class Audi implements Car {
    @Override
    public void drive() {
        System.out.println("Audi is runing!");
    }
}
```
```Java
package inthebloodhorse.designpatter.factory.simpleFactory;

public class Bmw implements Car {
    @Override
    public void drive() {
        System.out.println("Bmw is Running!");
    }
}
```
```Java
package inthebloodhorse.designpatter.factory.simpleFactory;

import java.util.HashMap;

public interface CarTypeMap {

    HashMap<Integer, Class> carType = new HashMap<Integer, Class>() {
        {
            put(1, Audi.class);
            put(2, Bmw.class);
        }
    };

}
```
```Java
package inthebloodhorse.designpatter.factory.simpleFactory;

public class CarFactory {
    public static Car createCar(Integer type) {
        Class clazz = CarTypeMap.carType.get(type);
        try {
            return (Car) clazz.newInstance();
        } catch (InstantiationException e) {
            e.printStackTrace();
        } catch (IllegalAccessException e) {
            e.printStackTrace();
        }
        return null;
    }
}
```
```Java
package inthebloodhorse.designpatter.factory.simpleFactory;

public class Client {
    public static void main(String[] args) {
        Car car1 = CarFactory.createCar(2);
        Car car2 = CarFactory.createCar(2);
        Car car3 = CarFactory.createCar(1);
        car1.drive();
        car3.drive();

    }
}
```
## 建造者模式
- 分离了对象子组件的单独构造（由Builder来负责）和装配（有Director负责）。从而可以构造出复杂的对象。这个模式适用于：某个对象的构建过程复杂的情况下使用。
- 由于实现了构建和装配的解耦。不同的构建器，相同的装配，也可以做出不用的对象；相同的构建器，不同的装配顺序也可以做出不用的对象。也就是实现了构建算法、装配算法的解耦，实现了更好的复用。

```Java
package inthebloodhorse.designpatter.builder;

public class Car {
    private Engine engine;
    private Seat seat;
    private Wheel wheel;

    public Car(Engine engine, Seat seat, Wheel wheel) {
        this.engine = engine;
        this.seat = seat;
        this.wheel = wheel;
    }

    public Engine getEngine() {
        return engine;
    }

    public void setEngine(Engine engine) {
        this.engine = engine;
    }

    public Seat getSeat() {
        return seat;
    }

    public void setSeat(Seat seat) {
        this.seat = seat;
    }

    public Wheel getWheel() {
        return wheel;
    }

    public void setWheel(Wheel wheel) {
        this.wheel = wheel;
    }
}

class Engine {
    String name;

    public Engine(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}

class Seat {
    String name;

    public Seat(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}

class Wheel {
    String name;

    public Wheel(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```
```Java
package inthebloodhorse.designpatter.builder;

abstract public class CarBaseBuilder {
    abstract Engine buildEngine();

    abstract Seat buildSeat();

    abstract Wheel buildWheel();
}
```
```Java
package inthebloodhorse.designpatter.builder;

public class AudiBuilder extends CarBaseBuilder {

    @Override
    public Engine buildEngine() {
        return new Engine("Audi牌发动机");
    }

    @Override
    public Seat buildSeat() {
        return new Seat("Audi牌座位");
    }

    @Override
    public Wheel buildWheel() {
        return new Wheel("Audi牌轮胎");
    }
}
```
```Java
package inthebloodhorse.designpatter.builder;

public interface CarDirector {

    Car directorCar();
}
```
```Java
package inthebloodhorse.designpatter.builder;

public class CarDirectorImpl implements CarDirector {
    private CarBaseBuilder builder;

    public CarDirectorImpl(CarBaseBuilder builder) {
        this.builder = builder;
    }

    @Override
    public Car directorCar() {
        Engine engine = builder.buildEngine();
        Seat seat = builder.buildSeat();
        Wheel wheel = builder.buildWheel();

        Car car = new Car(engine, seat, wheel);
        return car;
    }
}
```
```Java
package inthebloodhorse.designpatter.builder;

public class Client {
    public static void main(String[] args) {
        CarDirectorImpl director = new CarDirectorImpl(new BmwBuilder());
        Car car = director.directorCar();
        System.out.println(car.getEngine().getName());
    }
}
```

## 原型模式
- 通过new产生一个对象需要非常繁琐的数据准备或者权限访问，则可以使用原型模式。
- 使用java中的克隆技术，以某个对象为原型，复制出新的对象。显然，新的对象具备原型对象的特点
- 效率高，直接克隆，避免了重新执行构造过程步骤
- 克隆出的对象的属性值完全和原型对象相同。并且克隆出的新对象改变不会影响原型的对象。然后再修改克隆对象的值。

原型模式的实现:
- Cloneable接口和clone()方法

### Cloneable(涉及浅拷贝和深拷贝)
```Java
package inthebloodhorse.designpatter.prototype;

import java.io.Serializable;
import java.util.Date;

public class CloneSheep implements Cloneable, Serializable {
    private String name;
    private Date date;

    public CloneSheep() {

    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Date getDate() {
        return date;
    }

    public void setDate(Date date) {
        this.date = date;
    }

    @Override
    protected Object clone() throws CloneNotSupportedException {
        CloneSheep cloneSheep = (CloneSheep) super.clone();

        // 深拷贝
        cloneSheep.date = (Date) this.date.clone(); // 浅拷贝的话 去掉本行
        return cloneSheep;
    }
}
```
```Java
package inthebloodhorse.designpatter.prototype;

import java.util.Date;

public class Client {
    public static void main(String[] args) throws CloneNotSupportedException {
        Sheep sheep = new Sheep();
        sheep.setName("多利");
        sheep.setDate(new Date());
        Sheep newSheep = (Sheep) sheep.clone();
        System.out.println(newSheep.getDate());
        sheep.getDate().setTime(15555);
        System.out.println(sheep.getDate());
        System.out.println(newSheep.getDate());

    }
}
```
### 序列化，反序列化
需要注意，Sheep类要先实现Serializable接口
```Java
package inthebloodhorse.designpatter.prototype;

import com.sun.xml.internal.messaging.saaj.util.ByteOutputStream;

import java.io.*;
import java.util.Date;

public class Client {
    public static void main(String[] args) throws IOException, ClassNotFoundException {
        CloneSheep cloneSheep = new CloneSheep();
        cloneSheep.setName("多利");
        cloneSheep.setDate(new Date());
        System.out.println(cloneSheep.getDate());
        // 序列化，反序列化
        ByteArrayOutputStream bos = new ByteArrayOutputStream();
        ObjectOutputStream oos = new ObjectOutputStream(bos);
        oos.writeObject(cloneSheep);
        byte[] bytes = bos.toByteArray();
        ByteArrayInputStream bis = new ByteArrayInputStream(bytes);
        ObjectInputStream ois = new ObjectInputStream(bis);
        CloneSheep newSheep = (CloneSheep) ois.readObject();
        

        newSheep.setDate(new Date(newSheep.getDate().getTime() + 1000000));
        System.out.println(cloneSheep.getDate());
        System.out.println(newSheep.getDate());

    }
}
```
# 结构型模式
核心作用：从程序的结构上实现松耦合，从而可以扩大整体的类结构，用来解决更大的问题。
## 适配器模式
将一个类的接口转换成客户希望的另一个接口。Adapter模式使得原本由于接口不兼容而不能一起工作的那些类可以在一起工作。
模式中的角色：
- 目标接口（Target）：客户所期待的接口。目标可以是具体的或者抽象的类，也可以是接口。
- 需要适配的；类（Adaptee）：需要适配的类或适配者的类。
- 适配器（Adapter）:通过包装一个需要适配的对象，把原接口转换成目标接口

先创建被适配的类（即Client无法直接调用该类的方法）
```Java
package inthebloodhorse.designpatter.adapter;

// 被适配的类
public class Adaptee {
    public void run() {
        System.out.println("满足基本的需求");
    }
}
```
定义适配器接口（用于Client与被适配的类进行沟通）
```Java
package inthebloodhorse.designpatter.adapter;

public interface Target {
    void handleRequest();
}
```
定义适配器类，实现Target接口
```Java
package inthebloodhorse.designpatter.adapter;

public class Adapter implements Target {
    private Adaptee adaptee;

    @Override
    public void handleRequest() {
        adaptee.run();
    }

    public Adapter(Adaptee adaptee) {
        this.adaptee = adaptee;
    }
}
```
Client端调用被适配的类
```Java
package inthebloodhorse.designpatter.adapter;

public class Client {
    public void test1(Target t) {
        t.handleRequest();
    }

    public static void main(String[] args) {
        Client client = new Client();

        Adaptee adaptee = new Adaptee();

        Adapter adapter = new Adapter(adaptee);

        client.test1(adapter);
    }
}
```
## 代理模式
核心作用：
通过代理，控制对对象的访问。可以详细控制访问某个对象的方法，在调用这个方法前做前置处理，调用这个方法后做后置处理。
++AOP（面向切面编程）的核心实现机制++

核心角色：
- 抽象角色：定义代理角色和真实角色的公共对外方法。
- 真实角色：实现抽象角色，定义真实角色所要实现的业务逻辑，供代理角色调用。++关注真正的业务逻辑++。
- 代理角色：实现抽象角色，是真实角色的代理，通过真实角色的业务逻辑方法来实现抽象方法，并可以附加自己的操作。++将统一的流程控制放到代理角色中处理++。

应用场景：
- MyBatis中实现拦截器插件
- Spring中AOP的实现
- 数据库连接池关闭处理
- 切面框架 AspectJ

### 静态代理模式
创建工作接口
```Java
package inthebloodhorse.designpatter.proxy;

public interface Work {
    void doWork();
}
```
创建工人类实现工作接口
```Java
package inthebloodhorse.designpatter.proxy;

public class BadWorker implements Work {

    @Override
    public void doWork() {
        System.out.println("Work bad!!!");
    }
}
```
创建代理类，并添加前置，后置方法
```Java
package inthebloodhorse.designpatter.proxy;

public class ProxyWorker implements Work {
    private Work realWorker;

    public ProxyWorker(Work realWorker) {
        this.realWorker = realWorker;
    }

    private void doWorkerBefore() {
        System.out.println("准备前置工作");
    }

    private void doWorkerAfter() {
        System.out.println("工作后续");
    }

    @Override
    public void doWork() {
        doWorkerBefore();
        this.realWorker.doWork();
        doWorkerAfter();
    }
}
```
Client进行测试
```Java
package inthebloodhorse.designpatter.proxy;


public class Client {
    public static void main(String[] args) {
        Work realWorker = new BadWorker();
        ProxyWorker proxyWorker = new ProxyWorker(realWorker);
        proxyWorker.doWork();
    }
}
```
结果
```Bash
准备前置工作
Work bad!!!
工作后续
```
### 动态代理（主要）
#### JDK自身实现
定义Work接口和RealWorker实现Work接口
```Java
package inthebloodhorse.designpatter.proxy.dynamicproxy;

public interface Work {
    Integer doWork(Integer type);
}
```
```Java
package inthebloodhorse.designpatter.proxy.dynamicproxy;

public class RealWorker implements Work {

    @Override
    public Integer doWork(Integer type) {
        System.out.println("正在做工作:" + type);
        return type;
    }
}
```
定义WorkerHandler类实现InvocationHandler接口，实现invoke()方法
```Java
package inthebloodhorse.designpatter.proxy.dynamicproxy;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.util.Date;

public class WorkerHandler implements InvocationHandler {
    private Work worker;

    public WorkerHandler(Work worker) {
        this.worker = worker;
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        Long start = new Date().getTime();
        Object result = method.invoke(this.worker, args);
        Long end = new Date().getTime();
        System.out.println(String.format("耗时为:%d", end - start));
        return result;
    }
}
```
Client调用
```Java
package inthebloodhorse.designpatter.proxy.dynamicproxy;

import java.lang.reflect.Proxy;

public class Client {
    public static void main(String[] args) {
        RealWorker realWorker = new RealWorker();
        WorkerHandler handler = new WorkerHandler(realWorker);
        // JDK自带动态代理
        Work work = (Work) Proxy.newProxyInstance(ClassLoader.getSystemClassLoader(), new Class[]{Work.class}, handler);
        Integer result = work.doWork(2);
        System.out.println(result);
    }
}
```
## 桥接模式
在处理多继承的结构的时候，往往需要处理多维变化的场景，例如电子设备有很多品牌，每种产品又分为台式，笔记本，平板等，假如每个都要设计类的话，则是二维的维度，对于扩展极其不方便。所以我们需要用桥接模式将各个维度设计成独立的继承结构，使各个维度可以独立的扩展在抽象层建立关联。

品牌类
```Java
package inthebloodhorse.designpatter.bridge;

// 品牌
public interface Brand {
    void sale();

}

class Lenovo implements Brand {

    @Override
    public void sale() {
        System.out.println("销售联想");
    }
}

class Dell implements Brand {

    @Override
    public void sale() {
        System.out.println("销售戴尔");
    }
}
```
产品类型类
```Java
package inthebloodhorse.designpatter.bridge;

public class Computer {
    protected Brand brand;

    public Computer(Brand brand) {
        this.brand = brand;
    }

    public void sale() {
        brand.sale();
    }
}

class Desktop extends Computer {
    protected Brand brand;

    public Desktop(Brand brand) {
        super(brand);
    }

    public void sale() {
        super.sale();
        System.out.println("销售台式机");
    }
}

class Laptop extends Computer {
    protected Brand brand;

    public Laptop(Brand brand) {
        super(brand);
    }

    public void sale() {
        super.sale();
        System.out.println("销售笔记本");
    }
}
```
Client
```Java
package inthebloodhorse.designpatter.bridge;

public class Client {
    public static void main(String[] args) {
        // 联想笔记本
        Brand brand = new Lenovo();
        Laptop laptop = new Laptop(brand);
        laptop.sale();
    }
}
```
总结：
- 桥接模式可以取代多层继承的方案。多层继承违背了单一职责原则，复用性较差，类的个数也非常多。桥接模式可以极大的减少子类的个数，从而降低管理和维护的成本。
- 桥接模式极大的提高了系统可扩展性，在两个变化维度中任意扩展一个维度，都不需要修改原有的系统，符合开闭原则。

## 组合模式
使用场景：
把部分和整体的关系用树形结构来表示，从而使客户端可以使用统一的方式处理部分对象和整体对象。
组合模式核心：
- 抽象构件角色：定义了叶子和父节点构建的共同点
- 叶子构件角色：无子节点
- 父节点构件角色：有子节点

使用组合模式，模拟杀毒软件构架设计
```Java
package inthebloodhorse.designpatter.composite;

public interface AbstractFile {
    
    void kill(int number);

}
```
文件类（文件夹，文件）
```Java
package inthebloodhorse.designpatter.composite;


import java.util.ArrayList;

public class Folder implements AbstractFile {
    public String name;
    private ArrayList<AbstractFile> fileList = new ArrayList<>();

    public Folder(String name) {
        this.name = name;
    }

    public void add(AbstractFile abstractFile) {
        this.fileList.add(abstractFile);
    }


    @Override
    public void kill(int number) {
        for (int i = 0; i < number; i++) {
            System.out.print("*** ");
        }
        System.out.println(String.format("开始排查文件夹%s", this.name));
        for (AbstractFile abstractFile : fileList) {
            if (abstractFile.getClass().equals(Folder.class)) {
                abstractFile.kill(number + 1);
            } else {
                abstractFile.kill(number + 1);
            }
        }
    }


}

class TextFile implements AbstractFile {

    public String name;

    public TextFile(String name) {
        this.name = name;
    }

    @Override
    public void kill(int number) {
        for (int i = 0; i < number; i++) {
            System.out.print("*** ");
        }
        System.out.println(String.format("开始排查文件%s", this.name));
    }


｝
```
Client
```Java
package inthebloodhorse.designpatter.composite;

import java.io.File;

public class Client {
    public static void addFile(Folder file, File parent) {
        for (File f : parent.listFiles()) {
            if (f.isDirectory()) {
                Folder childFile = new Folder(f.getName());
                addFile(childFile, f);
                file.add(childFile);
            }
            if (f.isFile()) {
                file.add(new TextFile(f.getName()));
            }
        }
    }

    public static void main(String[] args) {
        String path = "E:\\blog\\BestOfDp.github.io\\source";
        File file = new File(path);
        Folder root = new Folder(file.getName());
        // 读取文件夹目录
        addFile(root, file);

        root.kill(0);


    }
}
```