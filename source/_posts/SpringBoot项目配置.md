---
title: SpringBoot项目配置
date: 2018-12-16 20:21:23
tags:
- Springboot
categories:
- Java
---
1. log日志
在resources目录下创建 logback-spring.xml文件，内容为
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<configuration>
    <appender name="consoleLog" class="ch.qos.logback.core.ConsoleAppender">
        <layout class="ch.qos.logback.classic.PatternLayout">
            <pattern>
                <!--设置输出格式-->
                %d - %msg%n
            </pattern>
        </layout>
    </appender>

    <appender name="fileInfoLog" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <filter class="ch.qos.logback.classic.filter.LevelFilter">
            <level>
                ERROR
            </level>
            <!--如果是ERROR则禁止-->
            <onMatch>DENY</onMatch>
            <!--不是ERROR就接受-->
            <onMismatch>ACCEPT</onMismatch>
        </filter>
        <encoder>
            <pattern>
                %msg%n
            </pattern>
        </encoder>
        <!--滚动策略-->
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <fileNamePattern>
                log/sell/info.%d.log
            </fileNamePattern>
        </rollingPolicy>
    </appender>

    <appender name="fileErrorLog" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <filter class="ch.qos.logback.classic.filter.ThresholdFilter">
            <level>
                ERROR
            </level>
        </filter>
        <encoder>
            <pattern>
                %msg%n
            </pattern>
        </encoder>
        <!--滚动策略-->
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <fileNamePattern>
                log/sell/error.%d.log
            </fileNamePattern>
        </rollingPolicy>
    </appender>
    <!--级别-->
    <root level="info">
        <appender-ref ref="consoleLog"/>
        <appender-ref ref="fileInfoLog"/>
        <appender-ref ref="fileErrorLog"/>
    </root>
</configuration>
```
2.Mysql数据库
在application.yml文件添加配置信息
```yml
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    username: root
      password: xxxx
      url: jdbc:mysql://localhost:3306/java_order?characterEncoding=utf-8&useSSL=false
    jpa:
      show-sql: true
      database-platform: org.hibernate.dialect.MySQL5InnoDBDialect
```
在maven引入响应的jar包
```xml
<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>

        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>
```
3.lombok（getter，setter）
引入
```xml
<dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>
```






