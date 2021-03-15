# JVM从0到实战
## 目录

- [jvm运行原理](#JVM运行原理)
    - [我们写的Java代码到底是如何运行起来的](#我们写的Java代码到底是如何运行起来的)
    - [JVM类加载机制](#JVM类加载机制)
## 大纲
###我们写的Java代码到底是如何运行起来的
    
![我们编写的java文件被执行的过程](https://github.com/zuolinlin/java-stack/blob/main/docs/java-core/images/jvm-process.png)
        
    JVM要运行这些字节码文件，首先得把这些.class文件加载进来
    此时会采用类加载器将这些字节码文件加载搭配JVM，供后续的代码运行使用
    最后一步，JVM会基于自己的**字节码执行引擎**，来执行加载到内存里我们写好的那些类了。
    
下一步讨论
###JVM类加载机制

一个类从加载到使用一般经历一下几个过程

![JVM的类加载器亲子层级](https://github.com/zuolinlin/java-stack/blob/main/docs/java-core/images/class_loader_process.png)


**加载**--**验证**--**准备**--**解析**--**初始化**--**使用**--**卸载**

加载阶段：
       代码在使用这个类的时候，.class 文件会加载进内存

验证阶段：
       根据java虚拟机规范，来验证你加载进来的.class 文件的内容是否符合规范

准备阶段：
      给类遍历分配内存空间，来一个默认的初始值

解析阶段：
      把符号引用替换为直接引用

初始化：
     new Object() 初始化一个对象，在准备阶段知识只是分配了内存空间，给了默认值，真正的赋值是在初始化阶段完成的
     此外这里有个非常重要的规则，就是初始化一个类的时候，发现它的父类还没有初始化，那么必须先初始化它的父类。
    

### 类加载器和双亲委派机制

     从加载到初始化实际上是有类加载器来完成的

 **java中有哪些类加载器呢？简单来说有下面几种**

    - 启动类加载器
        Bootstrap classLoader ，它主要是负债加载我们机器安装的java目录下的核心类的
        相信大家都知道，如果你要在机器上运用一个自己写好的Java系统，无论是Windows笔记本，还是linux服务器，是不是都得装一下JDK，
        那么你在安装目录下就有一个**lib**文件
        所以一旦你的JVM启动，那么首先就会依托启动类加载器，去加载你的JAVA 安装目录下的**lib**目录

    - 扩展类加载器
        Extension ClassLoader，这个类加载器也是类似的，就是在你JAVA安装目录下，有个**lib\ext**目录，
        这里有一些类，就是需要使用这个类加载器来加载的，支撑你的系统的运行。
        那么你的JVM一旦启动，是不是也得从java安装目录下，加载这个**lib\ext**目录中的类
 
    - 应用类加载器
        Application ClassLoader，这类加载器就去加载**classpath**环境变量所指定的路径中的类加载器
        其实大致就理解为去加载你写好的Java代码吧，这类加载器主要负责加载你写好的那些类到内存里。

    - 自定义类加载器
        除了上面那几种情况外，还可以自定义类加载器，去根据你的需求加载你的类。

jvm的类加载器是有亲子层级机构的，就是启动类加载器子啊最上层，拓展类加载器在第二层，第三层是应用程序类加载器，最下面一层是自定义加载器

![JVM的类加载器亲子层级](https://github.com/zuolinlin/java-stack/blob/main/docs/java-core/images/classloder-qinzichengji.png)

基于这个亲子层级关系，就有一个**双亲委派机制**

双亲委派机制：先由父类加载，不行的话在由儿子来加载。
![JVM的类加载器亲子层级](https://github.com/zuolinlin/java-stack/blob/main/docs/java-core/images/class_loader_shuangqinweipai.png)