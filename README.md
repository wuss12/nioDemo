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
---- 
![Image text](http://dl2.iteye.com/upload/attachment/0096/3970/e20c73df-9ade-3121-be5f-307e6baf328f.png)

Channel 和 Buffer 有多种类型，以下介绍主要的几种类型

* FileChannel 
* DatagramChannel
* SocketChannel
* ServerSocketChannel

以下是NIO中Buffer的常用实现
* ByteBuffer
* CharBuffer
* DoubleBuffer
* FloatBuffer
* IntBuffer
* LongBuffer
* ShortBuffer
这些Buffer 覆盖了常用的基本类型：byte,char,long,double ,float,int ,short

**Selector** 
select 允许单线程，处理多个select。如果你打开多个连接(通道)，但每个通道的流量都
很低，使用Select 就会很方便。如下图：
---
![](http://dl2.iteye.com/upload/attachment/0096/3972/79224e12-3615-3917-9e85-42e7edbd8b40.png)

要使用selector 首先需要往selector 注册Channel，然后调用他的Select()方法，这个方法会
一直阻塞到，注册的通道有事件就绪。一旦这个方法返回，线程就可以处理这些事件。


**java NIO  vs IO**
下面总结了NIO 和IO的主要区别

NIO|IO
---|---
Buffer oriented|Stream oriented
No Blocking IO|Blocking IO
Selectors| 


**NIO读取文件 解决中文乱码**

 ```
 private static StringBuilder readFromFile(String filePath) throws IOException {
        FileChannel fileChannel = FileChannel.open(Paths.get(filePath), StandardOpenOption.READ);

        ByteBuffer byteBuffer = ByteBuffer.allocate(1024);
        CharBuffer charBuffer = CharBuffer.allocate(1024);

        // 通过设置字符集的编码，并将ByteBuffer转换为CharBuffer来避免中文乱码
        Charset charset = Charset.forName("UTF-8");
        CharsetDecoder decoder = charset.newDecoder();

        StringBuilder sb = new StringBuilder((int) fileChannel.size());

        while (-1 != fileChannel.read(byteBuffer)){
            byteBuffer.flip();
            decoder.decode(byteBuffer, charBuffer, byteBuffer.limit() < 1024);

            charBuffer.flip();
//            System.out.println(charBuffer);
            sb.append(charBuffer);
            fileChannel.position(fileChannel.position()-byteBuffer.limit()+byteBuffer.position());
            byteBuffer.clear();
            charBuffer.clear();
        }
        fileChannel.close();
        return sb;
    }
    ```
