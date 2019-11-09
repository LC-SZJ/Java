[TOC]

# Java 网络编程

## 1. 网络编程入门

### 1.1 软件结构

- **C/S结构**：全称 Client / Server 结构，是指客户端和服务器结构。常见的有QQ、迅雷等软件。
- **B/S结构**：全称 Browser / Server 结构，是指浏览器和服务器结构。常见的有谷歌、火狐、IE等等。

两种架构各有优势，但是无论哪种架构，都离不开网络的支持。**网络编程**，就是在一定的协议下，实现两台计算机的通信的程序。

### 1.2 网络通信协议

- ***协议即为规则***
- **网络通信协议**：在计算机网络中，这些连接和通信的规则被称为网络通信协议。
- **TCP/IP协议**共分为四层，分别是：物理层、数据链路层 ，网络层，传输层，应用层。

### 1.3 协议分类

通信的协议还是比较复杂的，`java.net` 包中包含的类和接口，它们提供低层次的通信细节。我们可以直接使用这些类和接口，来专注于网络程序开发，而不用考虑通信的细节。

`java.net` 包中提供了两种常见的网络协议的支持：

- **UDP**：**用户数据报协议(User Datagram Protocol)**。UDP是**无连接通信协议**，即在数据传输时，数据的发送端和接收端不建立逻辑连接。简单来说，当一台计算机向另外一台计算机发送数据时，发送端不会确认接收端是否存在，就会发出数据，同样接收端在收到数据时，也不会向发送端反馈是否收到数据。

  由于使用**UDP协议消耗资源小，通信效率高**，所以通常都会用于**音频、视频和普通数据的传输**例如视频会议都使用UDP协议，因为这种情况即使偶尔丢失一两个数据包，也不会对接收结果产生太大影响。

  但是在使用UDP协议传送数据时，由于UDP的面向无连接性，不能保证数据的完整性，因此在传输重要数据时不建议使用UDP协议。UDP的交换过程如下图所示。

![UDP交换过程](https://ae01.alicdn.com/kf/H93c94394dbca4db7b7c8d019c1d5ac3dz.png)

   特点:数据被限制在64kb以内，超出这个范围就不能发送了。
   数据报(Datagram):网络传输的基本单位

- **TCP**：**传输控制协议 (Transmission Control Protocol)**。TCP协议是**面向连接的通信协议**，即传输数据之前，在发送端和接收端建立逻辑连接，然后再传输数据，它提供了两台计算机之间可靠无差错的数据传输。

  在TCP连接中必须要明确客户端与服务器端，由客户端向服务端发出连接请求，每次连接的创建都需要经过“三次握手”。

- 三次握手：TCP协议中，在发送数据的准备阶段，客户端与服务器之间的三次交互，以保证连接的可靠。
    - 第一次握手，客户端向服务器端发出连接请求，等待服务器确认。
    - 第二次握手，服务器端向客户端回送一个响应，通知客户端收到了连接请求。
    - 第三次握手，客户端再次向服务器端发送确认信息，确认连接。整个交互过程如下图所示。

![TCP三次握手过程](https://ae01.alicdn.com/kf/H64c40fd4083b44f2907061c72f465876v.png)

完成三次握手，连接建立后，客户端和服务器就可以开始进行数据传输了。由于这种面向连接的特性，TCP协议可以保证传输数据的安全，所以应用十分广泛，例如下载文件、浏览网页等。

### 1.4 网络编程三要素

#### 协议

- **协议**：计算机网络通信必须遵守的规则。

#### IP地址

- **IP地址：指互联网协议地址（Internet Protocol Address）**，俗称IP。IP地址用来给一个网络中的计算机设备做唯一的编号。假如我们把”个人电脑“比作“一台电话”的话，那么“IP地址“就相当于”手机号码“。

**IP地址分类**

- IPV4：是一个32位的二进制数，通常被分为4个字节，表示成`a.b.c.d`的形式，例如`192.168.65.100`。其中a、b、c、d都是 0~255之间的十进制整数，那么最多可以表示42亿个。
- IPV6：是一个128位的二进制数，每16个字节位一组，分成8组16进制数。号称可以世界上的每一粒沙子编上一个网址。

**常用命令**

- 查看本机的IP地址
	
	`ipconfig`
  
-  检查网络是否联通
`ping 空格 IP地址`

**特殊的IP地址**

- 本机的IP地址：`127.0.0.1`、`localhost`。

#### 端口号

  问题：我们必须保证数据能准确无误的发送到对方的计算机的指定软件上。
  解决：我们使用**IP地址+端口号**的方式就可以保证数据准确无误的发送到对方计算机的指定软件上。

- **端口号**：是一个逻辑端口，我们无法直接看到，可以一些软件查看端口号。
- 当我们的网络软件一打开的时候，那么操作系统就会为网络软件分配一个随机的端口号，或者网络软件在打开的时候向操作系统要指定的端口号。
- **端口号是由两个字节组成，取值范围在 0 ~ 65535 之间**

> 注意事项：
> **1024**之前的端口号我们不能使用，已经被系统分配给已知的网络软件了，网络软件的端口号不能重复。
> 

> 常用的端口号：
> 1. 80端口：网络端口	www.baidu.com:80	正确网址
> 2. 数据库：MySQL：3306； Oracle：1521
> 3. Tomcat服务器：8080
> 

## 2. TCP通信程序

### 2.1 概述

TCP通信能实现两台计算机之间的数据交互，通信的两端，要严格区分客户端（Client）和服务器端（Server）

**两端通信时步骤**：
1. 服务端程序，需要事先启动，等待客户端的连接。
2. 客户端主动连接服务器端，连接成功才能通信。服务器端不可以主动连接客户端。

TCP通信：面向连接的通信，客户端和服务器端必须经过三次握手，建立逻辑连接，才能通信（安全）。

通信的步骤：
  服务器端先启动，服务器端不会主动连接客户端，必须使用客户端请求服务器端，客户端和服务器端建立逻辑连接，而这个连接中包含一个对象，即**IO对象**，客户端和服务器端就可以使用IO对象进行通信，通信的数不仅仅是字符，所以IO对象是**字节流对象**。

客户端和服务器端进行一次数据交互，需要4个IO对象。


![TCP通信](https://ae01.alicdn.com/kf/H4766fcc5ffc64a3686dc4764d57a96b5q.png)


**服务器端必须明确的两件事情：**
1. 多个客户端同时和服务器端进行交互，服务器必须明确和哪个客户端进行的交互。
    在服务器端有一个方法，叫 accept，能获取到请求的客户端对象。
2. 多个客户端同时和服务器端进行交互，就需要使用多个IO流对象。
    服务器是没有 IO流的，服务器可以获取到请求的客户端对象 Socket，使用每个客户端的 Socket中提供的IO流和客户端进行交互。
    服务器使用客户端的字节输入流来读取客户端发生的数据。
    服务器使用客户端的字节输出流来给客户端回写数据。
    

**服务器使用客户端的流和客户端交互。**

### 2.2 Socket类

- TCP通信的客户端：向服务器发送连接请求，给服务器发送数据，读取服务器回写的数据。
- 表示客户端的类：
    - java.net.Socket：此类实现客户端套接字（也可以就叫“套接字”）。套接字是两台机器间通信的端点。
    - 套接字：包含了 IP地址和端口号的网络单位。

#### 构造方法

- Socket(String host, int port) ：创建一个流套接字并将其连接到指定主机上的指定端口号。
- 参数：
    - String host：服务器主机的名称 / 服务器的 IP地址。
    - int host：服务器的端口号。

####  成员方法

- OutputStream getOutputStream() ：返回此套接字的输出流。 
- InputStream getInputStream() ：返回此套接字的输入流。
- void close() ：关闭此套接字。

**实现步骤：**
1. 创建一个客户端对象Socket，构造方法中绑定服务器的IP地址和端口号。
2. 使用Socket对象中的方法 getOutputStream()，获取网络字节输出流OutputStream对象。
3. 使用网络字节输出流OutputStream对象中的方法 write，给服务器发送数据。
4. 使用Socket对象中的方法 getInputStream()，获取网络字节输入流InputStream对象。
5. 使用网络字节输出流InputStream对象中的方法 read，读取服务器回写的数据。
6. 释放资源（Socket）

> 注意事项：
> 1. 客户端和服务器端进行交互，必须使用 Socket中提供的网络流，不能使用字节创建的流对象。
> 2. 当我们创建客户端对象 Socket的时候，就会去请求服务器和服务器经过三次握手，建立连接通路。
> 	这时如果服务器没有启动，那么就会抛出异常**(ConnectException: Connection refused: connect)**。
> 	如果服务器已经启动，那么就可以进行交互了。
> 	

#### 代码实现（1）

```java
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.net.Socket;

/*
TCP通信的客户端
实现步骤：
    1.创建一个客户端对象Socket，构造方法中绑定服务器的IP地址和端口号
    2.使用Socket对象中的方法 getOutputStream，获取网络字节输出流对象
    3.使用网络字节输出流对象中的方法 write，给服务器发送数据
    4.使用Socket对象中的方法 getInputStream，获取网络字节输入流对象
    5.使用网络字节输入流对象中的方法，read，读取服务器回写的数据
    6.释放资源

 */
public class TCPClient {
    public static void main(String[] args) throws IOException {
        // 1.创建客户端对象，构造方法中绑定IP地址和端口号
        Socket socket = new Socket("127.0.0.1", 6666);
        // 2.使用Socket中的 getOutputStream，获取网络字节输出流对象
        OutputStream os = socket.getOutputStream();
        // 3.调用网络字节输出流对象的 writer方法，向服务器发送数据
        os.write("你好服务器".getBytes());
        // 4.使用Socket对象中的方法，getInputStream，获取网络字节输入流对象
        InputStream is = socket.getInputStream();
        // 5.使用网络字节输入流对象中的方法 read，读取服务器回写的数据
        byte[] bytes = new byte[1024];
        int len = is.read(bytes);
        System.out.println(new String(bytes, 0, len));
        // 6.释放资源(Socket)
        socket.close();
    }
}
```

### 2.3 ServerSocket类

- TCP通信的服务器端：接受客户端的请求，读取客户端发送的数据，给客户端回写数据
-  表示服务器的类：
    - java.net.ServerSocket:此类实现服务器套接字

#### 构造方法

- ServerSocket(int port):创建绑定到特定端口的服务器套接字。

服务器端必须明确一件事情：必须知道是哪个客户端请求的服务器
所以可以使用 accept方法获取请求到的客户端对象 Socket


#### 成员方法

- Socket accept():侦听并接受到此套接字的连接。


**服务器的实现步骤：**
1. 创建服务器ServerSocket对象,并向系统要指定的端口号
2. 使用ServerSocket对象中的方法，accept，获取请求到的客户端对象Socket
3. 使用Socket对象中的方法getInputStream，获取网络字节输入流对象
4. 使用网络字节输入流对象中的方法，read，读取客户端发送的数据
5. 使用Socket对象中的方法，getOutputStream，获取网络字节输出流对象
6. 使用网络字节输出流对象中的方法，write，回写给客户端数据
7. 释放资源(Socket, ServerSocket)

#### 代码实现（2）

```java
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.net.ServerSocket;
import java.net.Socket;

/*
TCP通信的服务器端：接受客户端的请求，读取客户端发送的数据，给客户端回写数据
表示服务器的类：
    java.net.ServerSocket:此类实现服务器套接字

    构造方法：
        ServerSocket(int port):创建绑定到特定端口的服务器套接字。
    服务器端必须明确一件事情：必须知道是哪个客户端请求的服务器
    所以可以使用 accept方法获取请求到的客户端对象 Socket

    成员方法：
         Socket accept():侦听并接受到此套接字的连接。
    服务器的实现步骤：
        1.创建服务器ServerSocket对象,并向系统要指定的端口号
        2.使用ServerSocket对象中的方法，accept，获取请求到的客户端对象Socket
        3.使用Socket对象中的方法getInputStream，获取网络字节输入流对象
        4.使用网络字节输入流对象中的方法，read，读取客户端发送的数据
        5.使用Socket对象中的方法，getOutputStream，获取网络字节输出流对象
        6.使用网络字节输出流对象中的方法，write，回写给客户端数据
        7.释放资源(Socket, ServerSocket)
 */
public class TCPServer {
    public static void main(String[] args) throws IOException {
        ServerSocket server = new ServerSocket(6666);
        Socket socket = server.accept();

        InputStream is = socket.getInputStream();
        byte[] bytes = new byte[1024];
        int len = is.read(bytes);
        System.out.println(new String(bytes, 0, len));

        OutputStream os = socket.getOutputStream();
        os.write("谢谢你".getBytes());

        socket.close();
        server.close();
    }
}
```


## 3. 综合案例

### 3.1 文件上传案例

#### 文件上传原理



![文件上传原理](https://ae01.alicdn.com/kf/Hd7f6d901e0a845b490af746af33d15ab1.png)

#### 客户端代码

```java
import java.io.FileInputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.net.Socket;

/*
    文件上传案例客户端：读取本地文件，上传到服务器，读取服务器回写的数据
    明确：
        数据源 D:\\aaa\\3.jpeg
        目的地 服务器
    实现步骤：
        1.创建本地字节输入流FileInputStream对象，构造方法中绑定读取数据的数据源
        2.创建一个客户端Socket对象，使用getOutputStream获取网络字节输出流OutputStream对象
        3.使用本地字节输入流对象中的方法read，读取本地文件
        4.使用网络字节输出流OutputStream对象中的方法，write,把文件上传到服务器
        5.使用Socket中的方法，getInputStream，获取网络字节输出流InputStream对象
        6.使用网络字节输出流InputStream对象中的方法，read读取服务器回写的数据
        7.释放资源(FileInputStream, Socket)
 */
public class FileUploadClient {
    public static void main(String[] args) throws IOException {
        // 1.创建本地字节输入流FileInputStream对象，构造方法中绑定读取数据的数据源
        FileInputStream fis = new FileInputStream("D:\\aaa\\3.jpeg");
        // 2.创建一个客户端Socket对象，使用getOutputStream获取网络字节输出流OutputStream对象
        Socket socket = new Socket("127.0.0.1", 8888);
        OutputStream os = socket.getOutputStream();
        // 3.使用本地字节输入流对象中的方法read，读取本地文件
        int len = 0;
        byte[] bytes = new byte[1024];
        while ((len = fis.read(bytes)) != -1) {
            // 4.使用网络字节输出流OutputStream对象中的方法，write,把文件上传到服务器
            os.write(bytes, 0, len);
        }
        // 解决
        socket.shutdownOutput();
        /*
            问题：
                fis.read(bytes)读取本地文件，while循环里不会读取到-1
                也不会把结束标记写给服务器
                服务器端读取客户端上传的数据时，永远也读取不到结束标记
                read方法处于堵塞状态，一直死循环等待结束标记

            解决：上传完文件，给服务器写一个结束标记
             void shutdownOutput()：禁用此套接字的输出流。
             对于 TCP 套接字，任何以前写入的数据都将被发送，并且后跟 TCP 的正常连接终止序列。

         */
        // 5.使用Socket中的方法，getInputStream，获取网络字节输出流InputStream对象
        InputStream is = socket.getInputStream();
        while ((len = is.read(bytes)) != -1) {
            // 6.使用网络字节输出流InputStream对象中的方法，read读取服务器回写的数据
            System.out.println(new String(bytes, 0, len));
        }
        // 7.释放资源(File Input Stream，Socket)
        fis.close();
        socket.close();
    }
}
```

#### 服务器端代码

```java
import java.io.*;
import java.net.ServerSocket;
import java.net.Socket;

/*
    文件上传案例服务器端：读取客户端上传的文件，保存到服务器的硬盘，给客户端回写“上传成功 ”
    明确：
        数据源：客户端上传的数据
        目的地：D:\\uplaod\\3.jepg
    实现步骤：
        1.创建服务器ServerSocket对象，向系统要指定的端口号
        2.使用ServerSocket对象中的方法，accept，获取当前请求的客户端Socket对象
        3.使用Socket对象中的方法，getInputStream，获取网络字节输入流InputStream对象
        4.判断 D:\\uplaod文件夹是否存在，不存在则创建
        5.创建本地字节输入流OutputStream对象，构造方法中绑定要写入数据的目的地
        6.使用网络字节输入流中的方法，read，读取客户端上传的数据
        7.使用本地字节输入流OutputStream对象中的方法，write，写入服务器的硬盘
        8.使用Socekt对象中的方法，getOutputStream，获取网络字节输出流OuputStream对象
        9.使用网络字节输出流对象中的方法，write，给客户端回写数据
        10.释放资源(File'OutputStream, Socket, ServerSocket)

 */
public class FileUploadServer {
    public static void main(String[] args) throws IOException {
//        1.创建服务器ServerSocket对象，向系统要指定的端口号
        ServerSocket server = new ServerSocket(8888);
//        2.使用ServerSocket对象中的方法，accept，获取当前请求的客户端Socket对象
        Socket socket = server.accept();
//        3.使用Socket对象中的方法，getInputStream，获取网络字节输入流InputStream对象
        InputStream is = socket.getInputStream();
//        4.判断 D:\\uplaod文件夹是否存在，不存在则创建
        File file = new File("D:\\upload");
        if (!file.exists()) {
            file.mkdirs();
        }
//        5.创建本地字节输入流OutputStream对象，构造方法中绑定要写入数据的目的地
        FileOutputStream fos = new FileOutputStream(file + "\\3.jpeg");
//        6.使用网络字节输入流中的方法，read，读取客户端上传的数据
        int len = 0;
        byte[] bytes = new byte[1024];
        while ((len = is.read(bytes)) != -1) {
//        7.使用本地字节输入流OutputStream对象中的方法，write，写入服务器的硬盘
            fos.write(bytes, 0, len);
        }
//        8.使用Socekt对象中的方法，getOutputStream，获取网络字节输出流OuputStream对象
//        9.使用网络字节输出流对象中的方法，write，给客户端回写数据
        socket.getOutputStream().write("上传成功".getBytes());
//        10.释放资源(FileOutputStream, Socket, ServerSocket)
        fos.close();
        socket.close();
        server.close();
    }
}
```

#### 对服务器端代码的优化

```java
import java.io.File;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.net.ServerSocket;
import java.net.Socket;
import java.util.Random;

/*
    文件上传案例服务器端：读取客户端上传的文件，保存到服务器的硬盘，给客户端回写“上传成功 ”
    明确：
        数据源：客户端上传的数据
        目的地：D:\\uplaod\\3.jepg
    实现步骤：
        1.创建服务器ServerSocket对象，向系统要指定的端口号
        2.使用ServerSocket对象中的方法，accept，获取当前请求的客户端Socket对象
        3.使用Socket对象中的方法，getInputStream，获取网络字节输入流InputStream对象
        4.判断 D:\\uplaod文件夹是否存在，不存在则创建
        5.创建本地字节输入流OutputStream对象，构造方法中绑定要写入数据的目的地
        6.使用网络字节输入流中的方法，read，读取客户端上传的数据
        7.使用本地字节输入流OutputStream对象中的方法，write，写入服务器的硬盘
        8.使用Socekt对象中的方法，getOutputStream，获取网络字节输出流OuputStream对象
        9.使用网络字节输出流对象中的方法，write，给客户端回写数据
        10.释放资源(File'OutputStream, Socket, ServerSocket)

 */
public class FileUploadServer {
    public static void main(String[] args) throws IOException {
        // 1.创建服务器ServerSocket对象，向系统要指定的端口号
        ServerSocket server = new ServerSocket(8888);
        /*
        优化 2:
        让服务器一直处于监听状态(死循环 accept方法)
        让一个客户端上传文件，就保存一个文件
         */
        while (true) {
            /*
            优化 3：
            使用多线程技术，提高程序的效率
            有一个客户端上传文件，就开启一个线程，完成文件的上传
             */
            new Thread(new Runnable() {
                // 开启线程任务,完成文件的上传
                @Override
                public void run() {
                    try { // Runnable接口中没有声明抛出异常,只能捕获异常
                        // 2.使用ServerSocket对象中的方法，accept，获取当前请求的客户端Socket对象
                        Socket socket = server.accept();
                        // 3.使用Socket对象中的方法，getInputStream，获取网络字节输入流InputStream对象
                        InputStream is = socket.getInputStream();
                        // 4.判断 D:\\uplaod文件夹是否存在，不存在则创建
                        File file = new File("D:\\upload");
                        if (!file.exists()) {
                            file.mkdirs();
                        }
                        /*
                        优化 1:
                        自定义一个文件的命名规则：防止同名的文件被覆盖
                        规则：域名+毫秒值+随机数
                         */
                        String filename = "itcast" + System.currentTimeMillis() + new Random().nextInt(999999) + ".jpeg";

                        // 5.创建本地字节输入流OutputStream对象，构造方法中绑定要写入数据的目的地
                        // FileOutputStream fos = new FileOutputStream(file + "\\3.jpeg");
                        FileOutputStream fos = new FileOutputStream(file + "\\" + filename);

                        // 6.使用网络字节输入流中的方法，read，读取客户端上传的数据
                        int len = 0;
                        byte[] bytes = new byte[1024];
                        while ((len = is.read(bytes)) != -1) {
                            // 7.使用本地字节输入流OutputStream对象中的方法，write，写入服务器的硬盘
                            fos.write(bytes, 0, len);
                        }
                        // 8.使用Socekt对象中的方法，getOutputStream，获取网络字节输出流OuputStream对象
                        // 9.使用网络字节输出流对象中的方法，write，给客户端回写数据
                        socket.getOutputStream().write("上传成功".getBytes());
                        // 10.释放资源(FileOutputStream, Socket, ServerSocket)
                        fos.close();
                        socket.close();

                    } catch (IOException e) {
                        System.out.println(e);
                    } finally {

                    }
                }
            }).start();
        }

        // 死循环，服务器就不用关闭了
        // server.close();
    }
}
```

### 3.2 模拟B / S服务器(学html的时候再回过头来看)


![模拟B/S服务器分析](https://ae01.alicdn.com/kf/H6100143fe23e4b8f960ff7e6c7778056E.png)


```java
import java.io.*;
import java.net.ServerSocket;
import java.net.Socket;

/*
    创建BS版本的TCP服务器
 */
public class BSTCPServer {
    public static void main(String[] args) throws IOException {
        // 创建一个服务器ServerSocket对象，和系统要指定的端口号
        ServerSocket server = new ServerSocket(8080);
        /*
            浏览器解析服务器回写的html页面，页面中如果有图片
            那么浏览器就会单独开启一个线程，读取服务器的图片
            我们就让服务器一直处于监听状态，客户端请求一个，服务器就会写一次
         */

        while (true) {
            // 使用accept方法获取请求到的客户端对象
            Socket socket = server.accept();
            new Thread(new Runnable() {
                @Override
                public void run() {
                    try {
                        // 使用socket对象中的方法，getInputStream，获取网络字节输入流InputStream对象
                        InputStream is = socket.getInputStream();
                        // 使用网络字节输入流中的方法，read，读取数据
                        /*int len = 0;
                        byte[] bytes = new byte[1024];
                        while ((len = is.read(bytes)) != -1) {
                            System.out.println(new String(bytes, 0, len));
                        }*/

                        // 把网络字节输入流对象，转换为字符缓冲输入流对象
                        BufferedReader br = new BufferedReader(new InputStreamReader(is));
                        // 把客户端请求信息的第一行读取出来，
                        String line = br.readLine();
                        // 把读取的数据进行切割，只要中间部分，
                        String[] arr = line.split(" ");
                        // 把路径前边的 / 去掉，进行截取
                        String htmlpath = arr[1].substring(1);

                        // 创建本地字节输入流对象，构造方法中绑定要读取的html路径
                        FileInputStream fis = new FileInputStream(htmlpath);

                        // 使用Socket中的getOutputStream方法，获取网络字节输出流对象
                        OutputStream os = socket.getOutputStream();

                        /*
                        学HTML的时候再学,固定的写法
                         */

                        os.write("HTTP/1.1 200 OK\r\n".getBytes());
                        os.write("Content-Type:text/html\r\n".getBytes());
                        os.write("\r\n".getBytes());

                        // 一读一写
                        int len = 0;
                        byte[] bytes = new byte[1024];
                        while ((len = fis.read(bytes)) != -1) {
                            os.write(bytes, 0, len);
                        }

                        // 释放资源
                        fis.close();
                        socket.close();

                    } catch (IOException e) {
                        e.printStackTrace();
                    }

                }
            }).start();
        }
    }
}
```