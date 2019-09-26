#Java基础复习 4


#面对对象

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

##字节流

###普通字节流

###文件字节流

###带缓冲的字节流


##字符流

###普通字符流

###文件字符流

###带缓冲的字符流

###字节转字符流

#参考：

https://www.cnblogs.com/yellowbananame/p/8946976.html（文件的操作）


https://zhuanlan.zhihu.com/p/39803062(文件大体框架)
