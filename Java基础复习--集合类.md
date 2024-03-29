﻿#Java基础复习--集合类



#集合类

 1. 集合类存放于java.util包中。
 2. 集合类存放的都是对象的引用，而非对象本身，出于表达上的便利，我们称集合中的对象就是指集合中对象的引用（reference)。
 3. 集合类型主要有3种：set(集）、list(列表）和map(映射)。
 4. 集合接口分为：Collection和Map，list、set实现了Collection接口

##Collection接口

List特点：
        
        元素有序；元素可以重复；元素都有索引（角标）。
List里存放的对象是有序的，同时也是可以重复的。
        List关注的是索引，拥有一系列和索引相关的方法，查询速度快， **因为往list集合里插入或删除数据时，会伴随着后面数据的移动，所以插入和删除数据速度慢。**


Set特点：

    元素无序；元素不可以重复； 

Set里存放的对象是无序，不能重复的，集合中的对象不按特定的方式排序，只是简单地把对象加入集合中。


Map特点：

    键值对；键不可以重复；值可以重复； 
    

Map集合中存储的是键值对，**键不能重复，值可以重复。**
根据键得到值，对map集合遍历时先得到键的set集合，对set集合进行遍历，得到相应的值。


###Iterator迭代器

Iterator是一个接口，作用是   **取**  集合中的元素，是一个**Collection系通用的取出元素的方法**。

**List系和Set系的对象里都有这个Iterator对象**。
比如arrayList的源码：
[![](https://ae01.alicdn.com/kf/He7d32f52860b4a3a82ea767aa4c0ce4fq.jpg)](https://ae01.alicdn.com/kf/He7d32f52860b4a3a82ea767aa4c0ce4fq.jpg)

Iterator重要的3个方法：

 - **hasNext()**

如果仍有元素可以迭代，则返回True

 - **next()**

返回迭代的下一个元素

 - **remove()**

从迭代器指向的collection中移除迭代器返回的最后一个元素

例子
```
    public static void main(String[] args) {
        Collection coo = new ArrayList();

        coo.add("adb");
        coo.add("adb2");
        coo.add("adb3");
        coo.add("adb4");
        coo.add("adb5");

        for (Iterator it = coo.iterator(); it.hasNext();) {
            System.out.println(it.next());
        }
    }
```


###List接口
总的特点：

**有序集合，允许相同元素和null**


####ArrayList

**非同步，允许相同元素和null，实现了动态大小的数组，遍历效率高，用的多**

```
//例子
    ArrayList<Integer> arrayList = new ArrayList<>();
    arrayList.add(1);
    arrayList.add(2);
    arrayList.add(1);
    arrayList.add(3);
    arrayList.add(2);
    arrayList.add(3);
 
    arrayList = new ArrayList<>(new HashSet<>(arrayList));
 
    for (int i=0;i<arrayList.size();i++){
        printlns("arrayList ["+ i +"] = "+arrayList.get(i));
    }
```
运行结果

    arrayList [0] = 1
    arrayList [1] = 2
    arrayList [2] = 3


####LinkedList

**非同步，允许相同元素和null，遍历效率低插入和删除效率高**

这个例子是和ArrayList比较插入数据时间
```
  public static void addTest(){

        //批量插入，每次都向首位插入数据；

        LinkedList linkedList = new LinkedList();

        long time1 = new Date().getTime();

        for(int m=0;m<300000;m++){
            linkedList.add(0,null);
        }

        long time2 = new Date().getTime();

        System.out.print("linkedList批量插入时间:"+(time2 - time1)+"ms\n");




        ArrayList arraylist = new ArrayList();

        long time3 = new Date().getTime();

        for(int n=0;n<300000;n++){
            arraylist.add(0, null);
        }

        long time4 = new Date().getTime();
        System.out.print("arrayList批量插入时间:"+(time4 - time3)+"ms\n");

    }
```
结果：

**linkedList批量插入时间:9ms
arrayList批量插入时间:3696ms**


####ArrayList和LinkList的使用

**Q：什么时候使用Arraylist，什么时候使用LinkedList？**
A：当你需要频繁查询数组的时候使用ArrayList比较快，当你需要频繁操作数组进行增删插入操作的时候使用LinkList比较合适。当然直接在末尾添加数据ArrayList用时也不是特别多，因为在末尾操作后面没有数据。

**Q：那什么时候适合用list呢**
A：涉及到“栈”、“队列”、“链表”等操作，应该考虑用List，具体的选择哪个List，根据下面的标准来取舍

对于需要快速插入，删除元素，应该使用LinkedList。
对于需要快速随机访问元素，应该使用ArrayList。
对于“单线程环境” 或者 “多线程环境，但List仅仅只会被单个线程操作”，此时应该使用非同步的类(如ArrayList)。
对于“多线程环境，且List可能同时被多个线程操作”，此时，应该使用同步的类(如Vector)。
**Q：线程安全的数组，什么是线程安全？**
A：线程安全就是当前资源只能被单独一个线程访问，也就是加同步锁的线程。默认的线程安全的list是Vector；




###Set接口

总的特点：

**不允许相同元素，最多有一个null元素**

####TreeSet

TreeSet是一个**有排序功能，且没有重复元素**的集合，**通过TreeMap实现**。

TreeSet的构造函数都是通过新建一个TreeMap作为实际存储Set元素的容器。因此得出结论: TreeSet的底层实际使用的存储容器就是TreeMap。
对于TreeMap而言，它采用一种被称为”红黑树”的排序二叉树来保存Map中每个Entry。每个Entry被当成”红黑树”的一个节点来对待。


TreeSet中含有一个"NavigableMap类型的成员变量"m，而m实际上是"TreeMap的实例"。


例子：
让元素自身具备比较排序功能，具备比较排序功能的元素只需要实现Comparable 接口。覆盖接口中CompareTo方法
```
public class TreeSetDemo {
 
	/**
	 * @param args
	 */
	public static void main(String[] args) {
 
		demo1();
	}
 
 
 
	/**
	 * 
	 */
	public static void demo1() {
		TreeSet ts = new TreeSet();
		
		ts.add("abc");
		ts.add("zaa");
		ts.add("aa");
		ts.add("nba");
		ts.add("cba");
		
		Iterator it = ts.iterator();
		
		while(it.hasNext()){
			System.out.println(it.next());
		}
	}
 
}
```
输出结果：

    aa
    abc
    cba
    nba
    zaa


####HashSet
**无序集合，不允许相同元素，最多有一个null元素**

例子：
2种方法遍历 
用Iterator来遍历
for循环遍历
```
package cn.java.text.Main;
import java.util.*;
public class Main4 {//HashSet
 
	public static void main(String[] args) {
		// TODO Auto-generated method stub
		Scanner ip = new Scanner(System.in);
        HashSet<String> hashSet = new HashSet<String>();
        int i;
        String aString;
        for(i = 0; i < 6; i++) {
        	aString = ip.next();
        	hashSet.add(aString);//加入列表
        }
        Iterator<String> iterator = hashSet.iterator();//遍历器
        while(iterator.hasNext())System.out.print(iterator.next()+" ");//判断是否有，有就输出
        String bString;
        bString =  ip.next();
        hashSet.remove(bString);//删除
        /*Iterator iterator2 = hashSet.iterator();
        while(iterator2.hasNext()) {//遍历器遍历
        	System.out.print(iterator2.next()+"  ");
        
        }*/
        for(String string: hashSet)System.out.print(string + " ");//for循环遍历
 
	}
 
}
```
结果：

    aa KK ff tt ff kk
    aa kk ff tt kk
    aa ff tt



###HashSet/TreeSet与HashMap/TreeMap之间的联系

[![](https://ae01.alicdn.com/kf/H9aadabf1ed184624bfc2b2efb8556d29H.jpg)](https://ae01.alicdn.com/kf/H9aadabf1ed184624bfc2b2efb8556d29H.jpg)

[![](https://ae01.alicdn.com/kf/H70d59821495c4109bc847bcb09c8071a1.jpg)](https://ae01.alicdn.com/kf/H70d59821495c4109bc847bcb09c8071a1.jpg)

Set内部干脆就放了一个map！
HashSet和TreeSet内部的很多方法实现，就是简单的调用了HashMap/TreeMap的方法

##Map接口

**没有实现collection接口，key不能重复，value可以重复，一个key映射一个value**

Map特点：
键值对；键不可以重复；值可以重复； Map集合中存储的是键值对，键不能重复，值可以重复。根据键得到值，对map集合遍历时先得到键的set集合，对set集合进行遍历，得到相应的值。

###TreeMap

**TreeMap是有序的,底层是红黑树,使用Comparator或者Comparable来比较key是否相等与排序的问题~**

因为没有学过红黑树，所以这里就敷衍的过去吧。

例子：
```
TreeMap<Integer, Integer> treeMap = new TreeMap<>();

    treeMap.put(1, 5);
    treeMap.put(2, 4);
    treeMap.put(3, 3);
    treeMap.put(4, 2);
    treeMap.put(5, 1);

    for (Entry<Integer, Integer> entry : treeMap.entrySet()) {

        String s = entry.getKey() +"大海，湖，河流" + entry.getValue();

        System.out.println(s);
    }
```


###HashMap
**实现Map接口，非同步，允许null作为key和value，用的多**

HashMap的方法有：

常用的方法有

    put(key,value),
    get(key),
    size(),
    containKey(key),
    containValue(value).


例子：
```
public class Main {
   public static void main(String[] args) {
      HashMap< String, String> hMap = 
      new HashMap< String, String>();
      hMap.put("1", "1st");
      hMap.put("2", "2nd");
      hMap.put("3", "3rd");
      Collection cl = hMap.values();
      Iterator itr = cl.iterator();
      while (itr.hasNext()) {
         System.out.println(itr.next());
     }
   }
}
```
结果：

    1st
    2nd
    3rd


另外的例子：
都是常用的方法

```
package map.Test;
 
import java.util.Collection;
import java.util.HashMap;
import java.util.Set;
 
public class HashMapDemo {
 
    public static void main(String[] args) {
        HashMap<String, String> map = new HashMap<String, String>();
        // 键不能重复，值可以重复
        map.put("san", "张三");
        map.put("si", "李四");
        map.put("wu", "王五");
        map.put("wang", "老王");
        map.put("wang", "老王2");// 老王被覆盖
        map.put("lao", "老王");
        System.out.println("-------直接输出hashmap:-------");
        System.out.println(map);
        /**
         * 遍历HashMap
         */
        // 1.获取Map中的所有键
        System.out.println("-------foreach获取Map中所有的键:------");
        Set<String> keys = map.keySet();
        for (String key : keys) {
            System.out.print(key+"  ");
        }
        System.out.println();//换行
        // 2.获取Map中所有值
        System.out.println("-------foreach获取Map中所有的值:------");
        Collection<String> values = map.values();
        for (String value : values) {
            System.out.print(value+"  ");
        }
        System.out.println();//换行
        // 3.得到key的值的同时得到key所对应的值
        System.out.println("-------得到key的值的同时得到key所对应的值:-------");
        Set<String> keys2 = map.keySet();
        for (String key : keys2) {
            System.out.print(key + "：" + map.get(key)+"   ");
 
        }
        /**
         * 另外一种不常用的遍历方式
         */
        // 当我调用put(key,value)方法的时候，首先会把key和value封装到
        // Entry这个静态内部类对象中，把Entry对象再添加到数组中，所以我们想获取
        // map中的所有键值对，我们只要获取数组中的所有Entry对象，接下来
        // 调用Entry对象中的getKey()和getValue()方法就能获取键值对了
        Set<java.util.Map.Entry<String, String>> entrys = map.entrySet();
        for (java.util.Map.Entry<String, String> entry : entrys) {
            System.out.println(entry.getKey() + "--" + entry.getValue());
        }
        
        /**
         * HashMap其他常用方法
         */
        System.out.println("after map.size()："+map.size());
        System.out.println("after map.isEmpty()："+map.isEmpty());
        System.out.println(map.remove("san"));
        System.out.println("after map.remove()："+map);
        System.out.println("after map.get(si)："+map.get("si"));
        System.out.println("after map.containsKey(si)："+map.containsKey("si"));
        System.out.println("after containsValue(李四)："+map.containsValue("李四"));
        //map.replace是jdk1.8方法
        System.out.println(map.replace("si", "李四2"));
        System.out.println("after map.replace(si, 李四2):"+map);
    }
 
}
```

###HashMap的put()源码浅析
[![](https://ae01.alicdn.com/kf/Hbe262020d3174f3ba4945f14b080582do.jpg)](https://ae01.alicdn.com/kf/Hbe262020d3174f3ba4945f14b080582do.jpg)

个人的理解：
    这段源码就只是告诉你
    
    哈希表是  **数组 + 链表**  组成的，2合1的结构表格
    Put的过程是把key放如数组 ，value放入链表的过程
    

这里面的判断语句，个人觉得有些累赘，有用但是麻烦。

#数组和集合都有哪些区别？
**数组特点：** 

    1. 数组本质上就是一段连续的内存空间，用于记录多个类型相同的数据； 
    2. 数据一旦声明完毕，则内存空间固定不变； 
    3. 插入和删除操作不方便，可能会移动大量的元素并导致效率太低； 
    4. 支持下标访问，可以实现随机访问； 
    5. 数组中的元素可以是基本数据类型，也可以使用引用数据类型。

**集合特点：** 

    1. 内存空间可以不连续，数据类型可以不相同； 
    2. 集合的内存空间可以动态的调整； 
    3. 集合的插入删除操作可以不移动大量元素； 
    4. 部分支持下标访问，部分不支持； 
    5. 集合中的元素必须是引用数据类型(你存储的是简单的int，它会自动装箱成Integer)；

可以看出数组和集合在数据的存储，访问，类型，长度等都有不同的地方。


#简单总结

##Collection系

[![](https://ae01.alicdn.com/kf/H376ab3e106d941a68f39f7e2f8345dfa9.jpg)](https://ae01.alicdn.com/kf/H376ab3e106d941a68f39f7e2f8345dfa9.jpg)


List接口：(允许有重复元素)
    

 - **ArrayList**:（查询多的情况下使用）
    底层数据结构： 数组Object[]    
    常用方法： **查询Get,遍历iterator，修改Set**
 - **LinkedList**：（增删多的情况下使用）
    底层数据结构： 环形双向链表
    常用方法： 增加add，删除remove



Set接口：（不允许有重复元素，保持唯一性）

 - **HashSet**
    底层数据结构： HashMap    


 - **TreeSet**
    底层数据结构：TreeMap


##Map系

[![](https://ae01.alicdn.com/kf/He8580339ffe04465a965683992449202h.jpg)](https://ae01.alicdn.com/kf/He8580339ffe04465a965683992449202h.jpg)


 - **HashMap**
    底层数据结构： 链表 + 数组 = 哈希表（元素是链表的数组）
    常用方法： **put(xxx,xxx) ,  get(XXX), remove(XXX);**

 - **TreeMap**
    底层数据结构：红黑树
    **可以实现按Key排序**

---------------------------------------
XJB不知道从哪里抄来的有点B用的总结
```
    /**
     *  数组和集合，都是干装载数据这工作，为什么集合就那么火啊？
     *
     *  答：
     *          差别在数组和集合的长度上。
     *          不同点：
     *                  1 长度的区别
     *                      集合的长度是可以增加，而且是自动扩容的
     *                      数组的长度是固定的
     *
     *                  2 内容不同
     *                      集合可以装不同类型的数据（一般不会）
     *                      数组一定是同一类型的元素
     *
     *                  3  数据的基本类型
     *                      集合只能装载 引用类型
     *                      数组可以装载 引用类型，也可以装载 基本类型
     *
     *           数组是手动增加容量，集合是自动增加容量，这样集合比数组方便，方便这点是最重要的。
     */
     
     
        /**
     *             集合就是容器，容器可以是茶壶，杯子等，那么杯子，茶壶等容器最重要的2个作用就是存，取
     *             所以，学习集合的时候就是要学习3个特点
     *                     集合它如何    存    数据（Collection体系用add() ，Map体系用put()）
     *                         它如何    取    数据（迭代器Iterator，增强for，List系还有普通for和ListIterator）
     *                         容器特点造成的一些问题
     */ 
```

写代码的重要思想：
```
    /**
     * 写代码：
     *          1       明确需求。 我要做什么？
     *          2       分析思路。 我要做什么？ 1，2，3.
     *          3       确定步骤。 每一个思路部分用到哪些语句，方法，对象。
     *          4       代码实现。 用具体的Java语言代码把思路体现出来。
     *
     *
     * 学习新技术的4点：
     *          1，该技术是什么？
     *          2，该技术有什么特点（使用注意）
     *          3，该技术怎么使用。demo
     *          4，该技术什么时候用？ test
     */
```



#参考：

https://zhuanlan.zhihu.com/p/60374460（大体参考知识框架）


https://blog.csdn.net/weixin_42139757/article/details/82108515(arrayList和LinkList的比较)

https://www.jianshu.com/p/12f4dbdbc652（treeSet）
https://blog.csdn.net/zhanshixiang/article/details/82492872（TreeSet的例子)


https://www.jianshu.com/p/e11fe1760a3d(treeMap)
https://zhuanlan.zhihu.com/p/35598760(treeMap)
https://blog.csdn.net/cyywxy/article/details/81151104（TreeMap和红黑树的解析）


https://zhuanlan.zhihu.com/p/21673805(HashMap)
https://www.cnblogs.com/weiziqiang/p/8947134.html(HashMap结构方法)
https://blog.csdn.net/qq_40374604/article/details/83057061（hashMap的例子)


https://zhuanlan.zhihu.com/p/36791608（上）
https://zhuanlan.zhihu.com/p/36797326（下，深入的理解HashSet和HashMap的区别）


https://www.cnblogs.com/java-zhao/p/5112542.html（小简单总结）

https://www.cnblogs.com/java-zhao/p/5112542.html（总结参考）
https://www.cnblogs.com/java-zhao/p/5101695.html（总结参考）
