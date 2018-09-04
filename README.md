# nioDemo
java NIO 学习

**java NIO( new IO) ,是从java 1.4开始进入的一个新的IO API，可以代替标准的java IO API**

NIO 的新的工作方式
- **channels and buffers(通道和缓冲区)：** 标准的IO 是基于字节流和字符流进行操作的，而NIO
是基于 channel(通道) 和 buffer(缓冲区)进行操作，数据总是从通道写入到缓冲区，或者从缓冲区写入到
通道

- **asynchronous IO(异步IO)：** java NIO 可以让我们异步的使用IO，例如：我们从通道读取数据到
缓冲区的时候，线程还可以做其它的事情，当数据被写入到缓冲区时，线程也可以处理其它的事情，数据从
缓冲区到 通道的时候也是一样。

- **Selectors（选择器）：** java NIO 引入了选择器的概念，选择器，可以用来监听多个通道的事件。
单个线程，可以监听多个通道


**Channel 和 BUffer**
基本上，所有的IO 都是从 channel 开始。Channel 有点像流，数据可以从Channel 中读取到 BUffer中，也
可以从 Buffer中写入到 Channel 中，如下图。
![Image text](http://dl2.iteye.com/upload/attachment/0096/3970/e20c73df-9ade-3121-be5f-307e6baf328f.png)