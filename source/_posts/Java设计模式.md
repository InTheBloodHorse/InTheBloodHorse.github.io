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

常见的单例模式实现方式
——主要：
- 饿汉式（线程安全，调用效率高。不能延时加载）
- 懒汉式（线程安全，调用效率不高。可以延时加载）
——其他：
- 双重检测锁式（由于JVM底层内部模型原因，偶尔会出问题，不建议使用）
- 静态内部类式（线程安全，调用效率高。可以延时加载）
- 枚举单例（线程安全，调用效率高。不能延时加载）

## 饿汉式
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
## 懒汉式
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
## 双重检测锁实现
```Java
package inthebloodhorse.designpatter.singleton;

public class Test {
    public static void main(String[] args) {
        // 饿汉式
        SingletonHunger s1 = SingletonHunger.getInstance();
        SingletonHunger s2 = SingletonHunger.getInstance();
        System.out.println(s1 == s2);

        // 懒汉式
        SingletonLazy sl1 = SingletonLazy.getInstance();
        SingletonLazy sl2 = SingletonLazy.getInstance();
        System.out.println(sl1 == sl2);

        DoubleCheckLock d1 = DoubleCheckLock.getInstance();
        DoubleCheckLock d2 = DoubleCheckLock.getInstance();
        System.out.println(d1 == d2);

    }
}
```

