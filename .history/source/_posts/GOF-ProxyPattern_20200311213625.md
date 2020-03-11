---
title: 今天也要学一点设计模式 —— 代理模式
date: 2020-03-11 21:33:48
reward: true
tags: GOF
---


代理模式

代理模式，顾名思义，实际上提供了一种访问目标对象的另外一种方式。也就是通过代理的方式去访问，这种模式的作用好处是 **在不改变原有对象的处理逻辑的基础之上，可以添加对原有对象的一些新的逻辑**。也就是可以扩展原有对象的功能。

<!-- toc -->

<!-- more -->

举个现实中常用的例子，我们平时生活中小饭店和外卖服务商家实际上是被代理者和代理者的例子，真正做饭的是饭店，而外卖则是拓宽了饭店的业务（外卖服务，同时也没有修改饭店做饭的逻辑）

三种代理模式的实现与解析

主要的实现思路都是在代理类中添加被代理类的对象作为自己的成员，然后向外部提供一个由被代理类为参数的构造方法，用于构造代理类。

### 2.1 静态代理

#### 2.1.1 使用条件

静态代理在使用的时候需要**代理类**和**被代理类**实现相同的接口或者父类，然后进行代理。

#### 2.1.2 代码实现

实例：麦当劳有自己的堂食服务和自己的外卖服务，其中外卖服务是堂食的代理类，而这两种服务都是麦当劳公司旗下的服务——

```java
/**
 * static proxy pattern McDonald interface
 *
 * @author mica
 */
public interface McDonald {

    /**
     * McDonald's business - cook for custom
     */
    void cook();

    /**
     * McDonald's send function
     */
    void sell();
}
```

堂食和外卖分别都是隶属于McDonald公司的一种服务吧，可以采用静态代理的方式进行代理——

```java
/**
 * McDonald's eat in service
 *
 * @author mica
 */
public class EatIn implements McDonald {

    /** log object */
    private static Log log = LogFactory.getLog(EatIn.class);

    @Override
    public void cook() {
        log.info("you can eat in! welcome to McDonald!");
    }

    @Override
    public void sell() {
        log.info("McDonald sell bed goods");
    }
}
```

外卖服务也是基于堂食的额外服务，需要添加额外配送费——

```java
/**
 * McDonald's takeaway service
 *
 * @author mica
 */
public class Takeaway implements McDonald {

    /** McDonald's eat in service object */
    private EatIn eatIn;

    /** log object */
    private static Log log = LogFactory.getLog(Takeaway.class);

    /** constructor  */
    public Takeaway(EatIn eatIn) {
        this.eatIn = eatIn;
    }

    @Override
    public void cook() {
        log.info("wait a few minutes! Delivered immediately");
        eatIn.cook();
        log.info("Please give me some tips! Thank you!");
    }

    @Override
    public void sell() {
        log.info("McDonald sell goods online!");
    }
}
```

> 注意：代理模式的精髓，就是在**代理类中**放置一个**被代理类**的对象，并且向外提供**一个以被代理类为参数的构造方法**。



按照以下的方式进行测试——

```java
/**
 * test eleme dynamic proxy
 *
 * @author mica
 */
public class TestTakeaway {

    @Test
    public void test() {
        EatIn eatIn = new EatIn();
        McDonald proxyEatIn = (McDonald) new Eleme(eatIn).getProxyObject();

        proxyEatIn.sell();
    }
}
```

#### 2.1.3 模式总结

##### 局限性：

静态代理最主要的局限性是代理类和被代理类都需要实现同一个接口，如下图——

![1583930181075](C:\Users\16700\AppData\Roaming\Typora\typora-user-images\1583930181075.png)

这样就将可以代理的类限制在很小的范围内，因此使用具有一定局限性。

##### 进步性：

这种代理模式，不需要借助其他的类，直接将被代理对象作为代理类的成员进行代理。使用简单



### 2.2 动态代理

#### 2.2.1 使用条件

被代理类必须实现一个接口，代理类通过实现固定接口InvocationHandler并实现invoke方法完成对被代理类的处理。

#### 2.2.2 代码示例

我们现在让eleme作为第三方去，配送McDonald的外卖，也就是由Eleme拓宽McDonald的堂食业务。

```java
/**
 * a takeaway company which provide takeaway service
 * like MEITUAN ELEME
 *
 * @author mica
 */
public class Eleme {

    /** log object */
    private static Log log = LogFactory.getLog(Eleme.class);

    /** proxy object, McDonald food can sold on ELEME */
    private EatIn eatIn;

    /**
     * constructor
     * @param eatIn McDonald's food
     */
    public Eleme(EatIn eatIn) {
        this.eatIn = eatIn;
    }

    /**
     * get proxy object using {@link InvocationHandler}
     *
     * @return target object
     */
    public Object getProxyObject() {
        return Proxy.newProxyInstance(eatIn.getClass().getClassLoader(),
                eatIn.getClass().getInterfaces(), (proxy, method, args) -> {
                       log.info("third party takeaway ELEME at your service!");
                       log.info(method.getName());
                       Object object = method.invoke(eatIn, args);
                       log.info("enjoy your food! please star 5 for me!");
                       return object;
                });
    }
}

```

McDonald的堂食代码已经在上面书写完成，以上的代码表示饿了么的替代业务，我上面采用的是lambda表达式书写的InvocationHandler类，可能看的不是很明确，Proxy.newProxyInstance第三个参数就需要传入一个InvocationHandler对象。动态代理则是通过实现InvocationHandler接口中的invoke方法完成对被代理类的代理操作。

#### 2.2.3 模式总结

##### 局限性：

虽然不需要代理类和被代理类都是实现一个同一个接口，但是代理类需要实现一个指定的接口InvocationHandler，通过实现这个类中的invoke方法完成对原有对象功能的扩展。而且被代理类必须自己也要实现一个接口。

![1583932635127](C:\Users\16700\AppData\Roaming\Typora\typora-user-images\1583932635127.png)

如果任性的你不给被代理类实现一个接口，那么将会得到以下的异常信息——

![1583804094380](C:\Users\16700\AppData\Roaming\Typora\typora-user-images\1583804094380.png)

##### 进步性：

打破静态代理只能代理同门师兄弟（实现同一个接口的类）的僵局，能够代理更多的类。



### 2.3 Cglib代理

#### 2.3.1 使用条件

对于一个普通的类，也能实现对他的代理。并且代理类和被代理类不需要实现同一个接口。但是需要注意的是被代理类需要实现MethodInterceptor接口，通过实现接口中的invoke方法对被代理类进行额外业务。

#### 2.3.2 代码实现

一家独立的小饭馆，不隶属于任何公司，就是自己开的一个小烧烤店。被外卖公司代理——

```java
/**
 * an independent restaurant
 * never belong to anyone
 *
 * @author mica
 */
public class IndependentRestaurant {

    /** log object */
    private static Log log = LogFactory.getLog(IndependentRestaurant.class);

    /**
     * an independent restaurant
     * cook food
     */
    public void cook() {
        log.info("independent restaurant at yor service!");
    }
}
```

代理类：——

```java
/**
 * a cglib proxy class
 *
 * @author mica
 */
public class CglibProxy implements MethodInterceptor {

    /** log object */
    private static Log log = LogFactory.getLog(CglibProxy.class);

    /** object to be proxy */
    private Object target;

    /**
     * constructor
     * @param object target
     */
    public CglibProxy(Object object) {
        this.target = object;
    }

    /**
     * generate an object for target
     * @return proxy object
     */
    public Object getProxyObject() {
        Enhancer enhancer = new Enhancer();
        enhancer.setSuperclass(target.getClass());
        enhancer.setCallback(this);
        return enhancer.create();
    }

    @Override
    public Object intercept(Object o, Method method, Object[] objects, MethodProxy methodProxy) throws Throwable {
        log.info("wait a minute!");
        Object object = method.invoke(target, objects);
        return object;
    }

```

测试类：——

```java
/**
 * test cglib
 *
 * @author mica
 */
public class TestCglib {

    @Test
    public void test() {
        IndependentRestaurant target = new IndependentRestaurant();
        IndependentRestaurant proxyObject = (IndependentRestaurant) new CglibProxy(target).getProxyObject();
        proxyObject.cook();
    }
}
```



#### 2.3.3 模式总结

##### 局限性：

相比起前面两种代理模式，这一种代理模式需要导入额外的包，cglib的相关包（或者spring-core包，spring-core包含了cglib的包）。而其余的两种直接通过使用JDK中已经存在的类就可以实现。

![1583932715491](C:\Users\16700\AppData\Roaming\Typora\typora-user-images\1583932715491.png)

代理类也需要实现MethodInterceptor

##### 进步性：

对静态代理和动态代理无法做到的一些场景进行补充。



