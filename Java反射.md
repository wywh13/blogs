## Java反射

​    Java的反射机制是指在运行状态中，对于任意一个类都能够知道这个类所有的属性和方法；并且对于任意一个对象，都能够调用它的任意一个方法；这种动态获取信息以及动态调用对象方法的功能称为 Java 语言的反射机制。

### 1. Java反射原理

​    在Java中，每一个类被虚拟机加载后都会在内存中创建一个该类的Class对象，用于存储该类的各种信息，Class类的构造函数被设计为私有的，这意味着我们不能通过new的方式来创建Class对象，只有JVM才能创建该类的实例。通过获取某个类的Class对象，可以获取该类的成员变量、成员方法，以及创建该类的对象。获取Class对象有以下3种方法：

1. 使用类名.class获取Class对象(使用这种办法生成Class类对象时，不会使JVM自动加载该类)。例如：

   ```java
   Class clazz = String.class;
   ```

2. 使用对象.getClass()方法获取Class对象。例如：

   ```java
   String str = "hello";
   Class clazz = str.getClass();
   ```

3. 使用Class.forName()方法获取Class对象,参数为完整类名(最常用/最安全/性能最好)。例如：

   ```java
   Class Clazz = Class.forName("java.lang.String");
   ```

获取到Class对象后，可以通过Class对象提供的方法来获取类的各项信息，以下为Class类的几个常用方法。

```java
        //返回一个包含Field对象的数组，存放该类或接口的所有可访问公共属性(含继承的公共属性)
        Field[] fields = clazz1.getFields();
        //返回一个包含Field对象的数组，存放该类或接口的所有属性(不含继承的属性)
        Field[] declaredFieldsfields = clazz1.getDeclaredFields();
        //返回一个指定公共属性名的Field对象
        Field field = clazz1.getField("CASE_INSENSITIVE_ORDER");
        //返回一个包含Method对象的数组，存放该类或接口的所有可访问公共方法(含继承的公共方法)
        Method[] methods = clazz1.getMethods();
        //返回一个包含Method对象的数组，存放该类或接口的所有方法(不含继承的方法)
        Method[] declaredMethods = clazz1.getDeclaredMethods();
        //返回一个包含Constructor对象的数组，存放该类的所有公共构造方法
        Constructor[] constructors = clazz1.getConstructors();
        //返回该类默认构造方法的对象
        Constructor declaredConstructor = clazz1.getDeclaredConstructor();
        //返回一个指定参数列表的Constructor对象
        Constructor constructor = clazz1.getConstructor(clazz1);
        //返回一个包含Class对象的数组，存放该类或接口实现的接口
        Class[] interfaces = clazz1.getInterfaces();
        //创建该类的一个新实例
        Object o = clazz1.getDeclaredConstructor(clazz1).newInstance("hello!");
        //获取该类的完整名
        System.out.println(clazz1.getName().toString());
```

### 2. Java反射的优缺点

优点：

1. 可以在运行时获取和操作类的信息，提高了程序的灵活性，使程序具有更好的灵活性和扩展性。
2. 可以在运行时轻松获取任意一个类的方法、属性，并且还能通过反射进行动态调用。
3. 提高代码的复用率，比如动态代理、spring管理bean，就是基于反射来实现的。

缺点：

1. Java反射机制是通过运行时动态获取和操作类的信息，涉及到动态类型的解析，所以jvm无法对这些代码进行优化，导致性能要比非反射调用更低性能较低。
2. Java反射可以绕过一些限制访问的属性或者方法，破坏了代码本身的抽象性，可能会产生安全性问题。