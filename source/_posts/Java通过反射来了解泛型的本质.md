---
title: Java通过反射来了解泛型的本质
date: 2018-10-14 12:02:44
categories:
- Java
---
## 什么是反射
Java的反射机制是在运行状态中，对于任何一个类，都能知道这个类的所有属性和方法，对于任意一个对象，都能够调用他的任意一个方法和属性；这种动态获取的信息以及动态调用对象的方法的功能称之为java语言的反射机制。反射的操作都是编译之后的操作。
## 创建类的三种方式
先定义一个类，方便测试
```Java
package cn.pyking;

class A{
	public void print() {
		System.out.println("hello");
	}
}
```
### A.class
```Java
Class class1 = A.class;
System.out.println(class1.getName());
```

### A.getClass()
```Java
Class class2 = a.getClass();
System.out.println(class2.getName());
```
### Class.forName()
```Java
Class class3  = null;
try {
	class3 = Class.forName("cn.pyking.A");
} catch (Exception e) {
	e.printStackTrace();
}
System.out.println(class3.getName());
```
一般用第三种方法，传入字符串即可，可以在配置中设置，通用性好。
### 如何使用
创建了类之后，我们就可以用newInstance()方法来创建该对象。（注意强制类型转换）
```Java
try {
	A a1 = (A)class3.newInstance();
	a1.print();
}catch (Exception e) {
	e.printStackTrace();
}
```

总代码
```Java
package cn.pyking;


public class Reflect {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		A a = new A();
		System.out.println(a.getClass());
		
		//1
		Class class1 = A.class;
		System.out.println(class1.getName());
		//2
		Class class2 = a.getClass();
		System.out.println(class2.getName());
		//3
		Class class3  = null;
		try {
			class3 = Class.forName("cn.pyking.A");
		} catch (Exception e) {
			e.printStackTrace();
		}
		System.out.println(class3.getName());
		
		try {
			A a1 = (A)class3.newInstance();
			a1.print();
		}catch (Exception e) {
			e.printStackTrace();
		}
		
	}

}

class A{
	public void print() {
		System.out.println("hello");
	}
}
```

## 泛型
重点来了。那就是泛型，了解过java的都知道泛型，即"参数化类型"。
先看下面的代码，我们先新建两个ArrayList，一个没有指定类型。一个指定了String类型。然后获取他们的类类型，比较
```Java
ArrayList list = new ArrayList();
ArrayList<String> sList = new ArrayList<String>();
//		sList.add(20); //wrong
Class class1 = list.getClass();
Class class2 = sList.getClass();
System.out.println(class1 == class2);
```
正常想法应该是打印false，因为我一个指定了泛型String，而另一个没有，肯定是不一样的。但是我们错了，两者是一样的，什么概念？就是其实编译结束之后并没有String不String的操作，编译之后集合是去泛型化的。既然这样，那么我们就可以通过反射绕过编译，给ArrayList添加一个原本并不期待的元素。
这里要提到 getMethod，invoke方法，找下相关资料即可。
```Java
try {
	Method method = class2.getMethod("add", Object.class);
	method.invoke(sList,100);
	System.out.println(sList.size());
	System.out.println(sList);
} catch (Exception e) {
	e.printStackTrace();
}
```
顺便提一下，我们还是不能把它输出出来了。因为还是要输出String类型的。
总代码
```Java
package cn.pyking;

import java.lang.reflect.Method;
import java.util.ArrayList;

public class ArrList {

	public static void main(String[] args) {
		ArrayList list = new ArrayList();
		ArrayList<String> sList = new ArrayList<String>();
//		sList.add(20); //wrong
		Class class1 = list.getClass();
		Class class2 = sList.getClass();
		sList.add("hello");
		System.out.println(class1 == class2);
		
		try {
			Method method = class2.getMethod("add", Object.class);
			method.invoke(sList,100);
			System.out.println(sList.size());
			System.out.println(sList.get(1));
		} catch (Exception e) {
			e.printStackTrace();
		}
	}

}

```



