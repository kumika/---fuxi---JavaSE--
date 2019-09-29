#Java基础复习--I/O流



#输入/输出
俄罗斯套娃，把这类输入输出作为一个娃娃对象

##文件

###创建

**构造文件：**

**File(String pathname)：**通过给定 **路径名字符串** 转换为 **抽象路径名** 来创建一个新 File 实例

特点：windows中的路径或文件名**不区分大小写**，但是最好别这样，**跨平台**可能会出现问题

**File(String parent, String child)**：根据 parent **路径名字符串** 和 child **路径名字符串**，创建一个新 File 实例

    好处: 单独操作父路径和子路径

File(File parent, String child)：根据 parent **抽象路径名** 和 child **路径名字符串**，创建一个 File 实例

    好处: 父路径是File类型,父路径可以直接调用File类方法

```
public class Test {
    public static void main(String[] args) {
        /*
        * File(String pathname)
        * 传递路径名: 可以写到文件夹,可以写到一个文件
        */
        File file = new File("f:\\a.txt");
        System.out.println(file);

        /*
        * File(String parent, String child)
        * 传递字符串父路径,字符串子路径
        */
        File file = new File("f:\\", "a.txt");
        System.out.println(file);

        /*
        * File(File parent, String child)
        * 传递传递File类型父路径,字符串子路径
        */
        File parent = new File("f:");//带不带"\\"都可以
        File file = new File(parent, "a.txt");
        System.out.println(file);
    }
}
```



**创建单/多级功能：**
public boolean createNewFile()：**创建文件，如果文件已存在，不再创建**
public boolean mkdir()：**创建单级文件夹，如果文件夹已存在，不再创建**
public boolean mkdirs()：**创建多级文件夹，文件夹已经存在了,不在创建**

    mkdirs() 也可以创建单级文件夹，所以推荐使用 mkdirs()

例子：
```
public class TestFile {    
    public static void main(String[] args) throws Exception {  
        File f1 = new File("E://aaa//bbb");  
        if (!f1.exists()) {  
            f1.mkdirs();  
        }  
        // f1.mkdirs(); 生成所有目录  
        // f1.mkdir(); 必须AAA目录存在才能生成BBB目录  
  
        File f2 = new File("E://aaa//bbb//c.txt");  
        if (!file.exists()) {  
            // 不能生成目录，只能创建文件，且/aaa/bbb目录必须存在  
            file.createNewFile();  
        }  
    }          
}   
```

###读写

读写的方法就是下面写的那些read，writer

1、按字节读取文件内容
2、按字符读取文件内容
3、按行读取文件内容
4、随机读取文件内容 

我原本以为文件file类是有自己的读写方法的，实际上是根本没有，有也是能否对这个文件进行读写的判断方法。
所以读写都是file类以外的类进行执行的，比如：字节流，字符流。

###删除

public boolean delete()： **删除文件或者文件夹，不走回收站，直接从硬盘删除！**
如果此路径名表示一个目录，则该目录 必须为空 才能删除。

```
//创建文件夹
File f = new File("C:/a/b/c");
f.mkdirs();  //可以递归创建文件夹，如果没有s则不行

//创建文件
File f1 = new File("C:/a/b/c/123.txt");
f1.createNewFile();

//删除文件
f1.delete();

```

boolean deleteOnExit():
文件使用完成后删除

###重命名
**public boolean renameTo(File dest)**

重新命名此抽象路径名表示的文件。
此方法行为的许多方面都是与平台有关的：重命名操作无法将一个文件从一个文件系统移动到另一个文件系统，dest为新命名的抽象文件
例子：
```
public boolean ReName(String path,String newname) {//文件重命名
        //Scanner scanner=new Scanner(System.in);
        File file=new File(path);
        if(file.exists()) {
        File newfile=new File(file.getParent()+File.separator+newname);//创建新名字的抽象文件
        if(file.renameTo(newfile)) {
            System.out.println("重命名成功！"); 
            return true;
        }
        else {
            System.out.println("重命名失败！新文件名已存在");
            return false;
        }
        }
        else {
            System.out.println("重命名文件不存在！");
            return false;
        }
    
    }
```
#I/O流

Output操作：把内存中的数据存储到持久化设备上这个动作称为输出（写）
Input操作：把持久设备上的数据读取到内存中的这个动作称为输入（读）
流的分类：

    流按流向分为两种：
            输出流：内存——>外设（写出数据）
            输入流：磁盘——>内存（读入数据）
    流按操作类型分为两种：
            字节流：字节流可以操作任何数据,因为在计算机中任何数据都是以字节的形式存储的
            字符流：字符流只能操作纯字符数据，比较方便。


**字节流的抽象父类：InputStream ，OutputStream
字符流的抽象父类：Reader ， Writer**

个人理解：
**流向是以内存为主视角**，

    数据从内存流出，为输出流
    数据向内存流入，为输入流


##字节流

字节流和字符流的区别：
      **区别就是字节流不需要缓冲区且可以不关闭流，而字符流需要缓冲区且需要关闭流。**

①、为什么要使用字符流？

　　因为使用字节流操作汉字或特殊符号语言的时候容易乱码，因为汉字不止一个字节，为了解决这个问题，建议使用字符流。

②、什么情况下使用字符流？

　　一般可以用记事本打开的文件，我们可以看到内容不乱码的。就是文本文件，可以使用字符流。而操作二进制文件（比如图片、音频、视频）必须使用字节流


[![](https://ae01.alicdn.com/kf/Hf90498a6e1da438bb102cc9ea788e243H.jpg)](https://ae01.alicdn.com/kf/Hf90498a6e1da438bb102cc9ea788e243H.jpg)

[![](https://ae01.alicdn.com/kf/H2744c462f8e24620a07ea125e7e4aef1I.jpg)](https://ae01.alicdn.com/kf/H2744c462f8e24620a07ea125e7e4aef1I.jpg)


###普通字节流

####InputStream类
核心方法：
abstract int read()：读取一个字节

    read()方法的特点：read() 执行一次，就会自动读取下一个字节，返回值是读取到的字节, 读取到结尾返回 -1

```
public class IoDemo {
    public static void main(String[] args) throws IOException {
        //创建字节输入流的子类对象
        FileInputStream fis = new FileInputStream("E:\\a.txt"); //ABCD
        //读取一个字节，调用read方法，返回int
        //使用循环方式，读取文件，循环结束的条件：read() 返回 -1
        int len = 0; //接收read()的返回值
        while((len = fis.read()) != -1){
            System.out.print((char)len); //可以转换成字符
        }
        //关闭资源
        fis.close();
    }
}
```

**int read(byte[] b)：**读取**一定数量**的字节，并将其存储在缓冲区**数组** b 中
**int read(byte[] b, int off, int len) ：**读取**最多 len 个字节**，存入 byte 数组。
byte **数组的作用**：缓冲的作用, 提高效率 
read 返回的 int，表示**读取到多少个有效的字节数**
```
public class IoDemo {
    public static void main(String[] args) throws IOException {
        //创建字节输入流的子类对象
        FileInputStream fis = new FileInputStream("E:\\a.txt"); 
        //创建字节数组
        byte[] b = new byte[1024];

        int len = 0;
        while((len = fis.read(b)) != -1){
            System.out.print(new String(b,0,len)); //使用String的构造方法，转为字符串
        }
        //关闭资源
        fis.close();
    }
}
```

####OutputStream类
核心方法:
**void close():** 关闭此输出流并释放与此流有关的所有系统资源。

    close的常规协定：该方法将关闭输出流。关闭的流不能执行输出操作，也不能重新打开。 
    如果流对象建立失败了,需要关闭资源吗？ 
    new 对象的时候，失败了，没有占用系统资源；释放资源的时候,对流对象判断null，变量不是null,对象建立成功，需要关闭资源

**write(byte[] b)：** 将 b.length 个字节从指定的 byte 数组写入此输出流

**write(byte[] b, int off, int len)：**将指定 byte 数组中从偏移量 off 开始的 len 个字节写入此输出流。

**write(int b)：** 将指定的字节写入此输出流。
**write 的常规协定：**向输出流写入一个字节。要写入的字节是参数 b 的八个低位。b 的 24 个高位将被忽略。

###文件字节流

####FileInputStream类

    public class FileInputStream extends InputStream

**FileInPutStream类继承InPutStream类**

构造方法：

**FileInputStream(File file)
FileInputStream(String name)**

流对象 绑定数据源

**输入流读取文件的步骤：**
创建字节输入流的子类对象
调用读取方法 **read** 读取
关闭资源

读取数据的方式：

    一次读取一个字节数据
    一次读取一个字节数组

例子：
读取单个字节
```
public class Copy {
    public static void main(String[] args) {
        long s = System.currentTimeMillis();
        //定义两个流的对象变量
        FileInputStream fis = null;
        FileOutputStream fos = null;
        try{
            //建立两个流的对象,绑定数据源和数据目的
            fis = new FileInputStream("D:\\test.mp3"); //8,003,733 字节
            fos = new FileOutputStream("d:\\test.mp3");
            //字节输入流,读取1个字节,输出流写1个字节
            int len = 0 ;
            while((len = fis.read())!=-1){
                fos.write(len);
            }
        }catch(IOException e){
            System.out.println(e);
            throw new RuntimeException("文件复制失败");
        }finally{
            try{
                if(fos!=null)
                    fos.close();
            }catch(IOException e){
                throw new RuntimeException("释放资源失败");
            }finally{
                try{
                    if(fis!=null)
                        fis.close();
                }catch(IOException e){
                    throw new RuntimeException("释放资源失败");            
               }
            }
        }
        long e = System.currentTimeMillis();
        System.out.println(e-s);
    }
}
//耗时：61817                    
```

读取字节数组
```
public class Copy_1 {
    public static void main(String[] args) {
        long s = System.currentTimeMillis();
        FileInputStream fis = null;
        FileOutputStream fos = null;
        try{
            fis = new FileInputStream("D:\\test.mp3");
            fos = new FileOutputStream("E:\\test.mp3");
            //定义字节数组,缓冲
            byte[] bytes = new byte[1024*10];
            //读取数组,写入数组
            int len = 0 ;
            while((len = fis.read(bytes))!=-1){
                fos.write(bytes, 0, len);
            }
        }catch(IOException e){
            System.out.println(e);
            throw new RuntimeException("文件复制失败");
        }finally{
            try{
                if(fos!=null)
                    fos.close();
            }catch(IOException e){
                throw new RuntimeException("释放资源失败");
            }finally{
                try{
                    if(fis!=null)
                        fis.close();
                }catch(IOException e){
                    throw new RuntimeException("释放资源失败");
                }
            }
        }   
        long e = System.currentTimeMillis();
        System.out.println(e-s);
    }
}
//耗时：41        
```


####FileOutputStream类

    public class FileOutputStream extends OutputStream

**FileOutPutStream类继承OutPutStream类**

构造方法：
**FileOutputStream(File file) ：**创建一个向指定 File 对象表示的文件中写入数据的文件输出流。
**FileOutputStream(File file, boolean append)：** 创建一个向指定 File 对象表示的文件中写入数据的文件输出流，以追加的方式写入。
**FileOutputStream(String name) ：**创建一个向具有指定名称的文件中写入数据的输出文件流。
**FileOutputStream(String name, boolean append) ：**创建一个向具有指定 name 的文件中写入数据的输出文件流，以追加的方式写入。

```
//实现追加写入：
FileOutputStream fos = new FileOutputStream(file, true);
```

流对象的使用步骤：

     创建流子类的对象,绑定数据目的
     调用流对象的方法write写
     close释放资源

注意事项：流对象的构造方法,可以创建文件，如果文件存在，直接覆盖

```
public class IoDemo {
    public static void main(String[] args) throws IOException {
        //创建流子类的对象,绑定数据目的
        FileOutputStream fos = new FileOutputStream("E:\\a.txt");
        //用write方法写数据
        //写一个字节
        fos.write(100); //d

        //写字节数组
        byte[] bytes = {65,66,67,68}; //ABCD
        fos.write(bytes);

        //写字节的一部分,开始索引，写几个
        fos.write(bytes,1,1); //B
        
        //写入字节数组的简便方式
        //写字符串
        fos.write("Hello".getBytes());
        //关闭资源
        fos.close();
    }
}
```


###带缓冲的字节流

可提高IO流的读写速度
分为字节缓冲流与字符缓冲流



####BufferedInputStream类

    public class BufferedInputStream extends FilterInputStream

BufferedInPutStream类继承**FilterInPutStream**类,不是继承**FileInPutStream**类，注意类名。

标准的**字节输入流**
方法:

    read()
    read(byte[] b, int off, int len)

构造方法：
BufferedInputStream(InputStream in)：可以传递任意的字节输入流，传递是谁，就提高谁的效率
BufferedInputStream(InputStream in, int size)

**可以传递的字节输入流 FileInputStream**

例子：
```
public class BufferedInputStreamDemo {
    public static void main(String[] args) throws IOException{
        //创建字节输入流的缓冲流对象,构造方法中包装字节输入流,包装文件
        BufferedInputStream bis = new
                BufferedInputStream(new FileInputStream("c:\\buffer.txt"));
        byte[] bytes = new byte[1024];
        int len = 0 ;
        while((len = bis.read(bytes))!=-1){
            System.out.print(new String(bytes,0,len));
        }
        bis.close();
    }
}
```


####BufferedOutputStream类

    public class BufferedOutputStream extends FilterOutputStream

BufferedOutPutStream类继承**FilterOutPutStream**类,不是继承**FileOutPutStream**类，注意类名。


**作用: 提高原有输出流的写入效率**
方法：

    write(int b)
    write(byte[] b, int off, int len)

构造方法：
**BufferedOuputStream(OuputStream out)：**可以传递任意的字节输出流，传递的是哪个字节流，就对哪个字节流提高效率

例子:
```
public class BufferedOutputStreamDemo {
    public static void main(String[] args)throws IOException {
        //创建字节输出流,绑定文件
        //FileOutputStream fos = new FileOutputStream("c:\\buffer.txt");

        //创建字节输出流缓冲流的对象,构造方法中,传递字节输出流
        BufferedOutputStream bos = new
                BufferedOutputStream(new FileOutputStream("c:\\buffer.txt"));

        bos.write(55);
        byte[] bytes = "HelloWorld".getBytes();
        bos.write(bytes);
        bos.write(bytes, 3, 2);

        bos.close();
    }
}
```

###四种文件复制方式的效率比较
结论：
字节流读写单个字节 ：125250 毫秒
字节流读写字节数组 ：193 毫秒
字节流缓冲区流读写单个字节：1210 毫秒
字节流缓冲区流读写字节数组 ：73 毫秒
代码
```
public class Copy {
    public static void main(String[] args)throws IOException {
        long s = System.currentTimeMillis();
        copy_4(new File("c:\\q.exe"), new File("d:\\q.exe"));
        long e = System.currentTimeMillis();
        System.out.println(e-s);
    }
    /*
     * 方法,实现文件复制
     *  4. 字节流缓冲区流读写字节数组
     */
    public static void copy_4(File src,File desc)throws IOException{
        BufferedInputStream bis = new BufferedInputStream(new FileInputStream(src));
        BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream(desc));
        int len = 0 ;
        byte[] bytes = new byte[1024];
        while((len = bis.read(bytes))!=-1){
            bos.write(bytes,0,len);
        }
        bos.close();
        bis.close();
    }
    /*
     * 方法,实现文件复制
     *  3. 字节流缓冲区流读写单个字节
     */
    public static void copy_3(File src,File desc)throws IOException{
        BufferedInputStream bis = new BufferedInputStream(new FileInputStream(src));
        BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream(desc));
        int len = 0 ;
        while((len = bis.read())!=-1){
            bos.write(len);
        }
        bos.close();
        bis.close();
    }

    /*
     * 方法,实现文件复制
     *  2. 字节流读写字节数组
     */
    public static void copy_2(File src,File desc)throws IOException{
        FileInputStream fis = new FileInputStream(src);
        FileOutputStream fos = new FileOutputStream(desc);
        int len = 0 ;
        byte[] bytes = new byte[1024];
        while((len = fis.read(bytes))!=-1){
            fos.write(bytes,0,len);
        }
        fos.close();
        fis.close();
    }

    /*
     * 方法,实现文件复制
     *  1. 字节流读写单个字节
     */
    public static void copy_1(File src,File desc)throws IOException{
        FileInputStream fis = new FileInputStream(src);
        FileOutputStream fos = new FileOutputStream(desc);
        int len = 0 ;
        while((len = fis.read())!=-1){
            fos.write(len);
        }
        fos.close();
        fis.close();
    }
}
```

##字符流

[![](https://ae01.alicdn.com/kf/H83d94f3870af4908a5cf5e636d4413cao.jpg)](https://ae01.alicdn.com/kf/H83d94f3870af4908a5cf5e636d4413cao.jpg)

[![](https://ae01.alicdn.com/kf/H4d46cb5e776349449f29a77414006008t.jpg)](https://ae01.alicdn.com/kf/H4d46cb5e776349449f29a77414006008t.jpg)

###普通字符流

####Reader抽象类

    public abstract class Reader
    extends Object
    implements Readable, Closeable

专门读取文本文件
方法介绍：
int read()： 读取单个字符
int read(char[] cbuf)：将字符读入数组
abstract int read(char[] cbuf, int off, int len) ：将字符读入数组的某一部分

**没有读取字符串的方法**



####Writer抽象类

    public abstract class Writer
    extends Object
    implements Appendable, Closeable, Flushable


写文件，写 文本文件

常用方法：
void write(int c)：写入单个字符
void write(String str) ：写入字符串
void write(String str, int off, int len)：写入字符串的某一部分
void write(char[] cbuf)：写入字符数组
abstract void write(char[] cbuf, int off, int len) ：写入字符数组的某一部分



###文件字符流

####FileReader类

    public class FileReader extends InputStreamReader

Reader类 是抽象类，找到子类对象 FileReader
构造方法：

    FileReader(File file)
    FileReader(String fileName)

写入的数据目的。绑定数据源。参数也是 File 类型对象 和 String 文件名。

例子：
```
public class ReaderDemo {
    public static void main(String[] args) throws IOException{
        FileReader fr = new FileReader("c:\\1.txt");
        //读字符
        /*int len = 0 ;
        while((len = fr.read())!=-1){
            System.out.print((char)len);
        }*/

        //读数组
        char[] ch = new char[1024];
        int len = 0 ;
        while((len = fr.read(ch))!=-1){
            System.out.print(new String(ch,0,len)); //转字符串
        }
        fr.close();
    }
}
```

#####flush 方法和 close 方法区别
flush()方法：用来刷新缓冲区的，刷新后可以再次写出，只有字符流才需要刷新 
close()方法：用来关闭流释放资源的的，如果是带缓冲区的流对象的close()方法，不但会关闭流，还会再关闭流之前刷新缓冲区，关闭后不能再写出



####FileWriter类

    public class FileWriter extends InputStreamWriter

Writer 类是抽象类，找到子类对象 FileWriter
构造方法：

    FileWriter(File file)
    FileWriter(String fileName)

参数也是 File 类型对象 和 String 文件名。
字符输出流，写数据的时候，**必须要运行**一个功能，**刷新功能：flush()**

```
public class WriterDemo {
    public static void main(String[] args) throws IOException{
        FileWriter fw = new FileWriter("c:\\1.txt");

        //写1个字符
        fw.write(100);
        fw.flush();

        //写1个字符数组
        char[] c = {'a','b','c','d','e'};
        fw.write(c);
        fw.flush();
        
        //写字符数组一部分
        fw.write(c, 2, 2);
        fw.flush();

        //写如字符串
        fw.write("hello");
        fw.flush();

        fw.close();
    }
}
```

**字符流复制文本文件**

字符流复制文本文件，必须是文本文件
字符流查询本机默认的编码表，简体中文GBK
FileReader 读取数据源
FileWriter 写入到数据目的

```
//读取字符数组
public class Copy_2 {
    public static void main(String[] args) {
        FileReader fr = null;
        FileWriter fw = null;
        try{
            fr = new FileReader("c:\\1.txt");
            fw = new FileWriter("d:\\1.txt");
            char[] cbuf = new char[1024];
            int len = 0 ;
            while(( len = fr.read(cbuf))!=-1){
                fw.write(cbuf, 0, len);
                fw.flush();
            }

        }catch(IOException ex){
            System.out.println(ex);
           throw new RuntimeException("复制失败");
        }finally{
            try{
                if(fw!=null)
                    fw.close();
            }catch(IOException ex){
                throw new RuntimeException("释放资源失败");
            }finally{
                try{
                    if(fr!=null)
                        fr.close();
                }catch(IOException ex){
                    throw new RuntimeException("释放资源失败");
                }
            }
        }
    }
}            
```



###带缓冲的字符流

####BufferedReader类

从字符输入流中读取文本，缓冲各个字符，从而实现字符、数组和行的高效读取
读取方法： read()， 单个字符,字符数组
构造方法：**BufferedReader(Reader r)**：可以任意的字符输入流，有：**FileReaderInputStreamReader**

例子：
```
public class BufferedReaderDemo {
    public static void main(String[] args) throws IOException {
        int lineNumber = 0;
        //创建字符输入流缓冲流对象,构造方法传递字符输入流,包装数据源文件
        BufferedReader bfr = new BufferedReader(new FileReader("c:\\a.txt"));
        //调用缓冲流的方法 readLine()读取文本行
        //循环读取文本行, 结束条件 readLine()返回null
        String line = null;
        while((line = bfr.readLine())!=null){
            lineNumber++;
            System.out.println(lineNumber+"  "+line);
        }
        bfr.close();
    }
}
```


####BufferedWriter类

写入方法： write () ，单个字符,字符数组,字符串
构造方法：
**BufferedWriter(Writer w)**：传递任意字符输出流，传递谁，就高效谁

能传递的字符输出流： FileWriter, OutputStreamWriter

例子：
```
public class BufferedWrierDemo {
    public static void main(String[] args) throws IOException{
        //创建字符输出流,封装文件
        FileWriter fw = new FileWriter("c:\\buffer.txt");
        BufferedWriter bfw = new BufferedWriter(fw);

        bfw.write(100);
        bfw.flush();
        bfw.write("你好".toCharArray());
        bfw.flush();

        bfw.write("你好");
        bfw.flush();

        bfw.write("我好好");
        bfw.flush();

        bfw.write("大家都好");
        bfw.flush();

        bfw.close();
    }
}
```

###字符流缓冲区流复制文本文件

```
/*
 *  使用缓冲区流对象,复制文本文件
 *  数据源  BufferedReader+FileReader 读取
 *  数据目的 BufferedWriter+FileWriter 写入
 *  读取文本行, 读一行,写一行,写换行
 */
public class Copy_1 {
    public static void main(String[] args) throws IOException{
        BufferedReader bfr = new BufferedReader(new FileReader("c:\\w.log"));   
        BufferedWriter bfw = new BufferedWriter(new FileWriter("d:\\w.log"));
        //读取文本行, 读一行,写一行,写换行
        String line = null;
        while((line = bfr.readLine())!=null){
            bfw.write(line);
            bfw.newLine();
            bfw.flush();
        }
        bfw.close();
        bfr.close();
    }
}
```


###字节转字符流


####InputStreamReader类

    java.io.InputStreamReader 继承 Reader

 字符输入流，读取文本文件
 字节流向字符的桥梁，将 **字节流** 转为 **字符流**
读取的方法：**read()** 读取1个字符，读取字符数组

构造方法：
InputStreamReader(InputStream in)：接收所有的 **字节输入流**

    可以传递的字节输入流： FileInputStream

InputStreamReader(InputStream in,String charsetName) : 传递编码表的名字

例子：
```
public class InputStreamReaderDemo {
    public static void main(String[] args) throws IOException {
        readUTF();
    }

    //转换流,InputSteamReader读取文本，采用UTF-8编码表,读取文件utf
    public static void readUTF()throws IOException{
        //创建字节输入流,传递文本文件
        FileInputStream fis = new FileInputStream("c:\\utf.txt");
        //创建转换流对象,构造方法中,包装字节输入流,同时写编码表名
        InputStreamReader isr = new InputStreamReader(fis,"UTF-8");
        char[] ch = new char[1024];
        int len = isr.read(ch);
        System.out.println(new String(ch,0,len));
        isr.close();
    }
}
```


####OutputStreamWriter类

    java.io.OutputStreamWriter 继承 Writer 类


字符输出流，写文本文件
是字符通向字节的桥梁，将 字符流 转成 字节流
方法write()：字符，字符数组，字符串

构造方法：
OutputStreamWriter(OuputStream out)：接收所有的**字节输出流**

    字节输出流目前只有： FileOutputStream

OutputStreamWriter(OutputStream out, String charsetName)

    String charsetName：传递 编码表名字：GBK、UTF-8


例子：
```
public class OutputStreamWriterDemo {
    public static void main(String[] args) throws IOException {
        writeUTF();
    }

   //转换流对象OutputStreamWriter写文本，采用UTF-8编码表写入
    public static void writeUTF()throws IOException{
        //创建字节输出流，绑定文件
        FileOutputStream fos = new FileOutputStream("c:\\utf.txt");
        //创建转换流对象，构造方法保证字节输出流，并指定编码表是UTF-8
        OutputStreamWriter osw = new OutputStreamWriter(fos,"UTF-8"); //GBK可以不写
        osw.write("你好");
        osw.close(); //使用close()连刷新带关闭
    }
}
```

####转换流子类父类的区别

**继承关系**
OutputStreamWriter 的子类: FileWriter
InputStreamReader 的子类：FileReader
**区别**
OutputStreamWriter 和 InputStreamReader 是**字符和字节的桥梁**：也可以称之为字符转换流。**字符转换流原理：字节流 + 编码表**
FileWriter 和 FileReader：作为子类，仅作为操作字符文件的便捷类存在。当操作的字符文件，使用的是默认编码表时可以不用父类，而直接用子类就完成操作了，简化了代码。
以下三句话功能相同


    InputStreamReader isr = new InputStreamReader(new FileInputStream("a.txt"));//默认字符集。
    InputStreamReader isr = new InputStreamReader(new FileInputStream("a.txt"),"GBK");//指定GBK字符集。
    FileReader fr = new FileReader("a.txt");

注意：一旦要指定其他编码时，绝对不能用子类，必须使用字符转换流。什么时候用子类呢？ 
条件：1、操作的是文件。2、使用默认编码。



#总结
##字节流
###字节输入流 InputStream

    FileInputStream 操作文件的字节输入流
    BufferedInputStream高效的字节输入流
    ObjectInputStream 反序列化流

###字节输出流 OutputStram

    FileOutputStream 操作文件的字节输出流
    BufferedOutputStream 高效的字节输出流
    ObjectOuputStream 序列化流
    PrintStream 字节打印流

##字符流
###字符输入流 Reader

    FileReader 操作文件的字符输入流
    BufferedReader 高效的字符输入流
    InputStreamReader 输入操作的转换流(把字节流封装成字符流)

###字符输出流 Writer

    FileWriter 操作文件的字符输出流
    BufferedWriter 高效的字符输出流
    OutputStreamWriter 输出操作的转换流(把字节流封装成字符流)
    PrintWriter 字符打印流

##方法：
###读数据方法：

    read() 一次读一个字节或字符的方法
    read(byte[] char[]) 一次读一个数组数据的方法
    readLine() 一次读一行字符串的方法(BufferedReader类特有方法)
    readObject() 从流中读取对象(ObjectInputStream特有方法)

###写数据方法：

    write(int) 一次写一个字节或字符到文件中
    write(byte[] char[]) 一次写一个数组数据到文件中
    write(String) 一次写一个字符串内容到文件中
    writeObject(Object ) 写对象到流中(ObjectOutputStream类特有方法)
    newLine() 写一个换行符号(BufferedWriter类特有方法)

##向文件中写入数据的过程 
1，创建输出流对象 
2，写数据到文件 
3，关闭输出流
##从文件中读数据的过程 
1， 创建输入流对象 
2， 从文件中读数据 
3， 关闭输入流
##文件复制的过程 
1， 创建输入流（数据源） 
2， 创建输出流（目的地） 
3， 从输入流中读数据 
4， 通过输出流，把数据写入目的地 
5， 关闭流
##File类

###方法

    获取文件名称 getName()
    获取文件绝对路径 getAbsolutePath()
    获取文件大小 length()
    获取当前文件夹中所有File对象 File[] listFiles()
    判断是否为文件 isFile()
    判断是否为文件夹 isDirectory()
    创建文件夹 mkdir() mkdirs()
    创建文件 createNewFile()

###异常

    try..catch…finally捕获处理异常
    throws 声明异常
    throw 抛出异常对象

###异常的分类
编译期异常 Exception
运行期异常 RuntimeException
注意：
编译期异常，必须处理，不然无法编译通过
运行期异常，程序运行过程中，产生的异常信息
###实现文件内容的自动追加
构造方法

    FileOutputStream(File file, boolean append)
    FileOutputStream(String fileName, boolean append)
    FileWriter(File, boolean append)
    FileWriter(String fileName, boolean append)

###实现文件内容的自动刷新
构造方法

    PrintStream(OutputStream out, boolean autoFlush)
    PrintWriter(OutputStream out, boolean autoFlush)
    PrintWriter(Writer out, boolean autoFlush)


#参考：

https://www.cnblogs.com/yellowbananame/p/8946976.html（文件的操作）


https://zhuanlan.zhihu.com/p/39803062(文件大体框架)
https://zhuanlan.zhihu.com/p/39886428(大体框架)

https://www.cnblogs.com/ysocean/p/6859242.html（看不懂的就看下这里的例子）

