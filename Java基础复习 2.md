#Java基础复习 2


#面对对象
面向对象的思维是，当我碰到这个问题域的时候，碰到这个程序的时候，我首先应该把这个问题里有哪些对象，对象与对象之间有什么关系抽象出来。

我实现这件事我第一步应该干什么，第二步应该干什么,那就是面向过程的思维了.

##类和对象

###属性和方法

```
/**
 * 一类事物封装到JAVA里面首先得写class，定义这个类，类名是什么可以自己取。
 * 这里把类名叫做Dog
 */
public class Dog {
    /**
     * 接下来就是写这个狗这个类的属性或者叫成员变量，
     * 比如说狗这个类的毛的颜色，怎么定义这个属性呢，
     * 首先得定义毛的一个类型,如使用int来定义毛的颜色类型
     */
    int furcolor; //定义属性：毛的颜色
    float height; //定义属性：狗的高度
    float weight; //定义属性：狗的体重
    
    /**
     * 狗的颜色，高度，体重这些属性定义完了，接下来要定义的就是方法了。
     * 如写一个CatchMouse（）方法，捉老鼠的方法。
     * CatchMouse这个方法里面有一个对象类型的参数，捉哪一只老鼠，这个对象参数是属于Mouse这个类的
     * @param m
     */
    void CatchMouse(Mouse m){
            //在方法体内写捉老鼠这个过程，怎么捉，跑着捉，走着捉
            System.out.println("我捉到老鼠了，汪汪！，老鼠要尖叫了！");
            /**
             * 老鼠尖叫一声，表示被狗咬到了，咬到了能不叫吗，很自然而然地想到，
             * 尖叫（scream()）这个方法是属于Mouse这个类里面的某一个方法。
             * 老鼠自己调用它，让它自己尖叫。这就是面向对象的思维。
             */
            m.scream();
    }
    
    public static void main(String[] args) {
        Dog  d = new Dog();//首先用new关键字创建一只狗
        Mouse m=new Mouse();//造出一只老鼠。
        d.CatchMouse(m);//然后用这只狗去抓老鼠，让狗调用CatchMouse()方法去捉某只老鼠。
    }
}
```


###封装

```
/**
 * 封装的老鼠类
 */
public class Mouse {
    /**
     * 老鼠自己有一个发出尖叫的方法
     * 当被狗咬到时就会发出尖叫
     */
    public void scream() {
        System.out.println("我被狗咬到了，好痛啊！");
    }

}
```

##构造方法
构造方法是用来创建一个新的对象的，与new组合在一起用，使用new+构造方法创建一个新的对象。
你new一个东西的时候，实际上你调用的是一个构造方法，构造方法就是把自己构造成一个新的对象，所以叫构造方法，构造一个新对象用的方法叫构造方法。

###定义

构造方法比较特殊，构造方法的名字必须和类的名字完全一模一样，包括大小写，并且没有返回值。如原来定义的一个person类，在类里面声明了两个成员变量id与age，这时候你可以再为这个person类定义一个它的构造方法person(int n，int i)，这个方法的名字和类名完全相同，并且没有返回值，也就是在这个方法前面不能写任何的方法的返回类型修饰符，连void都不可以写。

```
public class Person {
    int id;  //在person这类里面定义两个成员变量id和age,
    int age=20;  //给成员变量age赋了初值为20

    /**这里就是person这个类的一个构造方法
     * 构造方法的规则很简单，和类名要完全一样，一点都不能错，包括大小写。
     * 并且没有返回值，不能写void在它前面修饰
     * @param _id
     * @param _age
     */
    public Person(int _id,int _age ) {
        id = _id;
        age = _age;
    }
}
```

构造方法写好后就和new组合在一起使用，new的作用是构建一个新对象，创造一个新对象，所以new的时候实际当中调用的是构造方法。只有调用了这个构造方法才能构造出一个新的对象。例如：
```
 public static void main(String[] args) {
     Person tom = new Person(1, 25); // 调用person这个构造方法创建一个新的对象，并给这个对象的成员变量赋初始值
}
```

[![](https://ae01.alicdn.com/kf/Hc021094f92de4fb5b00ede5a0feafc53U.png)](https://ae01.alicdn.com/kf/Hc021094f92de4fb5b00ede5a0feafc53U.png)

下面是在main方法里面调用person构造方法时的内存分析情况：
[![](https://ae01.alicdn.com/kf/H050c416c77cb40fd97e7e0336c3039c3m.png)](https://ae01.alicdn.com/kf/H050c416c77cb40fd97e7e0336c3039c3m.png)
[![](https://ae01.alicdn.com/kf/He70da7356ded47cbae8049231214b15cT.png)](https://ae01.alicdn.com/kf/He70da7356ded47cbae8049231214b15cT.png)

**当方法调用完成之后，栈里面为它分配的空间全部都要消失，即把这个方法调用时分配给它的内存空间释放出来**，所以这个构造方法person调用完成之后，栈内存里面分配的两小块内存_id和_age自动消失了。这样就把它们所占的空间让了出来，让其他的方法去占用。**而new出来的对象则永远留在了堆内存里面。**


###重载

同一个类中的2个或2个以上的方法可以一同一个名字，只是它们的参数不同即可，在这种情况下，该方法就被称为重载，这个过程称为方法重载。

**构造方法可以重载.**

###this关键字

this一般出现在方法里面，当这个方法还没有调用的时候，this指的是谁并不知道。但是实际当中，你如果new了一个对象出来，那么this指的就是当前这个对象。
对哪个对象调用方法，this指的就是调用方法的这个对象（你对哪个对象调用这个方法，this指的就是谁）。
如果再new一个对象，这个对象他也有自己的this，他自己的this就当然指的是他自己了。

个人理解：
        作为方法参数的this，指的是调用/包括这个方法的对象
        在方法内部语句的this，指的是该方法对象
        在方法return位置的this，指的是调用该方法的对象
        在类内部的this，指的是该类对象

###super关键字

在JAVA类中**使用super来引用父类的成分，用this来引用当前对象**，如果一个类从另外一个类继承，我们new这个子类的实例对象的时候，这个子类对象里面会有一个父类对象。怎么去引用里面的父类对象呢？使用super来引用，this指的是当前对象的引用，super是当前对象里面的父对象的引用。

```
/**
 * 父类
 * @author gacl
 *
 */
class FatherClass {
    public int value;
    public void f() {
        value=100;
        System.out.println("父类的value属性值="+value);
    }
}

/**
 * 子类ChildClass从父类FatherClass继承
 * @author gacl
 *
 */
class ChildClass extends FatherClass {
    /**
     * 子类除了继承父类所具有的valu属性外，自己又另外声明了一个value属性，
     * 也就是说，此时的子类拥有两个value属性。
     */
    public int value;
    /**
     * 在子类ChildClass里面重写了从父类继承下来的f()方法里面的实现，即重写了f()方法的方法体。
     */
    public void f() {
        super.f();//使用super作为父类对象的引用对象来调用父类对象里面的f()方法
        value=200;//这个value是子类自己定义的那个valu，不是从父类继承下来的那个value
        System.out.println("子类的value属性值="+value);
        System.out.println(value);//打印出来的是子类自定义的那个value的值，这个值是200
        /**
         * 打印出来的是父类里面的value值，由于子类在重写从父类继承下来的f()方法时，
         * 第一句话“super.f();”是让父类对象的引用对象调用父类对象的f()方法，
         * 即相当于是这个父类对象自己调用f()方法去改变自己的value属性的值，由0变了100。
         * 所以这里打印出来的value值是100。
         */
        System.out.println(super.value);
    }
}

/**
 * 测试类
 * @author gacl
 *
 */
public class TestInherit {
    public static void main(String[] args) {
        ChildClass cc = new ChildClass();
        cc.f();
    }
}
```
运行结果：
[![](https://ae01.alicdn.com/kf/He0697c21b6ab4f6997795f6a5d451eeen.png)](https://ae01.alicdn.com/kf/He0697c21b6ab4f6997795f6a5d451eeen.png)

**画内存分析图了解程序执行的整个过程:**
分析任何程序都是从main方法的第一句开始分析的，所以首先分析main方法里面的第一句话:

    ChlidClass cc = new ChlidClass();

程序执行到这里时，首先在栈空间里面会产生一个变量cc，cc里面的值是什么这不好说，总而言之，通过这个值我们可以找到new出来的ChlidClass对象。
由于子类ChlidClass是从父类FatherClass继承下来的，所以当我们new一个子类对象的时候，这个子类对象里面会包含有一个父类对象，而这个父类对象拥有他自身的属性value。
这个value成员变量在FatherClass类里面声明的时候并没有对他进行初始化，所以系统默认给它初始化为0，成员变量（在类里面声明）在声明时可以不给它初始化，编译器会自动给这个成员变量初始化，但局部变量（在方法里面声明）在声明时一定要给它初始化，因为编译器不会自动给局部变量初始化，任何变量在使用之前必须对它进行初始化。

子类在继承父类value属性的同时，自己也单独定义了一个value属性，所以当我们new出一个子类对象的时候，这个对象会有两个value属性，一个是从父类继承下来的value，另一个是自己的value。在子类里定义的成员变量value在声明时也没有给它初始化，所以编译器默认给它初始化为0。因此，执行完第一句话以后，系统内存的布局如下图所示：
[![](https://ae01.alicdn.com/kf/Hc90256e2424f4fbba961ba779084bdab2.png)](https://ae01.alicdn.com/kf/Hc90256e2424f4fbba961ba779084bdab2.png)

接下来执行第二句话：

    cc.f();

当new一个对象出来的时候，这个对象会产生一个this的引用，这个this引用指向对象自身。如果new出来的对象是一个子类对象的话，那么这个子类对象里面还会有一个super引用，这个super指向当前对象里面的父对象。所以相当于程序里面有一个this，this指向对象自己，还有一个super，super指向当前对象里面的父对象。

　　这里调用重写之后的f()方法，方法体内的第一句话：“super.f();”是让这个子类对象里面的父对象自己调用自己的f()方法去改变自己value属性的值，父对象通过指向他的引用super来调用自己的f()方法，所以执行完这一句以后，父对象里面的value的值变成了100。接着执行“value=200；”这里的vaule是子类对象自己声明的value，不是从父类继承下来的那个value。所以这句话执行完毕后，子类对象自己本身的value值变成了200。此时的内存布局如下图所示：
[![](https://ae01.alicdn.com/kf/H718e958e95ad467eb2027dc3cc79aecaR.png)](https://ae01.alicdn.com/kf/H718e958e95ad467eb2027dc3cc79aecaR.png)

方法体内的最后三句话都是执行打印value值的命令，前两句打印出来的是子类对象自己的那个value值，因此打印出来的结果为200，最后一句话打印的是这个子类对象里面的父类对象自己的value值，打印出来的结果为100。

　　到此，整个内存分析就结束了，最终内存显示的结果如上面所示。
　　
##继承

###实现

###重写父类方法

###抽象类

###接口



##多态

###实现与应用

###类型转换


##关键字

###public/private/protected

###final

###static

