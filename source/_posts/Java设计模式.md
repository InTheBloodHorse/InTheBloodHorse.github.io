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