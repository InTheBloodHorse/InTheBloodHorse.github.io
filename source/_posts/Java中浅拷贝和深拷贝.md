---
title: Java中浅拷贝和深拷贝
date: 2019-04-29 15:10:49
categories:
- Java
---
## 什么是拷贝
Java中所有的类都默认继承了java.lang.Object类，而在java.lang.Object类中有一个方法clone()。
1. 拷贝对象返回的是一个新对象，而不是一个引用
2. 拷贝过程中并不会进行构造方法，而是新对象已经包含了原来对象的信息

## 实现Clone()方法
首先类实现Cloneable接口，另外就是重写clone()方法。
```Java
class Teacher implements Cloneable {
    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    protected Object clone() throws CloneNotSupportedException {
        return super.clone();
    }
}
```
## 浅拷贝
浅拷贝是指拷贝对象时仅仅拷贝对象本身（包括对象中的基本变量），而不拷贝对象包含的引用指向的对象。假设初始有类Person和类Teacher，在类Person中引用了类Teacher。现在我们拷贝（浅拷贝）了Person得到了 newPerson，此时Person和newPerson的引用都指向了同一个对象，即Teacher。当我们对Teacher进行操作，例如更改Teacher名字为"1ni"，那么Person和newPerson的getTeacherName()的结果都是"1ni"。
例如上文的 super.clone()就是简单的浅拷贝。
```Java
class Teacher implements Cloneable {
    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    protected Object clone() throws CloneNotSupportedException {
        return super.clone();
    }
}

class Person implements Cloneable {
    private String name;
    private Double score;
    private Teacher teacher;


    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Double getScore() {
        return score;
    }

    public Teacher getTeacher() {
        return teacher;
    }

    public void setTeacher(Teacher teacher) {
        this.teacher = teacher;
    }

    public void setScore(Double score) {
        this.score = score;
    }

    @Override
    protected Object clone() throws CloneNotSupportedException {
        return super.clone();
    }
}

public class ShallowCopy {

    public static void main(String[] args) throws CloneNotSupportedException {
        Teacher teacher = new Teacher();
        teacher.setName("yz");
        Person s1 = new Person();
        s1.setName("1ni");
        s1.setScore(55.5);
        s1.setTeacher(teacher);

        System.out.println("s1: " + s1.getName() + " " + s1.getScore());

        Person s2 = (Person) s1.clone();
        System.out.println("s2: " + s2.getName() + " " + s2.getScore());
        System.out.println("--------------------");
        s2.getTeacher().setName("新老师Tony");
        System.out.println("s1: " + s1.getName() + " " + s1.getTeacher().getName());
        System.out.println("s2: " + s2.getName() + " " + s2.getTeacher().getName());
    }
}
```
结果为
```Bash
s1: 1ni 55.5
s2: 1ni 55.5
--------------------
s1: 1ni 新老师Tony
s2: 1ni 新老师Tony
```

## 深拷贝
深拷贝不仅拷贝对象本身，而且拷贝对象包含的引用指向的所有对象
### 方法一
所以需要在Person中重写clone方法。对Person类中的Teacher对象进行一次拷贝
```Java
class Person implements Cloneable {
    private String name;
    private Double score;
    private Teacher teacher;


    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Double getScore() {
        return score;
    }

    public Teacher getTeacher() {
        return teacher;
    }

    public void setTeacher(Teacher teacher) {
        this.teacher = teacher;
    }

    public void setScore(Double score) {
        this.score = score;
    }

    @Override
    protected Object clone() throws CloneNotSupportedException {
        Person p = null;
        p = (Person) super.clone();
        p.setTeacher((Teacher) getTeacher().clone());
        return p;
    }
}

```
结果如下
```Bash
s1: 1ni 55.5
s2: 1ni 55.5
--------------------
s1: 1ni yz
s2: 1ni 新老师Tony
```

### 方法二
可以考虑将对象序列化，再反序列化得到结果。
```Java
package inthebloodhorse.copy;

import java.io.*;

class Student implements Serializable {
    private static final long serialVersionUID = 1L;
    String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}


class ClassRoom implements Serializable {
    private static final long serialVersionUID = 1L;
    Student student;

    public Student getStudent() {
        return student;
    }

    public void setStudent(Student student) {
        this.student = student;
    }

    public Object deepClone() throws IOException, ClassNotFoundException {
        // 写进流里
        ByteArrayOutputStream bos = new ByteArrayOutputStream();
        ObjectOutputStream oos = new ObjectOutputStream(bos);
        oos.writeObject(this);
        // 从流里读出来
        ByteArrayInputStream bi = new ByteArrayInputStream(bos.toByteArray());
        ObjectInputStream oi = new ObjectInputStream(bi);
        return oi.readObject();
    }
}

public class Copy2 {

    public static void main(String[] args) throws IOException, ClassNotFoundException {
        ClassRoom classRoom1 = new ClassRoom();
        Student student = new Student();
        student.setName("123");
        classRoom1.setStudent(student);
        ClassRoom classRoom2 = (ClassRoom) classRoom1.deepClone();
        classRoom1.getStudent().setName("1ni");
        System.out.println(classRoom1.getStudent().getName());
        System.out.println(classRoom2.getStudent().getName());
    }
}
```
结果如下
```Bash
教室一的学生:1ni
教室二的学生:123
```


