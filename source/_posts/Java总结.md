---
title: Java总结
date: 2019-05-30 21:18:55
categories:
- Java
---
# Java API

# Spring核心功能

![1.png](1.png)

## Core Container 核心容器

### Beans

负责管理对象

### Core

核心类

### Context

上下文参数，获取外部资源，管理注解等

### SpEL

expression.jar

## IOC/DI 控制反转/依赖注入

### IOC(Inversion of Control)

将对象交给容器控制，而不是传统的在内部直接控制。反转则是由容器来帮忙创建以及注入依赖对象，而不是主动去创建类。降低了类与类之间的耦合，使得程序变得灵活，可扩展。容器内的Bean默认是单例的，其中Bean作用域分为如下几种（Scope）：

#### singleton

IOC容器仅创建一个Bean实例

#### prototype

每次返回都是一个新的实例

#### request

该属性仅对http请求产生作用，每次http请求都会创建一个新的Bean，适用于WebApplicationContext环境

#### session

同一个Session共享一个Bean实例

#### global-session

所有的Session共享一个Bean实例

## AOP 面向切面编程

## 声明事务

## 创建Spring

1. 在IDEA中创建Spring工程。

2. src目录下面创建ApplicationContext.xml，其中配置的信息最终存储到了Spring的ApplicationContext容器中

3. spring配置文件是基于schema的，<font color="red">schema约束定义了xml文档的结构，内容和语法</font>，schema的文件扩展名为.xsd，每次引入一个xsd文件是一个namespace（xmlns）

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
               http://www.springframework.org/schema/beans/spring-beans.xsd">
   ```

4. Test

   ```Java
   public class Test {
       public static void main(String[] args) {
           ApplicationContext application = new ClassPathXmlApplicationContext("ApplicationContext.xml");
           Person person = (Person) application.getBean("person");
           Person person1 = (Person) application.getBean("person");
           System.out.println(person == person1);
       }
   }
   ```

## Spring创建对象的三种方式

### 构造方法

如果设定的条件匹配多个构造方法执行<font color="red">最后的构造方法</font>。

```XML
<bean id="personByNameAndId" class="cn.inthebloodhorse.entity.Person">
        <constructor-arg name="name" value="1ni"/>
        <constructor-arg name="id" value="1"/>
</bean>
```

### 实例工厂

1.定义实例工厂

```Java
public class PersonFactory {
    public Person getInstance() {
        return new Person("1ni",123);
    }
}
```

2.配置xml

```xml
<bean id="personFactory" class="cn.inthebloodhorse.factory.PersonFactory"></bean>
<bean id="person1" factory-bean="personFactory" factory-method="getInstance"></bean>
```

### 静态工厂

1.定义静态工厂

```Java
public class StaticPersonFactory {
    public static Person getInstance() {
        return new Person("2ni", 123456);
    }
}
```

2.配置xml

```xml
<bean id="person2" class="cn.inthebloodhorse.factory.StaticPersonFactory" factory-method="getInstance"></bean>
```

## 对Bean设置属性

1.通过set方法

使用<b>property</b>标签来设置 

```xml
<bean id="person" class="cn.inthebloodhorse.entity.Person">
    <property name="name" value="good"></property>
</bean>
```

## DI

依赖注入（Dependency Injection），当一个类A中需要依赖另一个类B对象时，把B赋值给A的过程称为依赖注入。

假设现在Person类依赖Teacher类，首先创建Teacher类，属性为id，name，className。

同样先创建teacher（bean），并且设置属性。

```xml
<bean id="teacher" class="cn.inthebloodhorse.entity.Teacher">
    <property name="id" value="1"></property>
    <property name="name" value="yz"></property>
    <property name="className" value="acm"></property>
</bean>
```

接着在person（bean）引用teacher（bean）,使用<font color="red">ref</font>进行属性的注入。

```xml
<bean id="person" class="cn.inthebloodhorse.entity.Person">
    <property name="name" value="good"></property>
    <property name="teacher" ref="teacher"></property>
</bean>
```

## MyBatis

出现

```Java
Exception in thread "main" java.lang.AbstractMethodError:org.mybatis.spring.transaction.SpringManagedTransaction.getTimeout()Ljava/lang/Integer;
```

原因是 mybatis-spring ——spring——mybatis jar包版本不一致。

![2.jpg](2.jpg)








