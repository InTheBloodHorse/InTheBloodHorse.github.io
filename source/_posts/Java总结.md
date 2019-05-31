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

####  singleton

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

## 整合MyBatis

ApplicationContext.xml配置

``` xml
<!--配置数据源-->
<bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
    <property name="url" value="jdbc:mysql://localhost:3306/meeting?characterEncoding=utf-8"></property>
    <property name="username" value="root"></property>
    <property name="password" value="qqq1201"></property>
    <property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
</bean>
<!--创建sqlSessionFactory对象-->
<bean id="sqlFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
    <property name="dataSource" ref="dataSource"></property>
</bean>
<!--扫描器，扫描Mapper-->
<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
    <property name="basePackage" value="cn.inthebloodhorse.mapper"></property>
    <!--与sqlFactory进行关联-->
    <property name="sqlSessionFactoryBeanName" value="sqlFactory"></property>
</bean>
```

运行测试方法出现版本不一致问题。

```Java
Exception in thread "main" java.lang.AbstractMethodError:org.mybatis.spring.transaction.SpringManagedTransaction.getTimeout()Ljava/lang/Integer;
```

原因是 mybatis-spring ——spring——mybatis jar包版本不一致。

![2.png](2.png)

## Servlet

导入spring-web jar包

配置项目WEF-INF目录下的web.xml，每次启动Tomcat 自动加载Spring配置文件

```xml
<!--上下文参数-->
<context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>classpath:ApplicationContext.xml</param-value>
</context-param>
<!--监听器，自动加载Spring配置文件-->
<listener>
    <!--spring-web包-->
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
</listener>
```

```java
@WebServlet("/tag")
public class TagServlet extends HttpServlet {
    private TagService tagService;

    @Override
    public void init(ServletConfig config) throws ServletException {
        super.init(config);
        WebApplicationContext requiredWebApplicationContext = WebApplicationContextUtils.getRequiredWebApplicationContext(getServletContext());
        tagService = (TagService) requiredWebApplicationContext.getBean("tagService");
    }

    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        req.setAttribute("list", tagService.listTag());
        req.getRequestDispatcher("index.jsp").forward(req, resp);
    }
}
```

1.IDEA找不到WebService注解

File -> Project Structure -> Peoject Settings -> Libraries -> add Servlet.api.jar

2.运行Spring项目，IDEA->Edit Configurations 添加TomcatServer



## AOP 面向切面编程

Aspect-Oriented Programming

能够将那些与业务无关，**却为业务模块所共同调用的逻辑或责任（例如事务处理、日志管理、权限控制等）封装起来**，便于**减少系统的重复代码**，**降低模块间的耦合度**，并**有利于未来的可拓展性和可维护性**。

**Spring AOP就是基于动态代理的**，如果要代理的对象，实现了某个接口，那么Spring AOP会使用**JDK Proxy**，去创建代理对象，而对于没有实现接口的对象，就无法使用 JDK Proxy 去进行代理了，这时候Spring AOP会使用**Cglib** ，这时候Spring AOP会使用 **Cglib** 生成一个被代理对象的子类来作为代理。

1. 前置通知(before)
2. 正常返回通知(after-return)
3. 异常返回通知(after-throw)
4. 返回通知(after)
5. 环绕通知(around) 类实现MethodInterceptor接口，arg.proceed()放行。

Spring提供了2种AOP实现方式

### Scheam-based

- 声明schema命名空间(aop)

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
            http://www.springframework.org/schema/beans/spring-beans.xsd
            http://www.springframework.org/schema/aop
            http://www.springframework.org/schema/aop/spring-aop.xsd">
```

- 每个通知都需要实现接口或类

```java
public class TagAspect implements MethodBeforeAdvice {
    @Override
    public void before(Method method, Object[] objects, Object o) throws Throwable {
        System.out.println("执行前置通知");
    }
}
```

- 配置Spring文件时在\<aop:config>配置

```xml
<!--声明通知类的的Bean-->
<bean id="tagBefore" class="cn.inthebloodhorse.aspect.TagAspect"></bean>
<aop:config>
    <!--配置切点-->
    <aop:pointcut expression="execution(* cn.inthebloodhorse.service.impl.TagServiceImpl.listTag())" id="tagList"/>
    <!--通知-->
    <aop:advisor advice-ref="tagBefore" pointcut-ref="tagList"/>
</aop:config>
```

- \* 通配符，匹配任意方法名，任意类名，任意一级包名
- （..）匹配任意方法参数 

### AspectJ

- 每个通知不需要实现接口或类
- 配置Spring文件时在\<aop:config>的子标签\<aop:aspect>中配置，选择前置后置等

```xml
<!--AspectJ方式-->
    <aop:config>
        <!--指定的类-->
        <aop:aspect ref="tagBefore">
            <!--指定的方法-->
            <aop:before method="doSomethingBefore" pointcut-ref="tagList"/>
        </aop:aspect>
    </aop:config>
```

- 实现环绕通知

1. 定义通知类

```java
public class AspectAroundTest {
    public Object around(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {
        System.out.println("进入AspectJ环绕通知");
        long time = new Date().getTime();
        Object result = proceedingJoinPoint.proceed();
        System.out.println(new Date().getTime() - time);
        System.out.println("AspectJ环绕通知结束");
        return result;
    }
}
```

2.xml配置

```xml
<!--定义通知类-->
<bean id="aroundTest" class="cn.inthebloodhorse.aspect.AspectAroundTest"/>
<aop:config>
    <aop:aspect ref="aroundTest">
        <!--定义切点-->
        <aop:pointcut id="select"
                      expression="execution(* cn.inthebloodhorse.service.impl.TagServiceImpl.listTag())"/>
        <!--定义环绕通知-->
        <aop:around method="around" pointcut-ref="select"/>
    </aop:aspect>
</aop:config>
```

- 有参数的通知

1.定义有参数的方法（这里的参数是定义切点的方法的参数）

```java
public void before(Integer id) {
    System.out.println("set" + id);
}
```

2.定义xml

```xml
<aop:config>
    <aop:aspect ref="aroundTest">
        <!--定义切点-->
        <aop:pointcut id="personCut"
                      expression="execution(* cn.inthebloodhorse.entity.Person.setId(Integer)) and args(id)"/>
        <!--定义前置通知-->
        <aop:before method="before" arg-names="id" pointcut-ref="personCut"/>
    </aop:aspect>
</aop:config>
```



### 使用注解实现(基于Aspect)

1.spring不会自动去寻找注解，需要在xml中配置注解所在的包

2.声明schema命名空间(xmlns:context)

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
            http://www.springframework.org/schema/beans/spring-beans.xsd
            http://www.springframework.org/schema/aop
            http://www.springframework.org/schema/aop/spring-aop.xsd
            http://www.springframework.org/schema/context
            http://www.springframework.org/schema/context/spring-context.xsd">
```

3.配置context

作用是用@Component注解代替\<bean>标签

```xml
<context:component-scan base-package="cn.inthebloodhorse.aspect,cn.inthebloodhorse.service.impl"/>
```

4.@Component

等同于\<bean>，假如没有参数则把类首字母变小写，也可以自定义名称

5.定义切点（pointcut）

```java
@Component
public class TagServiceImpl implements TagService {
    private TagMapper tagMapper;

    @Override
    @Pointcut(value = "execution(* cn.inthebloodhorse.service.impl.TagServiceImpl.listTag()))")
    public List<Tag> listTag() {
        return tagMapper.listTag();
    }

    public TagMapper getTagMapper() {
        return tagMapper;
    }

    public void setTagMapper(TagMapper tagMapper) {
        this.tagMapper = tagMapper;
    }
}
```

6.定义切面类

```java
@Component
@Aspect
public class AnnotationAspect {
    @Before("cn.inthebloodhorse.service.impl.TagServiceImpl.listTag()")
    public void doBefore() {
        System.out.println("注解前置通知");
    }

    @After("cn.inthebloodhorse.service.impl.TagServiceImpl.listTag()")
    public void doAfter() {
        System.out.println("注解后置通知");
    }

    @Around("cn.inthebloodhorse.service.impl.TagServiceImpl.listTag()")
    public Object doAround(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {
        long time = new Date().getTime();
        Object result = proceedingJoinPoint.proceed();
        System.out.println(new Date().getTime() - time);
        return result;
    }

    @AfterThrowing(value = "cn.inthebloodhorse.service.impl.TagServiceImpl.listTag()", throwing = "e")
    public void doThrow(Exception e) {
        System.out.println("出现异常：" + e.getMessage());
    }
}
```

7.xml声明动态代理方式

```xml
<!--proxy-target-class-->
<!--true 使用cglib动态代理-->
<!--false 使用jdk动态代理-->
<aop:aspectj-autoproxy proxy-target-class="true"></aop:aspectj-autoproxy>
```

### 代理模式

#### 代理模式优点

1. 保护真实对象
2. 让真实对象职责更明确
3. 提高程序可扩展性









