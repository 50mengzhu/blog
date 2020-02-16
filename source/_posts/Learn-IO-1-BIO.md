---
title: 学习Java IO和Netty 之 NIO
date: 2020-02-16 21:03:09
tags: Netty
reward: true
---

<!-- toc -->

&emsp;&emsp;Nio的核心概念主要有三个组件 —— Selector  Buffer   Channel

<!-- more -->

#### 图解三大组件

&emsp;&emsp;三个重要的组件可以结合以下的图形进行理解。

![1581846358920](1581846358920.png)

&emsp;&emsp;三大组件的关系如上图所示，

&emsp;&emsp;不用多说，Channel和Buffer是一对好兄弟，要想使用Channel进行数据的传输，那么必不可少的需要使用到Buffer（不然怎么能够实现异步操作）。

&emsp;&emsp;Selector实际上更像一个管理者，有一种翻译将Selector翻译为多路选择器，这是在说明它的功能和作用。此处先留下一个伏笔，等会在使用NIO创建一个Server端的流程中详细说说这个功能。



#### 使用NIO编程的具体流程

&emsp;&emsp;接下来我们看看在使用NIO的时候，是怎样创建以上三个组件？以及是怎样将以上三个组件联系在一起的？



&emsp;&emsp;那么使用NIO创建一个Server的流程如下——

```flow
st=>start: 开始
end=>end: 结束

step1=>operation: 首先需要创建一个多路选择器Selector，用于管理服务端的Channel
step2=>operation: 创建一个服务端的Channel，然后将这个Channel注册到多路选择器中
step3=>operation: 服务端的Channel对指定端口进行监听
step4=>operation: 当检测到客户端建立连接时，创建一个Channel与客户端进行通信
step5=>operation: 使用Channel对应的Buffer和客户端进行数据通信

st->step1
step1->step2
step2->step3
step3->step4
step4->step5
step5->end
```



&emsp;&emsp;在这里主要说一说Selector是怎样对注册在其中的Channel进行遍历的，在Selector这个类中有一个select方法，这个方法是专门用来选择出Selector中已经准备进行IO操作的所有的Channel。



#### 代码示例

服务端的代码如下所示——

```java
public class ServerSelectorDemo {

    /** 日志记录对象 */
    private static Log log = LogFactory.getLog(ServerSelectorDemo.class);

    public static void main(String[] args) {
        // 1. 首先创建一个服务端的 channel
        // 将服务器端 channel 绑定对应的端口，
        // 设置服务器端 channel 为非阻塞，并将其注册到对应的 selector 上

        // 2. 创建一个 选择器 selector 用于监听 channel 中的事件
        try (ServerSocketChannel serverSocketChannel = ServerSocketChannel.open();
             Selector selector = Selector.open()) {
            // 将serverSocketChannel的socket进行绑定并注册到selector中
            serverSocketChannel.socket().bind(new InetSocketAddress(SERVER_PORT));
            serverSocketChannel.configureBlocking(false);
            serverSocketChannel.register(selector, SelectionKey.OP_ACCEPT);

            // 3. 准备循环接收来自客户端的 连接
            while (true) {
                // 等待 1 ms 如果没有事件，那么就继续准备监听
                if (selector.select(TIMEOUT_THOUSAND) == 0) {
                    log.info(String.format("server has been waiting %dms", TIMEOUT_THOUSAND));
                    continue;
                }

                // 能进到这里说明已经监听到 channel 中存在事件
                // 获取所有的监听的事件，转换为迭代器类型便于遍历
                Set<SelectionKey> keySet = selector.selectedKeys();
                Iterator<SelectionKey> keyIterator = keySet.iterator();
                // 遍历所有的事件
                while (keyIterator.hasNext()) {
                    SelectionKey key = keyIterator.next();

                    if (key.isAcceptable()) {
                        // 通过 SelectionKey 和 channel 的对应关系完成处理
                        SocketChannel socketChannel = serverSocketChannel.accept();
                        socketChannel.configureBlocking(false);
                        socketChannel.register(selector, SelectionKey.OP_READ, ByteBuffer.allocate(1024));
                    }
                    if (key.isReadable()) {
                        SocketChannel socketChannel = (SocketChannel) key.channel();
                        // 获取 key 中的 buffer
                        ByteBuffer buffer = (ByteBuffer) key.attachment();
                        socketChannel.read(buffer);
                        log.info(new String(buffer.array(), StandardCharsets.UTF_8));
                        buffer.clear();
                    }

                    // 注意！完成之后需要将处理完成的 key 移除
                    keyIterator.remove();
                }
            }
        } catch (IOException e) {
            log.warn("open server socket channel failed!", e);
        }
    }
}

```



客户端的代码——

```java
public class ClientSelectorDemo {

    /** 日志记录对象 */
    private static Log log = LogFactory.getLog(ClientSelectorDemo.class);

    public static void main(String[] args) {
        try (SocketChannel socketChannel = SocketChannel.open()) {
            socketChannel.configureBlocking(false);

            // 如果链接不上就停止等待，不会造成阻塞
            if (!socketChannel.connect(new InetSocketAddress(HOST, SERVER_PORT))) {
                while (!socketChannel.finishConnect()) {
                    log.info("connect to server need server time");
                }
            }

            String greet = "Hello, World!~";
            ByteBuffer buffer = ByteBuffer.wrap(greet.getBytes(StandardCharsets.UTF_8));
            socketChannel.write(buffer);
        } catch (IOException e) {
            log.warn("create channel failed!", e);
        }
    }
}
```







#### 疑难杂症

关于select方法，我们需要知道的几点——

+ Selects a set of keys whose corresponding channels are ready for I/O operations.

+ 实际上，执行完select方法之后，会将所有已经准备进行IO操作的Channel对应的selectedKeys全部存放在Selector类中的selectedKeys对象中。然后通过SelectedKey的channel方法可以获取到对应的Channel。

+ <font color="red">不带参数的select方法</font>实际上是一个阻塞方法——This method performs a blocking selection operation.  It returns only after at least one channel is selected, this selector's wakeup method is invoked, or the current thread is interrupted, whichever comes first。那为什么说NIO是一种非阻塞的IO模型呢？实际上我这里也说了，是不带参数的select是一个阻塞方法，但是select还有其他的重载方法是不阻塞（不会永远阻塞）的，包括以下两种——

```java
/**
 * Selects a set of keys whose corresponding channels are ready for I/O
 * operations.
 *
 * <p> This method performs a blocking <a href="#selop">selection
 * operation</a>. It returns only after at least one channel is selected, this
 * selector's {@link #wakeup wakeup} method is invoked, the current thread is
 * interrupted, or the given timeout period expires, whichever comes first.
 *
 * <p>
 * This method does not offer real-time guarantees: It schedules the timeout as
 * if by invoking the {@link Object#wait(long)} method.
 * </p>
 *
 * @param timeout If positive, block for up to <tt>timeout</tt> milliseconds,
 *                more or less, while waiting for a channel to become ready; if
 *                zero, block indefinitely; must not be negative
 *
 * @return The number of keys, possibly zero, whose ready-operation sets were
 *         updated
 *
 * @throws IOException              If an I/O error occurs
 *
 * @throws ClosedSelectorException  If this selector is closed
 *
 * @throws IllegalArgumentException If the value of the timeout argument is
 *                                  negative
*/  
public abstract int select(long timeout) throws IOException; 

/**
 * Selects a set of keys whose corresponding channels are ready for I/O
 * operations.
 *
 * <p>
 * This method performs a non-blocking <a href="#selop">selection operation</a>.
 * If no channels have become selectable since the previous selection operation
 * then this method immediately returns zero.
 *
 * <p>
 * Invoking this method clears the effect of any previous invocations of the
 * {@link #wakeup wakeup} method.
 * </p>
 *
 * @return The number of keys, possibly zero, whose ready-operation sets were
 *         updated by the selection operation
 *
 * @throws IOException             If an I/O error occurs
 *
 * @throws ClosedSelectorException If this selector is closed
 */
public abstract int selectNow() throws IOException;
```



NIO解决了什么问题？

&emsp;&emsp;要想说明这个问题，我们首先看看BIO存在哪些问题，因为NIO就是为了优化BIO而产生的一种新的IO方式。因此我么首先看看BIO的缺点在哪——

+ 同步**阻塞**的IO模型，服务端实现模式是一个链接一线程（可以通过线程池来优化）
+ 在接受连接 和读取客户端接收数据时是**阻塞**的，如果没有数据那么就会导致**资源浪费**
+ 面向流编程（而且在BIO中**使用Stream**进行数据通信，**单向，效率较低**）
+ 采用多线程，操作系统**上下文切换**需要耗费很多资源

&emsp;&emsp;那么NIO解决了以上的问题——

+ 同步**非阻塞**的一种IO模型
+ 面向**块（缓存）**进行编程， Buffer可以进行**双向数据传输**
+ 将所有的Channel注册到Selector中，交由Selector进行管理，**单线程操作**，而且是由Channel中事件驱动的，**如果Channel没有任何事件那么Selector就回去做别的事情**



#### 写在后面的话

&emsp;&emsp;由上述的代码的书写中我们其实能够明显感受到，使用NIO编程和使用传统的IO编程的复杂度不是一个数量级的，虽然NIO很好用，但是却也导致代码量急剧增加，逻辑也变得复杂起来，因此在这时候Netty就随之诞生，于是这就是我们Netty出现前的宁静。

