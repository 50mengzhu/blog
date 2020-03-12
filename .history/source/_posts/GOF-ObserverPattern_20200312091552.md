---
title: 今天也要学一点设计模式 —— 观察者模式
date: 2020-03-12 09:15:04
tags:
---


观察者模式是我们在设计中常用的一种模式，在这种模式下，消息发送者和消息接收者相当于一种订阅号的模式，类似于我们在日常中使用的微信公众号功能，由公众号下发一篇文章，然后所有关注了该公众号的用户都能接收到这条信息。

### 原理逻辑

观察者模式的实现原理图如下：

![1583974793262](C:\Users\16700\AppData\Roaming\Typora\typora-user-images\1583974793262.png)

虽然观察者模式涉及到很多类，

<font color="red">消息提供者接口</font>则是提供一些关于消息接收者的**注册**、**取关**、**发送消息**等接口方法。

<font color="red">消息接收者接口</font>提供一个**消息处理**方法。

但其实观察者模式所有的秘密全部集中在消息提供者的实现类中，实现类将消息提供者和消息接收者关联起来。

> 这个实现类主要做了以下的工作：
>
> + 维护一个消息接收者的容器（Collection、数组等），这个就是用来管理所有订阅者的秘密
> + 在发送消息的时候，遍历所有订阅者，调用他们接口的方法，完成对信息的处理



### 代码示例

消息发送者，接口

```java
/**
 * message provider of observer pattern
 *
 * @author mica
 */
public interface MessageProvider {

    /**
     * register new receiver to MessageProvider
     * @param receiver message receiver
     */
    void register(MessageReceiver receiver);

    /**
     * remove receiver, the receiver will never receive
     * messaege from provider unless it register to provider
     * @param receiver message receiver
     */
    void removeListener(MessageReceiver receiver);


    /**
     * send message to everyone who register to provider
     * dispatch message
     * @param message message to send
     */
    void notify(String message);
}
```

消息发送者，实现类

```java
/**
 * MessageProvider implements
 *
 * @author mica
 */
public class MessageProviderImpl implements MessageProvider{

    /** object to store message receiver (only registered) */
    private Set<MessageReceiver> receivers = new HashSet<>();

    @Override
    public void register(MessageReceiver receiver) {
        receivers.add(receiver);
    }

    @Override
    public void removeListener(MessageReceiver receiver) {
        receivers.remove(receiver);
    }

    @Override
    public void notify(String message) {
        for (MessageReceiver receiver : receivers) {
            receiver.receive(message);
        }
    }
}
```

在实现类中，通过一个Set维护消息接收者的容器，然后在消息发送者发送消息的时候遍历Set集合中的所有元素，并调用他们的receive方法，以此完成消息接收端“自动”收取到了信息，或者说是消息接收端“自动”执行了receive方法。



消息接收者

```java
/**
 * message (from message provider) receiver
 *
 * @author mica
 */
public interface MessageReceiver {

    /**
     * receive message from provider
     *
     * @param message message to receive
     */
    void receive(String message);
}
```





