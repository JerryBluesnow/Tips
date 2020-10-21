# Java Tips

## Java调用 dll/so 动态链接库
- [Java调用C/C++实现的DLL动态库——JNI](https://www.cnblogs.com/xiehy/p/3365682.html)
- [Java调用C/C++编写的第三方dll动态链接库（非native API）--- JNI](https://www.cnblogs.com/AnnieKim/archive/2012/01/01/2309567.html)
- [C++和JNI的数据转换](https://www.cnblogs.com/daniel-shen/archive/2006/10/16/530587.html)
- [JNI中基本类型数组的传递方法(无需拷贝数据!!!)](https://blog.csdn.net/iteye_11349/article/details/82436966)

## 关键字
### @FunctionalInterface
- [JDK8新特性：函数式接口@FunctionalInterface的使用说明](https://blog.csdn.net/aitangyong/article/details/54137067)

### Interface关键字
- [Java interface关键字](https://blog.csdn.net/u013453970/article/details/47618283?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-2.channel_param&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-2.channel_param) - 从例子看interface

- [Java 接口(interface)的用途和好处](https://blog.csdn.net/nvd11/article/details/41129935?utm_medium=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param) - 解释的简直不能太好

### CommandLineRunner
- [Spring Boot 启动加载数据 CommandLineRunner](https://blog.csdn.net/catoop/article/details/50501710)

- [CommandLineRunner或者ApplicationRunner接口](https://www.jianshu.com/p/5d4ffe267596) - 更详细

### SpringApplication.run
- [SpringApplication.run执行流程详解](http://c.biancheng.net/view/4632.html)
- [springboot启动类--SpringApplication.run()详解](hhttps://blog.csdn.net/weixin_41884010/article/details/88844946)

## 安装java jdk
- [refer](https://www.cnblogs.com/carryLess/p/7508378.html)
   
    网站查询
    https://download.oracle.com/otn/java/jdk/8u261-b12/a4634525489241b9a9e1aa73d9e118e6/    jdk-8u261-linux-x64.tar.gz?xd_co_f=27b6b93d465637b95d61599612466900

    wget https://download.oracle.com/otn/java/jdk/8u261-b12/a4634525489241b9a9e1aa73d9e118e6/   jdk-8u261-linux-x64.tar.gz?AuthParam=1599643461_25207893d4c504094edb8a58b6341fe0   --no-check-certificate

    root@default:~# pwd

    export http_proxy=http://192.168.99.99:8080
    export https_proxy=https://192.168.99.99:8080
    export JAVA_HOME=/jerry/work/jdk1.8.0_261
    export JRE_HOME=${JAVA_HOME}/jre
    export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
    export PATH=${JAVA_HOME}/bin:$PATH

## download maven

    http://maven.apache.org/download.cgi
    https://www.linuxidc.com/Linux/2013-05/84489.htm
    apache-maven-3.6.3-bin.tar.gz
    wget https://downloads.apache.org/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.tar.gz --no-check-certificate

    export MAVEN_HOME=/jerry/work/apache-maven-3.6.3
    export PATH=${MAVEN_HOME}/bin:${PATH}

## Visual搭建Java开发环境
- [Visual Studio Code 搭建 Java 开发环境](https://blog.csdn.net/hezh1994/article/details/79895480)

## Ubuntu 16.04安装Java 8
 - [Ubuntu 16.04安装Java 8](https://www.cnblogs.com/-qing-/p/10894868.html)

## 使用parallelStream线程安全地收集数据
- [java8 使用parallelStream线程安全地收集数据](zhk.me/1281.html)
- [面试官问线程安全的List，看完再也不怕了！](https://blog.csdn.net/youanyyou/article/details/101442425?utm_medium=distribute.pc_aggpage_search_result.none-task-blog-2~all~sobaiduend~default-1-101442425.nonecase&utm_term=java%20list是线程安全的吗&spm=1000.2123.3001.4430)
- [Map、list 线程安全问题](https://blog.csdn.net/y_index/article/details/84988018?utm_medium=distribute.pc_aggpage_search_result.none-task-blog-2~all~sobaiduend~default-4-84988018.nonecase&utm_term=java%20list是线程安全的吗&spm=1000.2123.3001.4430)
- [Java中哪些集合是线程安全的，哪些是线程不安全的](https://www.cnblogs.com/aaaazzzz/p/12793428.html)
- java.util.Collections.SynchronizedList
- CopyOnWriteArrayList
- CopyOnWriteArraySet
- Vector

- [ConcurrentHashmap 是线程安全的类，那么并发的clear方法是否有问题呢？](https://www.zhihu.com/question/28482635)
```
    看完，也许对你理解有些帮助。在项目中也会经常用到ConcurrentHashMap做一些缓存。归根到底：一致性与效率之间的一种权衡选择关系！！请看下文分解相比同步锁synchronizedMap或者HashTable，ConcurrentHashMap引入了分段锁（segmentation），无需锁住全局，不论它变得多么大，仅仅需要锁定map的某个部分。1、优点：体现在效率方面，ConcurrentHashMap在线程安全的基础上提供了更好的写并发能力，仅仅需要锁定map的某个部分，而其它的线程不需要等到迭代完成才能访问map。2、缺点：体现在一致性方面，既然这么好，为什么不能替代其他的map，比如HashTable，因为ConcurrentHashMap是弱一致！3、解答题主的疑惑：什么是弱一致性，我举个简单的例子，例如题主说的clear方法！因为没有全局的锁，在清除完一个segments之后，正在清理下一个segments的时候，已经清理segments可能又被加入了数据，因此clear返回的时候，ConcurrentHashMap中是可能存在数据的。因此，clear方法是弱一致的。！！总结！！ConcurrentHashMap的弱一致性主要是为了提升效率，但是成为弱一致。Hashtable为了线程安全的强一致性，就需要全局锁，降低效率。一致性与效率之间的一种权衡选择关系
```

- [ConcurrentHashMap](https://www.cnblogs.com/yydcdut/p/3959815.html)  --- 这个非常好


## web前端后端数据交互
- jsp可以，不过你也可以用vue+html实现前后端分离.可以推荐element ui 给你,很方便，很简洁.
```

晨曦遇晓
等级 
勋章Blank	
全栈的路过，首先申明下你说的html转jsp这个问题 jsp其实就是后台的一个servlet文件而已 只不过在程序运行的过程中它会自动解析并渲染成html的文件，在接着说前后端的交互问题，一般都使用的是ajax，也有较少部分使用的是form表单，后台获取到了数据之后进行逻辑判断然后对数据库进行操作 如果现在和你聊mvc可能还太早了 简单点就是现在大部分都是一个人从表设计，创建，在到前端页面的创建，后台的逻辑以及对数据库的操作都是一个人来完成的 所以lz加油 你能行的 
2018-03-23 16:25:47#5得分 0

大昭逸
等级 
勋章Blank	
说的都这么不好理解,其实纯后台的人拿到html页面第一件事就是转成JSP,交互有2种技术,同步和异步,同步就是http协议中的内容,异步是Ajax,Ajax属于前端技术,不过后端人都会学习. 当然懂点前端技术(html+CSS+JS+jquery)是好的,办事不求人,
```
