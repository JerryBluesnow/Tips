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