## 常用类

#### String，StringBuffer及StringBuilder

参考地址：https://blog.csdn.net/itchuxuezhe_yang/article/details/89966303

##### **Java中的String**

String的开辟方式：内存中主要以存储堆栈的形式来存储数据，在堆中存放的是对象和引用类型的数据，在栈中存放的是值类型的数据，除此之外，在String的存储地方还有常量池，在常量池中存放的是声明并赋值但并为实例化的String类型，String中有一个方法intern()可以将存放在堆中的值转化为存放在常量池中的值。

##### **StringBuffer和StringBuilder的相同点**

1.StringBuffer和StringBuilder都继承了AbstractStringBuilder
2.都需要实例化创建对象
3.由于继承了同一个类,所有从父类继承的方法相同

##### **StringBuffer和StringBuilder的区别**

**1.线程安全上**：StringBuffer是用于解决大量拼接时产生很多中间对象问题而提供的一个类，是线程安全的，但StringBuilder不是线程安全的。

**2.使用的情况**：

1. 在字符串不经常发生的变化的业务场景优先使用String
2. 在单线程下,比如有大量的字符创操作的情况下,应该使用StringBuilder
3. 在多线程下,有大量的字符串大的操作的情况下,应该使用StringBuffer

**3.运算速度**：StringBuilder>StringBuffer>String

**4.声明后的状态**：

1. String是final类不能被继承且为字符串常量，
   而StringBuffer和StringBuilder均为字符串变量,
2. String对象一旦被创建便不更改,而StringBuilder和StringBuffer是可变的。

#### 整数常量池

##### **Java中整数常量池的概念：**

java中为了提高程序的执行效率，将[-128, 127]之间256个整数所有的包装对象提前创建好了，类加载时就已经创好了，放在了一个方法区的“整数常量池”当中。

目的是：如果一个整数范围在[-128, 127]里面的整数进行包装，包装时不需要再new对象了，直接从“整数常量池”中取出来。

池：就是缓存区的意思。缓存区的好处是：程序用起来执行很快，很方便。缺点是：如果没用到，就有点耗费了内存。

翻阅Integer的源代码，发现有一个私有静态内部类IntegerCache。
这个IntegerCache类中，有一个static final修饰的常量数组，不可改变的，名叫Integer[] cache。
这个cache数组保存了[-128, 127]所有整数的包装类地址，存在方法区的整数常量池当中。

##### **总结：**

范围在[-128, 127]内的整数，装箱成包装类时，底层不会new对象。共用在整数常量池当中的256个Integer对象。

例如：
Integer a = -128;
Integer b = -128;
则a和b指向整数常量池中的同一个对象。