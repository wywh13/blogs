#### 增强for循环

1. 增强for循环的作用： 简化迭代器的书写格式。(注意：增强for循环的底层还是使用了迭代器遍历。)

2. 增强for循环的适用范围： 如果是实现了Iterable接口的对象或者是数组对象都可以使用增强for循环。

3. 增强for循环的缺点：增强for循环和iterator遍历的效果是一样的，也就说增强for循环的内部也就是调用iteratoer实现的，但是增强for循环有些缺点，例如不能在增强循环里动态的删除集合内容、不能获取下标等。

4. 增强for循环的格式：for(数据类型 变量名 :遍历的目标){ }//数据类型 变量名:声明一个变量用来接收遍历目标遍历后的元素。

   

#### return

return在方法中的主要作用有两点：
1.返回方法指定类型值。
2.用于方法结束的标志,return 后面的语句不会被执行。即当方法中出现return语句时，退出该方法。



#### 值传递与引用传递（Java只有值传递）

值传递是对基本型变量而言的,传递的是该变量的一个副本,改变副本不影响原变量.
引用传递一般是对于对象型变量而言的,传递的是该对象地址的一个副本, 并不是原对象本身 。
一般认为,java内的基础类型数据传递都是值传递. java中实例对象的传递是引用传递

首先，不要纠结于 Pass By Value 和 Pass By Reference 的字面上的意义，否则很容易陷入所谓的“一切传引用其实本质上是传值”这种并不能解决问题无意义论战中。
更何况，要想知道Java到底是传值还是传引用，起码你要先知道传值和传引用的准确含义吧？可是如果你已经知道了这两个名字的准确含义，那么你自己就能判断Java到底是传值还是传引用。
这就好像用大学的名词来解释高中的题目，对于初学者根本没有任何意义。

一：搞清楚 基本类型 和 引用类型的不同之处
int num = 10;
String str = "hello";
![img](https://img-blog.csdnimg.cn/20190604110532597.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwNTc0NTcx,size_16,color_FFFFFF,t_70)

如图所示，num是基本类型，值就直接保存在变量中。而str是引用类型，变量中保存的只是实际对象的地址。一般称这种变量为"引用"，引用指向实际对象，实际对象中保存着内容。

二：搞清楚赋值运算符（=）的作用
num = 20;
str = "java";
![img](https://img-blog.csdnimg.cn/20190604110633208.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwNTc0NTcx,size_16,color_FFFFFF,t_70)
对于基本类型 num ，赋值运算符会直接改变变量的值，原来的值被覆盖掉。
对于引用类型 str，赋值运算符会改变引用中所保存的地址，原来的地址被覆盖掉。但是原来的对象不会被改变（重要）。
如上图所示，“hello” 字符串对象没有被改变。（没有被任何引用所指向的对象是垃圾，会被垃圾回收器回收）

三：调用方法时发生了什么？参数传递基本上就是赋值操作。
链接：https://www.nowcoder.com/questionTerminal/b296e9e1c40542ec8677c1e452b6b576
来源：牛客网

第一个例子：基本类型

```
void foo(int value) {
    value = 100;
}
foo(num); // num 没有被改变
```


第二个例子：没有提供改变自身方法的引用类型

```
void foo(String text) {
    text = "windows";
}
foo(str); // str 也没有被改变
第三个例子：提供了改变自身方法的引用类型
```

```
StringBuilder sb = new StringBuilder("iphone");
void foo(StringBuilder builder) {
    builder.append("4");
}
foo(sb); // sb 被改变了，变成了"iphone4"。
```


第四个例子：提供了改变自身方法的引用类型，但是不使用，而是使用赋值运算符。

```
StringBuilder sb = new StringBuilder("iphone");
void foo(StringBuilder builder) {
    builder = new StringBuilder("ipad");
}
foo(sb); // sb 没有被改变，还是 "iphone"。
```


重点理解为什么，第三个例子和第四个例子结果不同？

下面是第三个例子的图解：

![img](https://img-blog.csdnimg.cn/20190604110822161.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwNTc0NTcx,size_16,color_FFFFFF,t_70)

`builder.append(“4”`)之后

![img](https://img-blog.csdnimg.cn/20190604110838172.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwNTc0NTcx,size_16,color_FFFFFF,t_70)

下面是第四个例子的图解

![img](https://img-blog.csdnimg.cn/20190604110854297.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwNTc0NTcx,size_16,color_FFFFFF,t_70)

`builder = new StringBuilder(“ipad”);` 之后

![å¨è¿éæå¥å¾çæè¿°](https://img-blog.csdnimg.cn/20190604110911950.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwNTc0NTcx,size_16,color_FFFFFF,t_70)
（出自CSDN博主「never疯」，原文链接：https://blog.csdn.net/qq_40574571/java/article/details/90765349）

总结

1就 Java 语言本身来说，只有值传递，没有引用传递。
2.根据值传递，引用传递的定义来说：
        Java 中的基本类型，属于值传递。
        Java 中的引用类型，属于引用传递。
        Java 中的 String 及包装类，属于特殊群体，作为形参时，由于每次赋值都相当于重新创建了对象，因此看起来像值传递，但是其特性已经破坏了，值传递、引用传递的定义。因此他们属于引用传递的定义，却表现为值传递。
（出自CSDN博主「Eli Shaw」，原文链接：https://blog.csdn.net/xiaojinlai123/java/article/details/88678367）

