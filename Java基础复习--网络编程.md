#Java基础复习--网络编程



#网络编程

##网络模型
TCP/IP协议中的四层分别是应用层、传输层、网络层和链路层，每层分别负责不同的通信功能

[![](https://ae01.alicdn.com/kf/Hc3df5c1de00b46698782de062eb8b47bU.png)](https://ae01.alicdn.com/kf/Hc3df5c1de00b46698782de062eb8b47bU.png)

**链路层：**链路层是用于定义物理传输通道，通常是对某些网络连接设备的驱动协议，例如针对光纤、网线提供的驱动。

**网络层：**网络层是整个TCP/IP协议的核心，它主要用于将传输的数据进行分组，将分组数据发送到目标计算机或者网络。

**传输层：**主要使网络程序进行通信，在进行网络通信时，可以采用TCP协议，也可以采用UDP协议。

**应用层：**主要负责应用程序的协议，例如HTTP协议、FTP协议等。

##IP协议
[![](https://ae01.alicdn.com/kf/Hc4902475653e43b0acdba247f86e3226m.png)](https://ae01.alicdn.com/kf/Hc4902475653e43b0acdba247f86e3226m.png)
每个人的电脑都有一个独一无二的IP地址，这样互相通信时就不会传错信息了。

　　IP地址是用一个点来分成四段的，在计算机内部IP地址是用四个字节来表示的，一个字节代表一段，每一个字节代表的数最大只能到达255。
　　
　目前，IP地址广泛使用的版本是 IPv4，它是由4个字节大小的二进制数来表示，如：00001010000000000000000000000001。
由于二进制形式表示的IP地址非常不便记忆和处理，因此通常会将IP地址写成十进制的形式， 每个字节用一个十进制数字(0-255)表示，数字间用符号“.”分开，如 “192.168.1.100”
127.0.0.1 为 本地主机地址(本地回环地址)
DOS命令 ipconfig：查看本机IP地址

##TCP协议和UDP协议

[![](https://ae01.alicdn.com/kf/H4272e8fcedfd4f1e9c71ba187b6bd1b5d.png)](https://ae01.alicdn.com/kf/H4272e8fcedfd4f1e9c71ba187b6bd1b5d.png)

TCP和UDP位于同一层，都是建立在IP层的基础之上。由于两台电脑之间有不同的IP地址，因此两台电脑就可以区分开来，也就可以互相通话了。通话一般有两种通话方式：第一种是TCP，第二种是UDP。

**TCP是可靠的连接，TCP就像打电话，需要先打通对方电话，等待对方有回应后才会跟对方继续说话，也就是一定要确认可以发信息以后才会把信息发出去。
TCP上传任何东西都是可靠的，只要两台机器上建立起了连接，在本机上发送的数据就一定能传到对方的机器上，UDP就好比发电报，发出去就完事了，对方有没有接收到它都不管，所以UDP是不可靠的。**

TCP传送数据虽然可靠，但**传送得比较慢**，UDP传送数据不可靠，但是**传送得快**。


##TCP协议

TCP协议是**面向连接**的通信协议，即在传输数据前先在发送端和接收端建立逻辑连接，然后再传输数据，它提供了两台计算机之间可靠无差错的数据传输。

在TCP连接中必须要明确客户端与服务器端，由客户端向服务端发出连接请求，每次连接的创建都需要经过**“三次握手”**。

    第一次握手，客户端向服务器端发出连接请求，等待服务器确认 
    第二次握手，服务器端向客户端回送一个响应，通知客户端收到了连接请求 
    第三次握手，客户端再次向服务器端发送确认信息，确认连接

###HTTP
**HTTP就是一个用文本格式描述报文头并用双换行分隔报文头和内容，在TCP基础上实现的请求-响应模式的双向通信协议。**

基于TCP协议的一个报文协议，其报文头是不定长且任意扩展的，这也使得这个协议充满了生命力。


**没有人能完整描述HTTP协议**，因为这个协议的细节可以随时扩充。例如控制咖啡壶什么的……其实你说的越多，暴露的短处越多，所以什么都不说才是坠吼的。


####3次握手4次分手
参考：https://www.cnblogs.com/chenkeyu/p/8485412.html

个人理解：
3次握手：
客户端发出第一次请求----》服务端接收到，要告诉客户端：我接受到了，返回响应给客户端。
客户端接收到响应后，为了确保正确创建连接，再发出一次请求-----》服务端收到，此时完成3次握手。

握手是 建立连接的3次传递，2次由客户端发出，1次由服务端发出。

4次分手：
**TCP是双向的，所以需要在两个方向分别关闭，每个方向的关闭又需要请求和确认，所以一共就4次。**
客户端发出断开请求----》服务端收到，返回响应。
服务端发出断开请求----》客户端收到，返回响应。



###Sockect
[![](https://ae01.alicdn.com/kf/H9e071e8e97094f0e9fdd5cf0e426be88a.png)](https://ae01.alicdn.com/kf/H9e071e8e97094f0e9fdd5cf0e426be88a.png)

Socket的读写过程
[![](https://ae01.alicdn.com/kf/H9f2b216c9d60459895e25bedb72688f6E.gif)](https://ae01.alicdn.com/kf/H9f2b216c9d60459895e25bedb72688f6E.gif)

**出现Sockect的原因：**
TCP通信同UDP通信一样，都能实现两台计算机之间的通信，通信的两端都需要创建 socket 对象。
区别在于，UDP中只有 发送端 和 接收端，不区分 客户端 与 服务器 端，计算机之间可以任意地发送数据。
而TCP通信是严格区分客户端与服务器端的，在通信时，必须先由客户端去连接服务器端才能实现通信，服务器端不可以主动连接客户端，并且服务器端程序需要事先启动，等待客户端的连接。
在JDK中提供了两个类用于实现TCP程序，一个是 ServerSocket 类，用于表示服务器端，一个是 Socket 类，用于表示客户端。
通信时，首先创建代表服务器端的 ServerSocket 对象，该对象相当于开启一个服务，并等待客户端的连接，然后创建代表客户端的 Socket对象向服务器端发出连接请求，服务器端响应请求，两者建立连接开始通信。

**一个socket连接如何唯一标识？** 

    源端（ip+端口号）、目的（ip+端口号）唯一确定一个tcp连接


####TCP的客户端程序

实现TCP客户端，连接到服务器和服务器实现数据交换
实现TCP客户端程序的类： java.net.Socket

构造方法：
**Socket(String host, int port) ：**传递服务器IP和端口号

    构造方法只要运行,就会和服务器进行连接,连接失败就抛出异常


**OutputStream getOutputStream() ：**返回套接字的输出流。
作用： 将数据输出,输出到服务器

**InputStream getInputStream()** 返回套接字的输入流；
作用： 从服务器端读取数据


客户端服务器数据交换，**必须使用套接字对象 Socket 中的获取的IO流**，自己 new 的流是不行的


代码：
```
public class TCPClient {
    public static void main(String[] args) throws IOException {
        //创建Socket对象，连接服务器
        Socket socket = new Socket("127.0.0.1", 8888);
        //通过客户端的套接字对象 Socket方法，获取字节输出流将数据写向服务器
        OutputStream out = socket.getOutputStream();
        out.write("服务器ok！".getBytes());

        //读取服务器发回的数据，使用socket套接字对象中字节输入流
        InputStream in = socket.getInputStream();
        byte[] data = new byte[1024];
        int len = in.read(data);
        System.out.println(new String(data,0,len));

        socket.close();
    }
}
```

####TCP的服务器程序 & accept 方法

表示服务器程序的类： java.net.ServerSocket
构造方法： 
ServerSocket(int port)： 传递端口号

**必须要获得客户端的套接字对象 Socket**：Socket accept()



代码：
```
public class TCPServer {
    public static void main(String[] args) throws IOException {
        ServerSocket service = new ServerSocket(8888);
        //调用服务器套接字对象中的方法accpet() ，获取客户端套接字对象
        Socket socket = service.accept();
        //通过客户端套接字对象Socket，获取字节输入流，读取客户端发送来的消息
        InputStream in = socket.getInputStream();
        byte[] data = new byte[1024];
        int len = in.read(data);
        System.out.println(new String(data,0,len));

        //服务器向客户端回数据，字节输出流，通过客户端套接字对象获取字节输出流
        OutputStream out = socket.getOutputStream();
        out.write("收到，谢谢！！".getBytes());

        socket.close();
        service.close();
    }
} 
```


##UDP通信

###**数据包和发送对象**
**DatagramPacket数据包** 的作用就如同是“集装箱”，可以将发送端或者接收端的数据封装起来。然而运输货物只有“集装箱”是不够的，还需要有码头。
在程序中需要实现通信只有 DatagramPacket数据包也同样不行，为此JDK中提供的一个DatagramSocket类。
**DatagramSocket类** 的作用就类似于码头，使用这个类的实例对象就可以 **发送和接收DatagramPacket数据包**
DatagramPacket：**封装数据**
DatagramSocket：**发送 DatagramPacket**

###UDP发送端


**实现 UDP 协议的发送端**
实现**封装数据**的类 java.net.DatagramPacket 将数据包装
实现**数据传输**的类 java.net.DatagramSocket 将数据包发出去


**实现步骤**

 1. 创建 DatagramPacket 对象,封装数据, 接收的地址和端口
 2. 创建 DatagramSocket 对象
 3. 调用 DatagramSocket 类方法 send ，发送数据包
 4. 关闭资源


DatagramPacket构造方法：

    DatagramPacket(byte[] buf, int length, InetAddress address, int port)
    参数：字节数组，发送多少，IP地址，端口号

DatagramSocket构造方法:

    DatagramSocket()：空参数

DatagramSocket的方法: 

    send(DatagramPacket d)


代码:
```
public class UDPSend {
    public static void main(String[] args) throws IOException{
        //创建数据包对象,封装要发送的数据,接收端IP,端口 
        byte[] date = "你好UDP".getBytes();
        //创建InetAddress对象,封装自己的IP地址 
        InetAddress inet = InetAddress.getByName("127.0.0.1");
        DatagramPacket dp = new DatagramPacket(date, date.length, inet,6000);
        //创建DatagramSocket对象,数据包的发送和接收对象 
        DatagramSocket ds = new DatagramSocket();
        //调用ds对象的方法send,发送数据包 
        ds.send(dp);
        //关闭资源 
        ds.close();
    }
}
```

###UDP接收端


**实现UDP接收端**
实现**封装数据包**java.net.DatagramPacket 将数据**接收**
实现**数据传输**java.net.DatagramSocket 接收数据包


**实现步骤**

 1. 创建 DatagramSocket 对象，**绑定端口号**，要和**发送端端口号一致**
 2. 创建**字节数组**，接收发来的数据
 3. 创建数据包对象 DatagramPacket
 4. 调用 DatagramSocket 对象方法，receive(DatagramPacket dp)接收数据，数据放在数据包中
 5. 拆包：
 
    5.1. **发送的IP地址** ：数据包对象 DatagramPacket 方法getAddress() 获取的是**发送端的IP地址对象** ，返回值是 InetAddress对象
    5.2. **接收到的字节个数**：数据包对象 DatagramPacket 方法 getLength()
    5.3. **发送方的端口号**：数据包对象 DatagramPacket 方法 getPort()发送端口

6.**关闭资源**


代码：
```
public class UDPReceive {
    public static void main(String[] args)throws IOException {
        //创建数据包传输对象DatagramSocket 绑定端口号
        DatagramSocket ds = new DatagramSocket(6000);
        //创建字节数组
        byte[] data = new byte[1024];
        //创建数据包对象,传递字节数组
        DatagramPacket dp = new DatagramPacket(data, data.length);
        //调用ds对象的方法receive传递数据包
        ds.receive(dp);

        //获取发送端的IP地址对象
        String ip=dp.getAddress().getHostAddress();

        //获取发送的端口号
        int port = dp.getPort();

        //获取接收到的字节个数
        int length = dp.getLength();
        System.out.println(ip+" "+port+":"+new String(data,0,length));
        ds.close();
    }
}
```


#TCP图片上传案例

##TCP上传客户端
实现步骤:

 1. Socket套接字连接服务器
 2. 通过Socket获取字节输出流,写图片 
 3. 使用自己的流对象,读取图片数据源(FileInputStream、缓冲流）
 4. 读取图片,使用字节输出流,将图片写到服务器（采用字节数组进行缓冲）
 5. 通过Socket套接字获取字节输入流，读取服务器发回来的上传成功，
 6. 关闭资源

代码:
```
public class TCPClient {
    public static void main(String[] args) throws IOException {
        Socket socket = new Socket("127.0.0.1",8000);
        //获取字节输出流，将图片写到服务器
        OutputStream out = socket.getOutputStream();
        //创建字节输入流，读取本机上的数据源图片
        FileInputStream fis = new FileInputStream("D:\\test.jpg");
        //开始读写字节数组
        int len = 0;
        byte[] bytes = new byte[1024];
        while((len = fis.read(bytes)) != -1){
            out.write(bytes,0,len);
        }

        //给服务器写终止序列,向服务端写入一个结束标志
        socket.shutdownOutput();

        //获取字节输入流，读取服务器的"上传成功"
        InputStream in = socket.getInputStream();

        len = in.read(bytes); //复用byte数组
        System.out.println(new String(bytes,0,len));

        fis.close();
        socket.close();
    }
}
```

##TCP上传服务器

 1. ServerSocket套接字对象,监听端口8000
 2. 方法accept()获取客户端的连接对象
 3. 客户端连接对象获取字节输入流,读取客户端发送图片
 4. 创建File对象，绑定上传文件夹（判断文件夹存在，不存在，创建文件夹）
 5. 创建字节输出流，数据目的File对象所在文件夹
 6. 字节流读取图片，字节流将图片写入到目的文件夹中
 7. 将上传成功会写客户端
 8. 关闭资源

代码：
```
public class TCPServer {
    public static void main(String[] args) throws IOException {
        ServerSocket server = new ServerSocket(8000);
        Socket socket = server.accept();
        //通过客户端连接对象,获取字节输入流,读取客户端图片
        InputStream in = socket.getInputStream();
        //将目的文件夹封装到File对象
        File upload = new File("E:\\upload");
        if(! upload.exists()){
            upload.mkdirs();
        }

        //防止文件同名被覆盖,重新定义文件名字
        //规则:  域名+当前毫秒值+6位随机数
        String filename="wangdao"+System.currentTimeMillis()+new Random().nextInt(999999)+".jpg";

        //创建字节输出流,将图片写入到目的文件夹中
        FileOutputStream fos = new FileOutputStream(upload+File.separator+filename);
        //读写字节数组
        byte[] bytes = new byte[1024];
        int len = 0;
        while((len = in.read(bytes)) != -1){
            fos.write(bytes,0,len);
        }

        //通过客户端连接对象获取字节输出流
        //将"上传成功"写回客户端
        socket.getOutputStream().write("上传成功！".getBytes());

        fos.close();
        socket.close();
        server.close();
    }
}
```


#多线程上传案例

##客户端（不变）
代码：
```
public class TCPClient {
    public static void main(String[] args) throws IOException {
        Socket socket = new Socket("127.0.0.1",8000);
        //获取字节输出流，将图片写到服务器
        OutputStream out = socket.getOutputStream();
        //创建字节输入流，读取本机上的数据源图片
        FileInputStream fis = new FileInputStream("D:\\test.jpg");
        //开始读写字节数组
        int len = 0;
        byte[] bytes = new byte[1024];
        while((len = fis.read(bytes)) != -1){
            out.write(bytes,0,len);
        }

        //给服务器写终止序列,向服务端写入一个结束标志
        socket.shutdownOutput();

        //获取字节输入流，读取服务器的"上传成功"
        InputStream in = socket.getInputStream();

        len = in.read(bytes); //复用byte数组
        System.out.println(new String(bytes,0,len));

        fis.close();
        socket.close();
    }
}
```

##创建 Upload，实现 Runnable
```
public class Upload implements Runnable {
    private Socket socket;
    //传递socket
    public Upload(Socket socket){
        this.socket = socket;
    }

    @Override
    public void run() {

        //通过客户端连接对象,获取字节输入流,读取客户端图片
        InputStream in = null;
        try {
            in = socket.getInputStream();
            //将目的文件夹封装到File对象
            File upload = new File("E:\\upload");
            if(! upload.exists()){
                upload.mkdirs();
            }

            //防止文件同名被覆盖,重新定义文件名字
            //规则:  域名+当前毫秒值+6位随机数
            String filename="wangdao"+System.currentTimeMillis()+new Random().nextInt(999999)+".jpg";

            //创建字节输出流,将图片写入到目的文件夹中
            FileOutputStream fos = new FileOutputStream(upload+File.separator+filename);
            //读写字节数组
            byte[] bytes = new byte[1024];
            int len = 0;
            while((len = in.read(bytes)) != -1){
                fos.write(bytes,0,len);
            }

            //通过客户端连接对象获取字节输出流
            //将"上传成功"写回客户端
            socket.getOutputStream().write("上传成功！".getBytes());
            fos.close();
            socket.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

##服务端
```
public class TCPThreadServer {
    public static void main(String[] args) throws IOException {
        ServerSocket server = new ServerSocket(8000);
        while(true){
            //获取到一个客户端,必须开启新线程,为这个客户端服务
            Socket socket = server.accept();
            new Thread(new Upload(socket)).start();
        }
    }
}
```

#参考：
https://zhuanlan.zhihu.com/p/39987217
https://www.cnblogs.com/xdp-gacl/p/3631965.html（TCP通信的Socket的例子）
https://www.cnblogs.com/wangcq/p/3520400.html（socket的细节解说）
https://www.cnblogs.com/chenkeyu/p/8485412.html

http://www.52im.net/thread-1095-1-1.html（总框架的资料）

https://www.cnblogs.com/chenkeyu/p/8485412.html（3次握手和4次分手）

大牛的博客：
http://www.ruanyifeng.com/blog/computer/


