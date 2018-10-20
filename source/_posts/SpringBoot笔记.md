---
title: SpringBoot笔记
date: 2018-10-06 21:57:32
tags:
- Maven
categories:
- Java
---
## Maven中解决下载超时问题
Maven默认使用的是 开源中国的镜像，在cmd下面
```Bash
mvn spring-boot:run
```
会出现time out，这时候我们需要更换阿里镜像，在maven安装目录的下的settings.xml修改mirror
把
```Bash
<mirror>
        <id>nexus-osc</id>
        <mirrorOf>*</mirrorOf>
        <name>Nexus osc</name>
        <url>http://maven.oschina.net/content/groups/public/</url>
</mirror>
```
改成
```Bash
<mirror>
        <id>nexuss</id>
     <mirrorOf>*</mirrorOf>
     <name>Nexus aliyun</name>
     <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
</mirror>
```
这样就可以正常创建了
## 添加事务没有用，部分片段还是可以执行
在Service添加@Transactional注解，还是可以执行，那是因为Hibernate默认创建的表是MyISAM引擎，MyISAM引擎不支持事务操作。我们需要在application.yml下修改表的引擎。
```Bash
spring.jpa.database-platform=org.hibernate.dialect.MySQL5InnoDBDialect
```
如果还是不行的话，重新建表就好了。我们可以在run项目的时候看到控制的日志
```Bash
Hibernate: create table girl (id integer not null, age integer, cup_size varchar(255), primary key (id)) engine=InnoDB
```
## 用注解@Slf4j替代Logger
不想写private  final Logger logger = LoggerFactory.getLogger(XXX.class);可以用@Slf4j替代
1. 在pom文件下加入依赖
```Bash
<dependency>
   <groupId>org.projectlombok</groupId>
   <artifactId>lombok</artifactId>
</dependency>
```
2. 代码
```Java
package com.sell;
import lombok.extern.slf4j.Slf4j;
import org.junit.Test;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.junit.runner.RunWith;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.junit4.SpringRunner;/**
 *   日志测试
 */@RunWith(SpringRunner.class)
@SpringBootTest
@Slf4j
public class LoggerTest {
    private  final Logger logger = LoggerFactory.getLogger(LoggerTest.class);
    /**
     * 一、传统方式实现日志
     */
    @Test
    public  void test1(){
        logger.debug("debug message");
        logger.warn("warn message");
        logger.info("info message");
        logger.error("error message");
        logger.trace("trace message");
    }
    /**
     * 二、注解方式实现日志
     */
    @Test
   public  void test2(){
        log.debug("debug message");
        log.warn("warn message");
        log.info("info message");
        log.error("error message");
        log.trace("trace message");
    }
}
```
3. 如果在idea里面找不到log变量，给idea安装 lombok plugin插件即可
## 定义Entity实体的时候@DynamicUpdate注解实现时间自动更新
看了代码就明白了
```Java
package com.wechat.shopping.model;

import org.hibernate.annotations.DynamicUpdate;
import org.hibernate.annotations.UpdateTimestamp;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.Id;
import java.util.Date;

//类目
@Entity
@DynamicUpdate
public class ProductCategory {

    @Id
    @GeneratedValue
    private Integer categoryId;
    private String categoryName;
    private Integer categoryType;
    private Date createTime;
    private Date updateTime;

```
## 用lombok的@Data注解来实现自动getter，setter，toString方法
现在pom.xml下面添加依赖
```Bash
<dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
</dependency>
```
在idea里面也要安装lombok plugin插件即可。
修改之后的Entity代码为
```Java
package com.wechat.shopping.model;

import lombok.Data;
import org.hibernate.annotations.DynamicUpdate;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.Id;
import java.util.Date;

//类目
@Entity
@DynamicUpdate
@Data
public class ProductCategory {

    @Id
    @GeneratedValue
    private Integer categoryId;
    private String categoryName;
    private Integer categoryType;
//    private Date createTime;
//    private Date updateTime;
}
```
## 用@Transactional测试代码事务完全回滚
在测试函数打上@Transactional就可以实现事务的完全回滚
```Java
    @Test
    @Transactional
    public void saveTest() {
        ProductCategory productCategory = new ProductCategory("女生最爱", 11);
        ProductCategory result = repository.save(productCategory);
        Assert.assertNotNull(result);
//        Assert.assertNotEquals(null,result); 不期望为null,正确的为result
    }
```
## 用@JsonProperty来设置返回的json字段名
定义数据模型的时候，往往前端的需要字段名称和数据库定义的不同，比如 前端需要name，后台可能就是定义的是 productName，后端写name的话，很难区分这是哪跟哪，所以Spring里面的JsonProperty注解可以解决这个问题。(真的封装的太好了，哭唧唧)
```Java
package com.wechat.shopping.VO;

import com.fasterxml.jackson.annotation.JsonProperty;
import lombok.Data;

import java.util.List;

//data层
@Data
public class ProductVO {
    @JsonProperty("name")
    private String categoryName;

    @JsonProperty("type")
    private Integer categoryType;

    @JsonProperty("foods")
    private List<ProductInfoVO> productInfoVOList;
}
```
## 配置返回时间戳
springboot2.0后，spring会将时间自动给转成UTC字符串。
springboot1.x的版本返回的是时间戳。
在配置文件里配置一下就好了。
```Bash
spring
  jackson:
    serialization:
      write-dates-as-timestamps: true
```
## Swagger自动生成文档 返回404
哭死，被这个搞死了。返回错误
```Bash
No mapping found for HTTP request with URI [/swagger-ui.html] in DispatcherServlet with name 'dispatcherServlet'
```
说明资源没有映射上去。Spring Boot自动配置本身不会自动把/swagger-ui.html这个路径映射到对应的目录META-INF/resources/下面。我们加上这个映射即可。代码如下：
```Java
@Configuration
public class WebConfig extends WebMvcConfigurationSupport {


    @Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry.addResourceHandler("swagger-ui.html")
                .addResourceLocations("classpath:/META-INF/resources/");

        registry.addResourceHandler("/webjars/**")
                .addResourceLocations("classpath:/META-INF/resources/webjars/");
    }
｝
```
