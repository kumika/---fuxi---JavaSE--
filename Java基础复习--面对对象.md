#Java基础复习--面对对象


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

 - 子类拥有父类非 private 的属性、方法。
 - 子类可以拥有自己的属性和方法，即子类可以对父类进行扩展。
 - 子类可以用自己的方式实现父类的方法。
 - Java 的继承是单继承，但是可以多重继承，单继承就是一个子类只能继承一个父类，多重继承就是，例如 A 类继承 B 类，B 类继承 C
   类，所以按照关系就是 C 类是 B 类的父类，B 类是 A 类的父类，这是 Java 继承区别于 C++ 继承的一个特性。
 - 提高了类之间的耦合性（继承的缺点，耦合度高就会造成代码之间的联系越紧密，代码独立性越差）。


**继承关键字:**

    extends 关键字
    implements 关键字

###实现

公共父类：
```
public class Animal { 
    private String name;  
    private int id; 
    public Animal(String myName, int myid) { 
        name = myName; 
        id = myid;
    } 
    public void eat(){ 
        System.out.println(name+"正在吃"); 
    }
    public void sleep(){
        System.out.println(name+"正在睡");
    }
    public void introduction() { 
        System.out.println("大家好！我是"         + id + "号" + name + "."); 
    } 
}
```

企鹅类：
```
public class Penguin extends Animal { 
    public Penguin(String myName, int myid) { 
        super(myName, myid); 
    } 
}
```
老鼠类：
```
public class Mouse extends Animal { 
    public Mouse(String myName, int myid) { 
        super(myName, myid); 
    } 
}
```


###重写父类方法
方法的重写要遵循“两同两小一大”规则：

    “两同”即方法名相同、形参列表相同
    “两小”指的是子类方法返回值类型应比父类方法返回值类型更小或更像等，子类方法声明抛出的异常类应比父类方法声明抛出的异常类更小或相等；
    “一大”指的是子类方法的访问权限应比父类方法的访问权限更大或者相等。


例子：
父类：
```
public class Bird {
    //Bird 类的fly（）方法
    public void fly()
    {
        System.out.println("我在天空可劲的飞啊");
    }
}
```
子类：
```
public class Ostrich {
    // 重写Bird类的fly（）方法
    public void fly() {
        System.out.println("我能在地上可劲跑");
 
    }
 
    public static void main(String[] args) {
        // 创建Ostrich对象
        Ostrich os = new Ostrich();
        // 执行Ostrich对象的fly（）方法，将输出“我在地上可劲的跑”
        os.fly();
    }
}
```
结果输出：
我能在地上可劲跑



###抽象类
例子：
```
/**
 * 父类Animal
 * 在class的前面加上abstract，即声明成这样：abstract class Animal
 * 这样Animal类就成了一个抽象类了
 */
abstract class Animal {

    public String name;

    public Animal(String name) {
        this.name = name;
    }
    
    /**
     * 抽象方法
     * 这里只有方法的定义，没有方法的实现。
     */
    public abstract void enjoy(); 
    
}
```
Java语言规定，当一个类里面有抽象方法的时候，这个类必须被声明为抽象类。

　　子类继承父类时，如果这个父类里面有抽象方法，并且子类觉得可以去实现父类的所有抽象方法，那么子类必须去实现父类的所有抽象方法，如：
　　
```
/**
 * 子类Dog继承抽象类Animal，并且实现了抽象方法enjoy
 * @author gacl
 *
 */
class Dog extends Animal {
    /**
     * Dog类添加自己特有的属性
     */
    public String furColor;

    public Dog(String n, String c) {
        super(n);//调用父类Animal的构造方法
        this.furColor = c;
    }

    @Override
    public void enjoy() {
        System.out.println("狗叫....");
    }

}
```

这个父类里面的抽象方法，子类如果觉得实现不了，那么把就子类也声明成一个抽象类，如：
```
/**
 * 这里的子类Cat从抽象类Animal继承下来，自然也继承了Animal类里面声明的抽象方法enjoy()，
 * 但子类Cat觉得自己去实现这个enjoy()方法也不合适，因此它把它自己也声明成一个抽象的类，
 * 那么，谁去实现这个抽象的enjoy方法，谁继承了子类，那谁就去实现这个抽象方法enjoy()。
 * @author gacl
 *
 */
abstract class Cat extends Animal {

    /**
     * Cat添加自己独有的属性
     */
    public String eyeColor;

    public Cat(String n, String c) {
        super(n);//调用父类Animal的构造方法
        this.eyeColor = c;
    }
}
```
这里的子类Cat从抽象类Animal继承下来，自然也继承了Animal类里面声明的抽象方法enjoy()，但子类Cat觉得自己去实现这个enjoy()方法也不合适，因此它把它自己也声明成一个抽象的类，那么，谁去实现这个抽象的enjoy方法，谁继承了子类，那谁就去实现这个抽象方法enjoy()。如：

```
/**
 * 子类BlueCat继承抽象类Cat，并且实现了从父类Cat继承下来的抽象方法enjoy
 * @author gacl
 *
 */
class BlueCat extends Cat {

    public BlueCat(String n, String c) {
        super(n, c);
    }

    /**
     * 实现了抽象方法enjoy
     */
    @Override
    public void enjoy() {
        System.out.println("蓝猫叫...");
    }
    
}
```


###接口
接口的本质——接口是一种特殊的抽象类，这种抽象类里面只包含常量和方法的定义，而没有变量和方法的实现。
```
/**
 * java中定义接口
 */
public interface JavaInterfaces {

}
```


接口(interface)是一种特殊的抽象类，在这种抽象类里面，所有的方法都是抽象方法，并且这个抽象类的属性（即成员变量）都是声明成“public static final 类型 属性名”这样的，默认也是声明成“public static final”即里面的成员变量都是公共的、静态的，不能改变的。


```
package javastudy.summary;

/**
 * 这里定义了接口：Painter。 在Painter接口里面定义了paint()和eat()这两个抽象方法。
 * 
 * @author gacl
 * 
 */
interface Painter {
    public void eat();

    public void paint();
}

/**
 * 这里定义了两个接口：Singer 在Singer接口里面定义了sing()和sleep()这两个抽象方法。
 * 
 * @author gacl
 * 
 */
interface Singer {
    public void sing();

    public void sleep();
}

/**
 * 类Student实现了Singer这个接口
 * 
 * @author gacl
 * 
 */
class Student implements Singer {

    private String name;

    public Student(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    /**
     * 实现接口中定义的sing方法
     */
    @Override
    public void sing() {
        System.out.println("student is singing");
    }

    /**
     * 实现接口中定义的sleep方法
     */
    @Override
    public void sleep() {
        System.out.println("student is sleeping");
    }

    public void study() {
        System.out.println("Studying...");
    }

}

/**
 * Teacher这个类实现了两个接口：Singer和Painter。 这里Teacher这个类通过实现两个不相关的接口而实现了多重继承。
 * 
 * @author gacl
 * 
 */
class Teacher implements Singer, Painter {

    private String name;

    public Teacher(String name) {
        this.name = name;
    }

    /**
     * 在Teacher类里面重写了这两个接口里面的抽象方法，
     * 通过重写抽象方法实现了这两个接口里面的抽象方法。
     */
    @Override
    public void eat() {
        System.out.println("teacher is eating");
    }

    public String getName() {
        return name;
    }

    @Override
    public void paint() {
        System.out.println("teacher is painting");
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    public void sing() {
        System.out.println("teacher is singing");
    }

    @Override
    public void sleep() {
        System.out.println("teacher is sleeping");
    }

    public void teach() {
        System.out.println("teaching...");
    }
}

public class TestInterfaces {

    public static void main(String[] args) {
        /**
         * 这里定义了一个接口类型的变量s1
         */
        Singer s1 = new Student("le");
        s1.sing();
        s1.sleep();
        Singer s2 = new Teacher("steven");
        s2.sing();
        s2.sleep();
        Painter p1 = (Painter)s2;
        p1.paint();
        p1.eat();
    }
}
```
这里验证了两个规则，**“一个类可以实现多个无关的接口”**，Teacher类既实现了Singer接口，同时也实现了Painter接口，而Singer接口和Painter接口是无关系的两个接口。**“多个无关的类可以实现同一接口”**，Student类和Teacher类都实现了Singer接口，而Student类和Teacher类并不是关系很密切的两个类，可以说是无关的两个类。



接口更进一步的特性

```
package javastudy.summary;

/**
 * 把“值钱的东西”这个类定义成一个接口Valuable。在接口里面定义了一个抽象方法getMoney()
 * @author gacl
 *
 */
interface Valuable {
    public double getMoney();
}

/**
 * 把“应该受到保护的东西”这个类定义成一个接口Protectable。
 * 在接口里面定义了一个抽象方法beProtected();
 * @author gacl
 *
 */
interface Protectable {
    public void beProteced();
}

/**
 * 这里是接口与接口之间的继承，接口A继承了接口Protectable，
 * 因此自然而然地继承了接口Protectable里面的抽象方法beProtected()。
 * 因此某一类去实现接口A时，除了要实现接口A里面定义的抽象方法m()以外，
 * 还要实现接口A从它的父接口继承下来的抽象方法beProtected()。
 * 只有把这两个抽象方法都实现了才算是实现了接口A。
 * @author gacl
 *
 */
interface A extends Protectable {
    void m();
}

/**
 * 这里定义了一个抽象类Animal。
 * @author gacl
 *
 */
abstract class Animal {
    private String name;
    /**
     * 在Animal类里面声明了一个抽象方法enjoy()
     */
    abstract void enjoy();
}

/**
 * 这里是为了实现了我们原来的语义：
 * “金丝猴是一种动物”同时“他也是一种值钱的东西”同时“他也是应该受到保护的东西”。而定义的一个类GoldenMonKey。
 * 为了实现上面的语义，这里把“值钱的东西”这个类定义成了一个接口Valuable，
 * 把“应该受到保护的东西”这个类也定义成了一个接口Protectable。这样就可以实现多继承了。
 * GoldenMonKey类首先从Animal类继承，然后GoldenMonKey类再去实现Valuable接口和Protectable接口，
 * 这样就可以实现GoldenMonKey类同时从Animal类，Valuable类，Protectable类继承了，即实现了多重继承，
 * 实现了原来的语义。
 * @author gacl
 *
 */
class GoldenMonKey extends Animal implements Valuable,Protectable {

    /**
     * 在GoldenMoKey类里面重写了接口Protectable里面的beProtected()这个抽象方法，
     * 实现了接口Protectable。
     */
    @Override
    public void beProteced() {
        System.out.println("live in the Room");
    }

    /**
     * 在GoldenMoKey类里面重写了接口Valuable里面的getMoney()这个抽象方法，实现了接口Valuable。
     */
    @Override
    public double getMoney() {
        return 10000;
    }

    /**
     * 这里重写了从抽象类Animal继承下来的抽象方法enjoy()。
     * 实现了这抽象方法，不过这里是空实现，空实现也是一种实现。
     */
    @Override
    void enjoy() {
        
    }
    
    public static void test() {
        /**
         * 实际当中在内存里面我们new的是金丝猴，在金丝猴里面有很多的方法，
         * 但是接口的引用对象v能看到的就只有在接口Valuable里面声明的getMoney()方法，
         * 因此可以使用v.getMoney()来调用方法。而别的方法v都看不到，自然也调用不到了。
         */
        Valuable v = new GoldenMonKey();
        System.out.println(v.getMoney());
        /**
         * 把v强制转换成p，相当于换了一个窗口，通过这个窗口只能看得到接口Protectable里面的beProtected()方法
         */
        Protectable p = (Protectable)v;
        p.beProteced();
    } 
}

/**
 * 这里让Hen类去实现接口A，接口A又是从接口Protectable继承而来，接口A自己又定义了一个抽象方法m()，
 * 所以此时相当于接口A里面有两个抽象方法：m()和beProtected()。
 * 因此Hen类要去实现接口A，就要重写A里面的两个抽象方法，实现了这两个抽象方法后才算是实现了接口A。
 * @author gacl
 *
 */
class Hen implements A {

    @Override
    public void beProteced() {
        
    }

    @Override
    public void m() {
        
    }
    
}

/**
 * java中定义接口
 */
public class JavaInterfacesTest {

    public static void main(String[] args) {
        GoldenMonKey.test();
    }
}
```



接口总结：接口和接口之间可以相互继承，类和类之间可以相互继承，类和接口之间，只能是类来实现接口。


##多态

动态绑定，也叫多态


###实现与应用

```
package javastudy.summary;

class Animal {
    /**
     * 声明一个私有的成员变量name。
     */
    private String name;

    /**
     * 在Animal类自定义的构造方法
     * @param name
     */
    Animal(String name) {
        this.name = name;
    }

    /**
     * 在Animal类里面自定义一个方法enjoy
     */
    public void enjoy() {
        System.out.println("动物的叫声……");
    }
}

/**
 * 子类Cat从父类Animal继承下来，Cat类拥有了Animal类所有的属性和方法。
 * @author gacl
 *
 */
class Cat extends Animal {
    /**
     * 在子类Cat里面定义自己的私有成员变量
     */
    private String eyesColor;

    /**
     * 在子类Cat里面定义Cat类的构造方法
     * @param n
     * @param c
     */
    Cat(String n, String c) {
        /**
         * 在构造方法的实现里面首先使用super调用父类Animal的构造方法Animal(String name)。
         * 把子类对象里面的父类对象先造出来。
         */
        super(n);
        eyesColor = c;
    }

    /**
     * 子类Cat对从父类Animal继承下来的enjoy方法不满意，在这里重写了enjoy方法。
     */
    public void enjoy() {
        System.out.println("我养的猫高兴地叫了一声……");
    }
}

/**
 * 子类Dog从父类Animal继承下来，Dog类拥有了Animal类所有的属性和方法。
 * @author gacl
 *
 */
class Dog extends Animal {
    /**
     * 在子类Dog里面定义自己的私有成员变量
     */
    private String furColor;

    /**
     * 在子类Dog里面定义Dog类的构造方法
     * @param n
     * @param c
     */
    Dog(String n, String c) {
        /**
         * 在构造方法的实现里面首先使用super调用父类Animal的构造方法Animal(String name)。
         * 把子类对象里面的父类对象先造出来。
         */
        super(n);
        furColor = c;
    }

    /**
     * 子类Dog对从父类Animal继承下来的enjoy方法不满意，在这里重写了enjoy方法。
     */
    public void enjoy() {
        System.out.println("我养的狗高兴地叫了一声……");
    }
}

/**
 * 子类Bird从父类Animal继承下来，Bird类拥有Animal类所有的属性和方法
 * @author gacl
 *
 */
class Bird extends Animal {
    /**
     * 在子类Bird里面定义Bird类的构造方法
     */
    Bird() {
        /**
         * 在构造方法的实现里面首先使用super调用父类Animal的构造方法Animal(String name)。
         * 把子类对象里面的父类对象先造出来。
         */
        super("bird");
    }

    /**
     * 子类Bird对从父类Animal继承下来的enjoy方法不满意，在这里重写了enjoy方法。
     */
    public void enjoy() {
        System.out.println("我养的鸟高兴地叫了一声……");
    }
}

/**
 * 定义一个类Lady(女士)
 * @author gacl
 *
 */
class Lady {
    /**
     * 定义Lady类的私有成员变量name和pet
     */
    private String name;
    private Animal pet;

    /**
     * 在Lady类里面定义自己的构造方法Lady()，
     * 这个构造方法有两个参数，分别为String类型的name和Animal类型的pet，
     * 这里的第二个参数设置成Animal类型可以给我们的程序带来最大的灵活性，
     * 因为作为养宠物来说，可以养猫，养狗，养鸟，只要是你喜欢的都可以养，
     * 因此把它设置为父类对象的引用最为灵活。
     * 因为这个Animal类型的参数是父类对象的引用类型，因此当我们传参数的时候，
     * 可以把这个父类的子类对象传过去，即传Dog、Cat和Bird等都可以。
     * @param name
     * @param pet
     */
    Lady(String name, Animal pet) {
        this.name = name;
        this.pet = pet;
    }

    /**
     * 在Lady类里面自定义一个方法myPetEnjoy()
     * 方法体内是让Lady对象养的宠物自己调用自己的enjoy()方法发出自己的叫声。
     */
    public void myPetEnjoy() {
        pet.enjoy();
    }
}

public class TestPolymoph {
    public static void main(String args[]) {
        /**
         * 在堆内存里面new了一只蓝猫对象出来，这个蓝猫对象里面包含有一个父类对象Animal。
         */
        Cat c = new Cat("Catname", "blue");
        /**
         * 在堆内存里面new了一只黑狗对象出来，这个黑狗对象里面包含有一个父类对象Animal。
         */
        Dog d = new Dog("Dogname", "black");
        /**
         * 在堆内存里面new了一只小鸟对象出来，这个小鸟对象里面包含有一个父类对象Animal。
         */
        Bird b = new Bird();

        /**
         * 在堆内存里面new出来3个小姑娘，名字分别是l1，l2，l3。
         * l1养了一只宠物是c(Cat)，l2养了一只宠物是d(Dog)，l3养了一只宠物是b(Bird)。
         * 注意：调用Lady类的构造方法时，传递过来的c，d，b是当成Animal来传递的，
         * 因此使用c，d，b这三个引用对象只能访问父类Animal里面的enjoy()方法。
         */
        Lady l1 = new Lady("l1", c);
        Lady l2 = new Lady("l2", d);
        Lady l3 = new Lady("l3", b);
        /**
         * 这三个小姑娘都调用myPetEnjoy()方法使自己养的宠物高兴地叫起来。
         */
        l1.myPetEnjoy();
        l2.myPetEnjoy();
        l3.myPetEnjoy();
    }
}
```
运行结果：

    我养的猫高兴地叫了一声
    我养的狗高兴地叫了一声
    我养的鸟高兴地叫了一声


**内存图理解动态绑定（多态）**
首先从main方法的第一句话开始分析：

    Cat c = new Cat("Catname","blue");

当Cat(String n,String c)构造方法调用结束后，真真正正在堆内存里面new出了一只Cat，这只Cat里面包含有父类对象Animal，这个Animal对象有自己的属性name，name属性的值为调用父类构造方法时传递过来的字符串Catname。除此之外，这只Cat还有自己的私有成员变量eyesColor，eyesColor属性的属性值为调用子类构造方法时传递过来的字符串blue。所以执行完这句话以后，内存中的布局是栈内存里面有一个引用c，c指向堆内存里面new出来的一只Cat，而这只Cat对象里面又包含有父类对象Animal，Animal对象有自己的属性name，属性值为Catname，Cat除了拥有从Animal类继承下来的name属性外，还拥有一个自己私有的属性eyesColor，属性值为blue。这就是执行完第一句话以后整个内存布局的情况如下图所示：

[![](https://ae01.alicdn.com/kf/Hba26121d6e054bb9890cdab366ad77714.png)](https://ae01.alicdn.com/kf/Hba26121d6e054bb9890cdab366ad77714.png)

接着看这句话：

    Lady l1 = new Lady(“l1”,c);


[![](https://ae01.alicdn.com/kf/Hf425ca3e75d44260b7c65658eeced670c.png)](https://ae01.alicdn.com/kf/Hf425ca3e75d44260b7c65658eeced670c.png)

主要理解：
        在Cat对象里面的animal类对象的方法，在经过cat类重写后，animal类方法的地址就指向cat类方法的地址（方法的地址被改变了），这样才能**实现动态绑定，实现父类指向子类对象，使程序的可扩展性达到了最好**


**动态绑定（多态）这种机制能帮助我们做到这一点——让程序的可扩展性达到极致。因此动态绑定是面向对象的核心，如果没有动态绑定，那么面向对象绝对不可能发展得像现在这么流行，所以动态绑定是面向对象核心中的核心。**

####总结动态绑定（多态）：
-----------

动态绑定是指在“执行期间”（而非编译期间）判断所引用的实际对象类型，根据其实际的类型调用其相应的方法。所以实际当中找要调用的方法时是动态的去找的，new的是谁就找谁的方法，这就叫动态绑定。动态绑定帮助我们的程序的可扩展性达到了极致。

多态的存在有三个必要的条件：

 1. 要有继承（两个类之间存在继承关系，子类继承父类）
 2. 要有重写（在子类里面重写从父类继承下来的方法）
 3. 父类引用指向子类对象

　　这三个条件一旦满足，当你调用父类里面被重写的方法的时候，实际当中new的是哪个子类对象，就调用子类对象的方法（这个方法是从父类继承下来后重写后的方法）。

###类型转换

**父类引用指向子类对象的时候，它看到的只是作为父类的那部分所拥有的属性和方法，至于作为子类的那部分它没有看到。**

```
package javastudy.summary;

/**
 * 父类Animal
 * @author gacl
 *
 */
class Animal {

    public String name;

    public Animal(String name) {
        this.name = name;
    }
}

/**
 * 子类Cat继承Animal
 * @author gacl
 *
 */
class Cat extends Animal {

    /**
     * Cat添加自己独有的属性
     */
    public String eyeColor;

    public Cat(String n, String c) {
        super(n);//调用父类Animal的构造方法
        this.eyeColor = c;
    }
}

/**
 * 子类Dog继承Animal
 * @author gacl
 *
 */
class Dog extends Animal {
    /**
     * Dog类添加自己特有的属性
     */
    public String furColor;

    public Dog(String n, String c) {
        super(n);//调用父类Animal的构造方法
        this.furColor = c;
    }

}

/**
 * 下面是这三个类的测试程序
 * @author gacl
 *
 */
public class TestClassCast {

    /**
     * @param args
     */
    public static void main(String[] args) {

        Animal a = new Animal("name");
        Cat c = new Cat("catname","blue");
        Dog d = new Dog("dogname", "black");
        /**
         * a instanceof Animal这句话的意思是a是一只动物吗？
         * a是Animal这个类里面的是一个实例对象，所以a当然是一只动物，其结果为true。
         */
        System.out.println(String.format("a instanceof Animal的结果是%s",a instanceof Animal));//true
        /**
         * c是Cat类的实例对象的引用，即c代表的就是这个实例对象，
         * 所以“c是一只动物”打印出来的结果也是true。
         * d也一样，所以“d是一只动物”打印出来的结果也是true。
         */
        System.out.println(String.format("c instanceof Animal的结果是%s",c instanceof Animal));//true
        System.out.println(String.format("d instanceof Animal的结果是%s",d instanceof Animal));//true
        /**
         * 这里判断说“动物是一只猫”，不符合逻辑，所以打印出来的结果是false。
         */
        System.out.println(String.format("a instanceof Cat的结果是%s",a instanceof Cat));
        /**
         * 这句话比较有意思了，a本身是Animal类的实例对象的引用，
         * 但现在这个引用不指向Animal类的实例对象了，而是指向了Dog这个类的一个实例对象了，
         * 这里也就是父类对象的引用指向了子类的一个实例对象。
         */
        a = new Dog("bigyellow", "yellow");
        System.out.println(a.name);//bigyellow
        /**
         * 这里的furColor属性是子类在继承父类的基础上新增加的一个属性，是父类没有的。
         * 因此这里使用父类的引用对象a去访问子类对象里面新增加的成员变量是不允许的，
         * 因为在编译器眼里，你a就是Animal类对象的一个引用对象，你只能去访问Animal类对象里面所具有的name属性，
         * 除了Animal类里面的属性可以访问以外，其它类里面的成员变量a都没办法访问。
         * 这里furColor属性是Dog类里面的属性，因此你一个Animal类的引用是无法去访问Dog类里面的成员变量的，
         * 尽管你a指向的是子类Dog的一个实例对象，但因为子类Dog从父类Animal继承下来，
         * 所以new出一个子类对象的时候，这个子类对象里面会包含有一个父类对象，
         * 因此这个a指向的正是这个子类对象里面的父类对象，因此尽管a是指向Dog类对象的一个引用，
         * 但是在编译器眼里你a就是只是一个Animal类的引用对象，你a就是只能访问Animal类里面所具有的成员变量，
         * 别的你都访问不了。
         * 因此一个父类(基类)对象的引用是不可以访问其子类对象新增加的成员(属性和方法)的。
         */
        //System.out.println(a.furColor);
        System.out.println(String.format("a指向了Dog，a instanceof Animal的结果是%s",a instanceof Animal));//true
        /**
         * 这里判断说“a是一只Dog”是true。
         * 因为instanceof探索的是实际当中你整个对象到底是什么东西，
         * 并不是根据你的引用把对象看出什么样来判断的。
         */
        System.out.println(String.format("a instanceof Dog的结果是%s",a instanceof Dog));//true
        /**
         * 这里使用强制转换，把指向Animal类的引用对象a转型成指向Dog类对象的引用，
         * 这样转型后的引用对象d1就可以直接访问Dog类对象里面的新增的成员了。
         */
        Dog d1 = (Dog)a;
        System.out.println(d1.furColor);//yellow
    }

}
```

##关键字

###public/private/protected

[![](https://ae01.alicdn.com/kf/H28215b1142e64a998e7eb451998dc765u.jpg)](https://ae01.alicdn.com/kf/H28215b1142e64a998e7eb451998dc765u.jpg)

访问的权限：

    private 在本类
    
    protect 在本类   同一个包    继承类
    
    public  在本类   同一个包    继承类    其他类


###final

 - 修饰变量

凡是对成员变量或者局部变量(在方法中的或者代码块中的变量称为本地变量)声明为final的都叫作final变量。final变量经常和static关键字一起使用，作为常量。

final修饰基本数据类型的变量时，必须赋予初始值且不能被改变，修饰引用变量时，该引用变量不能再指向其他对象

 - 修饰方法

final也可以声明方法。方法前面加上final关键字，代表这个方法不可以被子类的方法重写。如果你认为一个方法的功能已经足够完整了，子类中不需要改变的话，你可以声明此方法为final。final方法比非final方法要快，因为在编译的时候已经静态绑定了，不需要在运行时再动态绑定。

 - 修饰类

使用final来修饰的类叫作final类。final类通常功能是完整的，它们不能被继承。Java中有许多类是final的，譬如String, Interger以及其他包装类。

###static

[![](https://ae01.alicdn.com/kf/Hff1c068095db4f90bad8f6a20a3bb3afD.png)](https://ae01.alicdn.com/kf/Hff1c068095db4f90bad8f6a20a3bb3afD.png)
