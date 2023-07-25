## JDK，JRE与JVM

![img](https://ss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=2855240162,1683693815&fm=26&gp=0.jpg)

​                                                    JDK示意图

JDK(Java SE Development Kit):Java标准开发工具包，它提供了编译、运行Java程序所需的各种工具和资源，包括Java编译器、Java运行时环境，以及常用的Java类库等；安装目录如下：

![img](https://img2018.cnblogs.com/blog/1362965/201901/1362965-20190114160755933-897193066.png)

JRE( Java Runtime Environment) ：Java运行环境，用于解释执行Java的字节码文件。普通用户而只需要安装 JRE（Java Runtime Environment）来运行 Java 程序。而程序开发者必须安装JDK来编译、调试程序；安装目录如下：

![img](https://img2018.cnblogs.com/blog/1362965/201901/1362965-20190114161959489-1682755970.png)

在这里可以认为bin里的就是jvm，lib中则是jvm工作所需要的类库，而jvm和 lib和起来就称为jre；

JVM(Java Virtual Mechinal)：Java虚拟机，是JRE的一部分。它是整个java实现跨平台的最核心的部分，负责解释执行字节码文件，是可运行java字节码文件的虚拟计算机。所有平台的上的JVM向编译器提供相同的接口，而编译器只需要面向虚拟机，生成虚拟机能识别的代码，然后由虚拟机来解释执行。

当使用Java编译器编译Java程序时，生成的是与平台无关的字节码，这些字节码只面向JVM。不同平台的JVM都是不同的，但它们都提供了相同的接口。JVM是Java程序跨平台的关键部分，只要为不同平台实现了相应的虚拟机，编译后的Java字节码就可以在该平台上运行。

### 区别与联系

1. JDK 用于开发，JRE 用于运行java程序 ；如果只是运行Java程序，可以只安装JRE，无序安装JDK。
2. JDk包含JRE，JDK 和 JRE 中都包含 JVM。
3. JVM 是 java 编程语言的核心并且具有平台独立性。



## 什么是编译型语言和解释型语言

计算机是不能理解高级语言的，更不能直接执行高级语言，它只能直接理解机器语言，所以使用任何高级语言编写的程序若想被计算机运行，都必须将其转换成计算机语言，也就是机器码。而这种转换的方式有两种：

1. 编译
2. 解释

由此高级语言也分为编译型语言和解释型语言。

![è¿éåå¾çæè¿°](https://img-blog.csdn.net/20180802084101214?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTIxODQ1Mzk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

### 编译型语言

使用专门的编译器，针对特定的平台，将高级语言源代码一次性的编译成可被该平台硬件执行的机器码，并包装成该平台所能识别的可执行性程序的格式。在编译型语言写的程序执行之前，需要一个专门的编译过程，把源代码编译成机器语言的文件，如exe格式的文件，以后要再运行时，直接使用编译结果即可，如直接运行exe文件。因为只需编译一次，以后运行时不需要编译，所以编译型语言执行效率高。

总结

1. 一次性的编译成平台相关的机器语言文件，运行时脱离开发环境，运行效率高；

2. 与特定平台相关，一般无法移植到其他平台；

3. 现有的C、C++、Objective等都属于编译型语言。

   ![img](https://img-blog.csdn.net/20180802084112768?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTIxODQ1Mzk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

### 解释型语言

使用专门的解释器对源程序逐行解释成特定平台的机器码并立即执行。解释型语言不需要事先编译，其直接将源代码解释成机器码并立即执行，所以只要某一平台提供了相应的解释器即可运行该程序。

总结

1. 解释型语言每次运行都需要将源代码解释称机器码并执行，效率较低；
2. 只要平台提供相应的解释器，就可以运行源代码，所以可以方便源程序移植；
3. Python等属于解释型语言。

![è¿éåå¾çæè¿°](https://img-blog.csdn.net/20180802084124396?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTIxODQ1Mzk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)