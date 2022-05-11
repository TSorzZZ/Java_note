# Spring5 框架

## 一、spring框架概述

__定义__：轻量级开源的JAVAEE框架，用于解决企业开发的复杂性

核心部分：

1) IOC(控制反转)：将创建对象的过程交给spring管理

2) AOP(面向切面编程): 不修改源代码进行功能增强

特点：

1) 方便解耦，简化开发

2) AOP编程支持

3) 方便程序测试（junit4）

4) 方便和其他框架整合(mybatis等)

5) 方便进行事务操作

6) 降低API开发难度

   ![image-20220506111438338](C:\Users\USER\AppData\Roaming\Typora\typora-user-images\image-20220506111438338.png)

## 二、IOC容器

<font color ='red'>1)IOC底层原理</font>

<font color ='red'>2)IOC接口(BeanFactory)</font>

<font color ='red'>3)IOC操作Bean管理(基于xml)</font>

<font color ='red'>4)IOC操作Bean管理(基于注解)</font>

### 2.1、 IOC底层原理

__什么是IOC？__

控制反转，把对象创建和对象之间的调用过程交给Spring进行管理。

目的：降低耦合度(**类的属性或方法如果改变，只需要更改xml配置文件**)

底层原理：IOC，工厂模式，注解

__IOC过程__

**第一步**：xml配置文件，配置创建的对象

```xml
<bean id="dao" class="com.atguigu.UserDao"></bean>
```

**第二步**：创建工厂类

```java
class UserFactory {
	public static UserDao getDao(){
        String classValue = class属性值; //1 xml解析获得类
        Class clazz = Class.forName(classValue)	//2 通过反射创建对象
        return (UserDao)clazz.newInstance(); //3 创建实例
    }
}
```



**IOC接口**

+ IOC基于IOC容器完成，IOC容器的底层就是**对象工厂**

+ Spring提供的IOC容器两种接口：

  1）BeanFactory：Spring内部使用的接口，不提供给开发人员使用

  **加载配置文件时不会创建对象，再获取对象时才会创建对象*

  2）ApplicationContext：BeanFactory接口的子接口，提供更多更强大的功能，一般由开发人员使用

  *加载配置文件时就会把配置文件对象创建(相当于初始化的时候就把耗时的工作先做了)

+ 查看接口或者类的结构（CTRL+H）

### 2.2、Bean管理(IOC具体操作)

Bean管理主要是指两个行为：1）Spring创建对象 2）Spring注入属性

Bean管理的两种方式：1）基于xml配置文件 2）基于注解的方式



#### **2.2.1、基于xml方式创建对象**

```xml
<bean id="user" class="com.atguigu.spring.User"></bean>
```

id：唯一标识

class：类全路径（包类路径）

**创建对象时，默认执行无参构造方法**



#### **2.2.2、基于xml方式注入属性**

DI(Dependency Injection)：依赖注入，

1）使用set方法进行注入

+ 创建类和对应的set方法

+ 在xml进行对象的创建和属性注入

  ```xml
  <bean id="book" class = "Book">
          <property name="bookname" value="sbsbsb"></property>
  </bean>
  ```

  

2）基于有参构造方法的注入

```xml
</bean>
    <bean id="book" class = "Book">
        <constructor-arg index="0" value="dineng"></constructor-arg>
        <constructor-arg name="bookname" value="dineng"></constructor-arg>
    </bean>
```



3）p空间注入（为了简化xml配置方式）

```xml
<beans xmlns:p="http://www.springframework.org/schema/p"> //添加p名称空间
    
    <bean id="book" class = "Book" p:bookname="book3"> //直接在bean标签中进行操作
    </bean>
    
</beans>
```



xml注入其他类型的属性

1）字面量

+ null值 

  ```xml
  <property name="address"><null/></property>
  ```

  

+ 属性值包含特殊符号









