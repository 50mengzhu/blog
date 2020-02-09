---
title: 学习Spring —— 理解IOC（DI）
date: 2020-02-09 19:33:11
tags: spring
reward: true
---

<!-- toc -->



首先理解Spring作为一个框架，它解决的问题是什么？他是一个什么类型的框架？

<!-- more -->

> 实际上，我们不难发现。对于Spring来说，他其实提供了一整套的解决方案，查看Spring官网上给出的图片——
>
> ![See the source image](111732_x0O8_1249631.png)
>
> 我们根据以上的图片其实可以看到，Spring整体的框架中，包含着Test、Core、AOP、Data Access、Web这几个层面的框架。而对于Spring来说，最核心的实际上是IOC思想，采用DI的方式进行实现。

Spring框架解决了以下的几个问题：作为一个容器，这个容器里面存放的是各种Bean，通过IOC的方式在容器中进行托管；采用AOP的思想，理解不深，此处先留下一个坑
此外Spring框架实际上是一个整合型、设计型的框架。


想要使用Spring的核心功能，那么首先需要导入的包为——

> ```
> commons-logging
> org.springframework  spring-beans
> org.springframework  spring-context
> org.springframework  spring-core
> org.springframework  spring-expression
> ```

也就是上图中Core Container中的几个组件，此外还需要导入一个额外的日志包commons-logging用于日志记录。这个包将会被spring框架依赖。



## 普通java对象和Spring方式对对象操作的对比

### 手动管理java对象的方式

```java
class Person {
    private Integer id;
    private String  name;
    
    // --setter and getter
    
}
```



通过 new Person的方式进行对象创建，然后通过set方式进行属性赋值。



### 通过Spring框架创建对象

首先需要创建一个spring的配置文件xml，

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="person" class="com.dx.learn.Person">
        <property name="id" value="1001"></property>
        <property name="name" value="unix"></property>
    </bean>

    <bean id="person2" class="com.dx.learn.Person">
        <property name="id" value="1001"></property>
        <property name="name" value="unix"></property>
    </bean>
</beans>
```

通过创建一个bean的标签，在其中设置id和class属性，建立bean和java对象的对应关系。

然后在java代码中需要建立以下的代码进行创建。

```java
// 首先需要初始化容器
ApplicationContext context = new ClassPathXmlApplicationContext(xml位置);
// 通过这样的方式获取xml文件中对应的Bean文件
context.getBean();
```

关于getBean的三种重载——

+ 通过id获取java对象

需要注意的是，在Bean的配置当中属性id是唯一的，如果配置多个Bean采用相同的id，那么就会报出以下的错误。

+ 通过类名获取对象

&emsp;&emsp;其实我们从这里可以看到，使用类名获取对象的时候。我们不难联想到java中通过全类名获取java对象的方式就是java反射。事实上，Spring也确实是通过java反射的这种方式来创建对象的。

&emsp;&emsp;我们知道java反射一般采用的代码是按照如下的格式进行的——

```java
Class class = Class.forName(className);
Object object = class.newInstance(); // 只能调用无参构造函数
```

&emsp;&emsp;也就是说，反射一般会采用对象的无参构造方法。那么我们创建一个有参构造方法覆盖原本的默认构造方法，然后再次重新获取Bean，看看会发生什么——

```java
class Person {
    private Integer id;
    private String  name;
    
    // --setter and getter
 
    // add parameter constructor
    public Person(Integer id, String name) {
        this.id = id;
        this.name = name;
    }
}
```

&emsp;&emsp;然后再次执行代码，得到以下的错误——BeanCreationException 最后定位到的错误就是NoSuchMethodException，实际上就验证了Spring采用恰好是java反射的方式进行对象的构造。



+ 通过id和类名的方式获取对象

&emsp;&emsp;这个就是能够更加唯一的定位到Bean所对应的java对象。以上两种方式的结合。



## 理解DI（依赖注入）

&emsp;&emsp;理解一下依赖注入，实际上就是Spring管理java对象并向java对象进行赋值的操作。DI实际上就是IOC的一种实现。IOC算是一种思想，而DI则是IOC的一种基本的实现方式。



&emsp;&emsp;那么需要注入的值的类型有哪几种呢？根据java中的类型主要有以下几种——

+ 字面值

什么叫字面值的类型呢，在java中包含的八种基本类型及其包装类都是属于字面值。

+ 引用类型

这种类型就是我们自己创建的实体类对象。

+ 集合类型



&emsp;&emsp;那么DI的基本方式有哪些的呢？主要有几下几种方式——

### 方式一：通过set方法注入

&emsp;&emsp;这种方式主要是在配置文件中，在`bean`的子标签下通过`property`为代码属性赋值。

>  P.S. 注意这里的property中的name属性是通过类中set方法后面的字段来决定注入的，与类中的属性名称无关。看如下示例——

```java
public class Person {
    
    /** ID */
    private Integer idx;
    
    /** 姓名 */
    private String name;

    // ----- setter and getter --
    
    public Integer getId() {
        return idx;
    }

    public void setId(Integer id) {
        this.idx = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "Person[id=" + idx + ", name=" + name + "]";
    }

    public Person(Integer id, String name) {
        this.idx = id;
        this.name = name;
    }

    public Person() {
    }
}
```

然后在配置文件中按照以下的方式进行书写——

```xml
<bean id="person" class="包名.Person">
    <!-- 注意此处注入的name属性的值 -->
	<property name="id" value="1001"></property>
    <property name="name" value="unix"></property>
</bean>
```



>  P.S. 在Spring中可以采用以下的方式进行属性的注入——P命名空间，
>
> 首先需要引入p命名空间——即在配置文件的命名空间中的添加以下的代码——
>
> ```xml
> xmlns:p="http://www.springframework.org/schema/p"
> ```
>
> 然后就可以通过以下的方式进行属性的注入了——
>
> ```xml
> <bean id="testP" class="包名.User" p:id="1002" p:desc="hello"></bean>
> ```
>
> 

### 方式二：通过构造方法注入

&emsp;&emsp;还有一种为属性赋值的方法，就是通过构造方法为属性赋值。采用的方式是以下的方式——

```xml
<bean id="per" class="包名.Person">
	<constructor-arg value="11"></constructor-arg>
    <constructor-arg value="test"></constructor-arg>
</bean>
```

&emsp;&emsp;这种方式根据类型中对应的构造方法进行的操作，按照参数数量和参数的类型对构造方法进行匹配，完成属性值的注入。以下是对应的java实体的构造方法代码。

```java
public Person(Integer id, String name) {
	this.idx = id;
    this.name = name;
}
```



<hr/>

&emsp;&emsp;那么需要注入的值的类型有哪几种呢？根据java中的类型主要有以下几种——

- 字面值

&emsp;&emsp;什么叫字面值的类型呢，在java中包含的八种基本类型及其包装类都是属于字面值。

- 引用类型

&emsp;&emsp;这种类型就是我们自己创建的实体类对象。采用ref的方法就能实现创建。

> &emsp;&emsp;假如我们的Person类中存在一个自定义的Teacher类，如下：
>
> ```java
> public class Person {
> 
>  /** ID */
>  private Integer idx;
> 
>  /** 姓名 */
>  private String name;
> 
>  /** 教师实体 */
>  private Teacher teacher;
>  
>  ... 
> }
> ```
>
> &emsp;&emsp;那么因为在原生的java中出现这种情况是通过new Teacher然后set的方式进行元素赋值，而在Spring中对象的所有生命周期全部由Spring来管理。所以为了进行注入，我们采用的方式是让Spring管理一个新的Teacher对象，然后在配置文件中使用ref属性引用该对象。示例如下——
>
> ```xml
> <bean id="teacher" class="com.dx.learn.Teacher">
> 	<property name="tid">
>  	<value>1999</value>
> 	</property>
>  <property name="tName">
>  	<value>"huli"</value>
> 	</property>
> </bean>
> 
> <bean id="person" class="com.dx.learn.Person">
> 	<property name="id" value="1001"></property>
>  <property name="name" value="unix"></property>
>  <!-- 使用 ref 进行 teacher 的引用 -->
>  <property name="teacher" ref="teacher"></property>
> </bean>
> ```
>
> &emsp;&emsp;然后在Java中就可以通过person这个id获取到teacher的信息了。
>
> &emsp;&emsp;此外，还可以采用级联的方式修改teacher的值——配置文件如下——
>
> ```xml
> <bean id="person" class="com.dx.learn.Person">
> 	<property name="id" value="1001"></property>
>  <property name="name" value="unix"></property>
>  <property name="teacher" ref="teacher"></property>
>  <property name="teacher.tName" value="Name!"></property>
> </bean>
> ```
>
> &emsp;&emsp;此外还可以通过使用内部bean的方式对引用对象进行注入
>
> ```xml
> <bean id="person2" class="com.dx.learn.Person">
> 	<property name="id" value="10086"></property>
>     	<property name="name" value="10086"></property>
>     	<property name="teacher">
>     		<bean id="ttt" class="com.dx.learn.Teacher">
>         		<property name="tName" value="admin"></property>
>             	<property name="tid" value="110"></property>
>         	</bean>
>     	</property>
> </bean>
> ```
>
> **P.S. 需要格外注意的一点是，对于内部bean，只能被所属的那个bean调用，而不能被其他的类调用。**

+ 集合类型

&emsp;&emsp;主要是采用以下的方式进行值的注入采用的方法。因为集合主要有三种类型List、Set以及Map，那么针对这几种实现类型，应当怎样进行引用注入呢？

&emsp;&emsp;首先在Teacher中创建几个基本的类型——

```java
public class Teacher {
    
    /** 教师名称 */
    private Integer tid;

    /** 教师名称 */
    private String tName;

    /** 学生列表 */
    private List<Person> sList;

    /** 课程列表 */
    private Set<String> cls;

    /** 老师的领导 */
    private Map<String, String> bossMap;
    
    ...
}
```

&emsp;&emsp;针对以上的三种类型，采用的注入方式如下配置文件——

```xml
<bean id="teacher2" class="com.dx.learn.Teacher">
	<property name="tid" value="10010"></property>
    <property name="tName" value="test"></property>
    <property name="sList">
    	<list>
        	<ref bean="person"/>
            <ref bean="person2"/>
        </list>
    </property>
	<property name="cls">
    	<set>
        	<value>test</value>
		    <value>hash</value>
        </set>
    </property>
    <property name="bossMap">
    	<map>
        	<entry>
		        <key>
        			<value>100</value>
                </key>
                    <value>show</value>
                </entry>
                <entry>
                <key>
                	<value>101</value>
                </key>
                <value>show</value>
            </entry>
        </map>
    </property>
</bean>
```

&emsp;&emsp;除开以上的在property的子标签中创建集合元素外，还可以采用以下的方式能够创建集合类型的数据——此时我们需要引入一个新的命名空间util——

```xml
xmlns:util="http://www.springframework.org/schema/util"
<!-- 千万不要忘记了这个，否则会报 -->
xsi:schemaLocation="...
  http://www.springframework.org/schema/util
  http://www.springframework.org/schema/util/spring-util-4.0.xsd"
```

&emsp;&emsp;采用以下的方式进行配置——

```xml
<bean id="teacher3" class="com.dx.learn.Teacher">
	<property name="tid" value="10000"></property>
    <property name="tName" value="tea3"></property>
    <property name="sList" ref="list"></property>
    <property name="cls" ref="set"></property>
    <property name="bossMap" ref="map"></property>
</bean>

<util:list id="list">
	<ref bean="per"/>
    <ref bean="person"/>
    <ref bean="person2"/>
</util:list>

<util:set id="set">
	<value>chinese</value>
    <value>english</value>
    <value>math</value>
</util:set>

<util:map id="map">
	<entry>
    	<key>
        	<value>key1</value>
        </key>
        <value>value1</value>
    </entry>
    <entry>
    	<key>
        	<value>key2</value>
        </key>
        <value>value2</value>
    </entry>
</util:map>
```





## 工厂Bean

&emsp;&emsp;在Spring中存在一种特殊的Bean，这种bean类似于设计模式中的工厂模式，就是通过通过继承/实现FactoryBean这个接口创建一个工厂Bean，如下代码——

```java
public class Car {

    /** 汽车 id */
    private Integer id;

    /** 汽车品牌 */
    private String brand;

    /** 价格 */
    private Double price;
    
	... 
}
```

&emsp;&emsp;然后实现FactoryBean创建一个Factory类，如下——

```java
public class MyFactory implements FactoryBean<Car> {

    @Override
    public Car getObject() throws Exception {
        Car car = new Car();
        car.setBrand("BWM");
        car.setPrice(10000D);
        car.setId(2);
        return car;
    }

    @Override
    public Class<?> getObjectType() {
        return Car.class;
    }

    @Override
    public boolean isSingleton() {
        return false;
    }
}
```

&emsp;&emsp;根据上述我们学习到的Spring注入的方式，创建一个配置文件如下——

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="factory" class="com.dx.learn.factory.MyFactory"></bean>
</beans>
```

&emsp;&emsp;先等一会儿，按照之前的理解。按照以上的写法我们应该获取的是MyFactory这样对象，但实际上，打印出来的结果是Car对象。也就是说，在Spring中如果存在对于FactoryBean的注入，那么注入的对象实际上是工厂生产的对象实体类。




