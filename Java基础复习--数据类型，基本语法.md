#Java基础复习--数据类型，基本语法
基本的纲要
[![](https://ae01.alicdn.com/kf/Hc3df6c7d724e4b6286ec467d0f7ea31e1.jpg)](https://ae01.alicdn.com/kf/Hc3df6c7d724e4b6286ec467d0f7ea31e1.jpg)



#java的基础知识

#数据类型

##基本数据类型

8种类型，分4种整型，2种浮点类型 单一表示Unicode字符的char字符型，Boolean布尔类型

###整型

4种整型：

 1. Long
 2. Int
 3. Byte
 4. short

###浮点类型

 1. float
 2. double

##引用类型

任意对象，数组，类，接口


##区别

基本类型的变量是存放在栈区（栈区指内存里的栈内存）
[![](https://ae01.alicdn.com/kf/H4584759fb36f4fc98dc2fcf7410557e72.png)](https://ae01.alicdn.com/kf/H4584759fb36f4fc98dc2fcf7410557e72.png)


引用类型的值是同时保存在栈内存和堆内存的对象
[![](https://ae01.alicdn.com/kf/Hea6932c5c7d0452ba237e488f2ad5355u.png)](https://ae01.alicdn.com/kf/Hea6932c5c7d0452ba237e488f2ad5355u.png)


##对象引用

2个对象变量A,B
person  A = {}；//A保存了一个空对象的实例（地址）
person  B = A //A和B都指向了这个空对象。

A.name = "faker"；
B.age  = 23;

A.equals(B)

他们之间的关系：
[![](https://ae01.alicdn.com/kf/Hcd4e07b487d849bfadfeccfafbc9c02bv.png)](https://ae01.alicdn.com/kf/Hcd4e07b487d849bfadfeccfafbc9c02bv.png)
**引用类型的赋值其实是对象保存栈区地址指针的赋值，因此2个变量对象指向同一个对象，任何操作都会相互影响(Java是值传递)。**

###值传递和引用传递

值传递和引用传递，引出这个问题的是因为：
在参数赋值的时候，方法的参数类型上出现赋值问题
这是什么赋值问题？

    方法参数是形参，基本类型和对象（引用类型）作为形参是有区别的。

什么区别？

    基本类型的变量在创建的时候，栈内存传递的是数据本身的副本，也就是传递值（传递复制品）。引用类型的变量（对象）在创建的时候，栈内存传递的是该对象在堆内存的地址，也就是传递的是地址。


那为什么引用类型能改变数据内容，不是传递的是地址吗？

    因为规定了如果在函数中没有改变这个变量副本的地址，而是改变了地址中的值，那么在函数内的改变会影响到传入的参数。


图片解释：
左边2个框框都是在栈内存，右边框框是在堆内存

[![](https://ae01.alicdn.com/kf/H069ba8c098cf4f5f8ccaf4757da9f463F.jpg)](https://ae01.alicdn.com/kf/H069ba8c098cf4f5f8ccaf4757da9f463F.jpg)
[![](https://ae01.alicdn.com/kf/H4655522251324f19bfb428dfb725f437G.jpg)](https://ae01.alicdn.com/kf/H4655522251324f19bfb428dfb725f437G.jpg)
这个num都是在栈内存上的。
[![](https://ae01.alicdn.com/kf/H96321246b1a644bbbac08c39580fb0e9k.jpg)](https://ae01.alicdn.com/kf/H96321246b1a644bbbac08c39580fb0e9k.jpg)

这个`值传递	引用传递`问题其实是解决当方法参数有基本类型与引用类型，并且调用方法结束后还使用该基本类型变量的时候，出现的赋值问题。要清楚哪个类型是能改变





#基本语法

##基本运算

算术运算符
+，—，*，/，%余，++自加，--自减

关系运算符
<， >， >=， <=，== ，!=
逻辑运算符
非！，与&，或|，^异或,&& 与,|| 或

赋值运算符
= ，+=，-=，*=，/=

字符串连接运算符
+

##基本语句
###if语句
if
if ····else
if ····else if
if ····else if····else if ···else


###Switch语句
Switch(){
    case XXX :
                ····
    case XXX :
                ····
    default：
                ····
}
也可以加break语句


###循环语句

for

while

do····while


##函数（方法）
###函数定义
```
 1 public class TestMethod{
 2     public static void main(String args[]){
 3         m();
 4         m1(3);
 5         m2(2,3);
 6         int i = m3(4,5);
 7         System.out.println(i);
 8     }
 9     //以下定义的都是静态方法，静态方法可以在main()方法里面直接调用
10     public static void m(){
11             System.out.println("Hello!");
12             System.out.println("李相赫");
13         }
14         
15     public static void m1(int i){
16             if(i==5){
17                     return;
18                 }
19             System.out.println(i);
20         }
21         
22     public static void m2(int i,int j){
23             System.out.println(i+j);
24         }
25         
26     public static int m3(int i,int j){
27             return i+j;
28         }
29 }
```
方法执行到return语句后，这个方法的执行就结束了，**方法可以有返回值，但可以不用这个返回值**。方法首先要定义，然后才能调用。


###函数重载
在JAVA中，可以在同一个类中存在多个函数，函数名称相同但参数列表不同。这就是函数的重载（overlording）。

**重载的特征：**

    函数名和返回值类型完全一致。
    
    参数的数量不同、或数量相同而类型和次序不同，以方便JVM区分到底调用哪个函数。

```
public class TestOverLoad {

    void max(int a, int b) {
        System.out.println(a > b ? a : b);
    }

    /*
     * int max(int a, int b) { 
     *         return a > b ? a : b; 
     * }
     */

    void max(float a, float b) {
        System.out.println(a > b ? a : b);
    }
}
```
或者
```
int max(int a, int b) {
    System.out.println("调用的int max(int a, int b)方法");
    return a > b ? a : b;
}
     
int max(short a, short b) {
    System.out.println("调用的int max(short a, short b)方法");
    return a > b ? a : b;
}
```


重写Overriding

“重载”不同于“重写”

“重写（覆盖）”概念存在于继承关系中，子类可继承父类中的方法而不需要单独编辑，这提供便捷化。但有的时侯，子类不想原封不动地继承父类的方法，而是想作一定的修改，这就需要采用方法的重写。

父类中存在一个函数，子类中也存在一个同名函数，在了类中对函数重新编辑，做得更具体化。

重写的规则：

1、在子类中可以根据需要对从父类中继承来的方法进行重写。

2、重写的方法和被重写的方法必须具有相同方法名称、参数列表和返回类型。

3、重写方法不能使用比被重写的方法更严格的访问权限。


###函数调用
函数调用
```
public class Demo03{
    int age;
    public static void main(String []args){
        System.out.println(Demo04.name);//静态调用静态1
        Demo04.eat();

        Demo04 d = new Demo04();//静态调用静态2
        System.out.println(d.name);
        d.eat();

        Demo03 d1 = new Demo03();//静态调用非静态
        d1.method();
        System.out.println(d1.age);
    }
    public void method(){
        System.out.println("first method");
    }

}
public class Demo04{
    static String name = "张三";

    public static void eat(){
        System.out.println("肉夹馍");
    }
}

```




###递归函数

递归：**在一个方法内部对自身的调用就称为递归**

递归方法执行在内存中执行的过程如下图所示：
重点在继续执行的判断条件
[![](https://ae01.alicdn.com/kf/Haecd7f140b5f4769b3d9360d2e1e078aR.png)](https://ae01.alicdn.com/kf/Haecd7f140b5f4769b3d9360d2e1e078aR.png)

例子：
使用递归计算第5个斐波那契数列数
```
/*计算第5个斐波那契数列数*/
/*
斐波那契数列特点：f(1)=1,f(2)=1,f(3)=f(1)+f(2),f(4)=(f2)+(f3)……依次类推。
即后一个数都是等于前两个数的和，这样的数列就是斐波那契数列。
*/
/*
使用递归调用的方法计算
*/
public class Fab{
    public static void main(String args[]){
        System.out.println(f(5));
    }
    
    public static int f(int n){
            if(n==1||n==2){
                    return 1;
                }else{
                        return f(n-1)+f(n-2);
                    }
        }
}
```
整个在内存中执行过程如下图所示
[![](https://ae01.alicdn.com/kf/Habdbe761c8b940d6993346a1eba818fbN.png)](https://ae01.alicdn.com/kf/Habdbe761c8b940d6993346a1eba818fbN.png)

###程序的执行过程

[![](https://ae01.alicdn.com/kf/H4205ec3439f848b88d16297ef40a954fi.png)](https://ae01.alicdn.com/kf/H4205ec3439f848b88d16297ef40a954fi.png)



##数组

###创建
一维数组的声明方式有2种：

格式一：数组元素类型  数组名[ ];  即type var[ ];
格式二：数组元素类型[ ] 数组名; 即type[ ] var;
格式二声明数组的方法与C#上声明一维数组的方法一样。
例如：
       
       int a1[ ];   int[ ] a2;
       double b[ ];
       person[ ] p1;  String s1[ ];

注意：**JAVA语言中声明数组时不能指定其长度(数组中的元素个数)**

       如：int a[5]; 这样声明一维数组是非法的。


**创建数组对象：**
　JAVA中使用关键字new创建数组对象。

　格式为：数组名 = new 数组元素的类型[数组元素的个数]
例如：
[![](https://ae01.alicdn.com/kf/H2cc65d6ecb194e6f9ed033581a8055d6b.png)](https://ae01.alicdn.com/kf/H2cc65d6ecb194e6f9ed033581a8055d6b.png)
[![](https://ae01.alicdn.com/kf/H82a6c907dc334a5db492b53270bc27e4J.png)](https://ae01.alicdn.com/kf/H82a6c907dc334a5db492b53270bc27e4J.png)


动态的初始化
```
public class Test{
    public static void main(String args[ ]){
    int a[ ];  //定义数组，即声明一个int类型的数组a[ ]
    a=new int[3];  //给数组元素分配内存空间。
    a[0]=3; a[1]=9; a[2]=8;  //给数组元素赋值。
    Date days[ ];
    days=new Date[3];
    days[0]=new Date(1, 4, 2004);
    days[1]=new Date(2, 4, 2004);
    days[2]=new Date(3, 4, 2004);
    } 
}

class Date{
    int year, month, day;
    Date(int y, int m, int d){
        year = y ;
        month = m ;
        day = d ;
    }
}
```
数组的内存分配情况：
[![](https://ae01.alicdn.com/kf/H95afd28a3e364dffb1dcdf7a30000b3e3.png)](https://ae01.alicdn.com/kf/H95afd28a3e364dffb1dcdf7a30000b3e3.png)

###遍历
for循环
```
//遍历数组
public class ThroughTheArray{
	public static void main(String[] args) {
		// TODO Auto-generated method stub
		int[] arr = {12,4,1,66,54,6,74,-3};//静态创建一个数组
		for (int i = 0; i < arr.length; i++) {
			System.out.print(arr[i] + ",");
		}
	}
}
```
增强for循环foreach
```
//遍历数组
public class ThroughTheArray {
	public static void main(String[] args) {
		// TODO Auto-generated method stub
		int[] arr = {12,4,1,66,54,6,74,-3};//静态创建一个数组
		for (int i : arr) {
			System.out.print(i + ",");
		}
	}
}
```
利用jdk自带的方法  --> java.util.Arrays.toString()
```
//遍历数组
public class ThroughTheArray {
	public static void main(String[] args) {
		// TODO Auto-generated method stub
		int[] arr = {12,4,1,66,54,6,74,-3};//静态创建一个数组
		System.out.println(java.util.Arrays.toString(arr));
	}
}
```

###多维数组

 1. 一维数组：一维数组就是一行，一行小格。
 2. 二维数组：二维数组就是一行加一列组成的一个平面分成的小格，有行有列。
 3. 三维数组：三维数组就是一个立方体。

人类对最多认识到三维空间

理解JAVA中的各个维度的数组模型
[![](https://ae01.alicdn.com/kf/Hec13f3392ec243eea20b53d11251f7a81.png)](https://ae01.alicdn.com/kf/Hec13f3392ec243eea20b53d11251f7a81.png)

##异常
###异常分类
运行时异常

    Exception

它规定的异常是程序本身可以处理的异常。异常和错误的区别是，异常是可以被处理的，而错误是没法处理的。

编译时异常

    error

Error是错误，对于所有的编译时期的错误以及系统错误都是通过Error抛出的。这些错误表示故障发生于虚拟机自身、或者发生在虚拟机试图执行应用时，如Java虚拟机运行错误（Virtual MachineError）、类定义错误（NoClassDefFoundError）等。这些错误是不可查的，因为它们在应用程序的控制和处理能力之 外，而且绝大多数是程序运行时不允许出现的状况。


###异常处理

Java异常处理的五个关键字：try、catch、finally、throw、throws

#参考：

https://www.cnblogs.com/xdp-gacl/tag/java%E5%9F%BA%E7%A1%80%E6%80%BB%E7%BB%93/default.html?page=2（总体知识框架参考的对象）


https://www.cnblogs.com/bobo-site/p/9306049.html（基本类型的堆栈）

https://mp.weixin.qq.com/s?__biz=MzAwNTM0ODY1Mg==&mid=2457116077&idx=1&sn=ea9c69e95fede0f7410d6b6f1889ae9e&chksm=8c9e31eebbe9b8f887e779de89e829abb37fefc3f792ffd69bec8fd5d441834e2beeaa8dff67&token=313878124&lang=zh_CN#rd（这是值传递与引用传递的区别）
