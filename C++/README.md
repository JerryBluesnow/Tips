# C++ Tips

## Makefile
- [Makefile C与C++混编的简单写法](https://blog.csdn.net/qq_33195791/article/details/100584342)
- [C/C++ 编写一个通用的Makefile 来编译.c .cpp 或混编](https://www.cnblogs.com/sylar-liang/p/4792514.html)
- [大型工程多级目录代码Makefile编写](https://www.cnblogs.com/jchen2020fighting/p/12175788.html)

## C HOOK
- This is for C HOOK for Game
- [全系统注入-游戏安全实验室](http://gslab.qq.com/article-204-1.html)

- [游戏修改器制作教程七：注入DLL的各种姿势](http://blog.csdn.net/xfgryujk/article/details/50478295)

- [游戏注入教程（一）--远程线程注入](http://blog.csdn.net/wyansai/article/details/52077963)

- [农民工の博客-5篇文章](http://blog.csdn.net/wyansai/article/category/6328876)

- [郁金香外挂技术](http://www.yjxsoft.com/forum.php?mod=forumdisplay&fid=4)

- [外挂制作技术](http://blog.sina.com.cn/s/articlelist_1457737921_0_1.html)

- [[视频]编写代码读取游戏数据-注入DLL](http://www.iqiyi.com/w_19rteanr1h.html)

- [[讨论]绕开游戏对全局钩子的检测](http://bbs.csdn.net/topics/370046194)

- [游戏外挂编程之神器CE的使用 ](http://www.cnblogs.com/egojit/archive/2013/06/14/3135147.html)

- [游戏外挂编程三之游戏进程钩子](https://www.cnblogs.com/egojit/archive/2013/06/16/3138266.html)

- [HOOK钩子的概念](https://jingyan.baidu.com/article/e75aca855afa03142fdac643.html)

- [HOOK钩子教程](http://blog.sina.com.cn/s/blog_651cccf70100tkv6.html)

- [多线程防关,防杀,防删除自身保护程序编写思路](https://www.2cto.com/kf/201002/44758.html)

- [如何让你的程序避开全局键盘钩子的监视](http://blog.okbase.net/BlueSky/archive/3839.html)

- [[CSDN]API HOOK 全局钩子， 防止进程被杀](http://download.csdn.net/download/lygf666/4164019)
- [XueTr这个牛B工具的进程钩子检测如何实现？？](https://bbs.pediy.com/thread-163373.htm)
- [[知乎]Win10下用SetWindowsHookEx设置钩子后部分进程假死？](https://www.zhihu.com/question/64221483)
- [SetWindowsHookEx为某个进程安装钩子](http://blog.csdn.net/hczhiyue/article/details/18449455)
- [HOOK API（四）—— 进程防终止](https://www.cnblogs.com/fanling999/p/4601118.html)
- [PChunter里边有个扫描指定进程中所有钩子的功能，原理是什么？](https://bbs.pediy.com/thread-210688.htm)

## 驱动级注入
    教程:1、打开驱动级别的dll注入器.exe 2、加载驱动 填写要操作的进程名  填写要注入  dll的路径   3 点设置完毕  注意不要关掉
    可以打开要注入的进程测试一下，可注入大部分有保护的游戏
## 游戏应用层钩子
    1. 直接加载驱动的办法过掉钩子，不需要什么XT 
    2. 代码直接注入到游戏进程
    3. 好像很多游戏把钩子什么的都屏蔽了吧，话说我也不怎么懂啊
    @ 春色园
    是的，但是普通游戏是没有屏蔽掉的。即使屏蔽了我们还是有其它办法把我们的代码写入游戏   内存。只要技术够牛连Windows系统内核占据的内存都可以操作，何况运行在windows下的游戏进程内存？？
    4. LPWSTR s1=(LPWSTR)(LPCTSTR)txt_prc_name;
    好丑的代码，建议楼主以后不要用这样的代码了。丑陋不说，还有重大隐患。正确做法是使用   CString::GetBuffer.外挂除了实现功能外最重要是稳定，一些细节你不注意你的外挂就没  市场了。
    说到代码的健壮性，还有你的SetHook 形参没必要用LPWSTR啊，直接LPCWSTR就OK，更好的 是LPCTSTR。
    建议楼主打好扎实的基础知识，走稳了再跑。
    话可能有些不好听，请见谅

- [利用vs2019编译器远程调试linux程序（走心版）](https://blog.csdn.net/foxriver_gjg1989/article/details/102854440)
## C++ 11

- [C++11 FAQ中文版](https://wizardforcel.gitbooks.io/cpp-11-faq/content/part1.html)
- [cpp11 github](https://github.com/DragonFive/cpp11)
- [搞懂C++11中的匿名函数](https://mp.weixin.qq.com/s/w_G8qy4UMucUqZ9Is22yqw)

## C++ 20

- [C++20新特性个人总结](https://blog.csdn.net/qq811299838/article/details/105153096/)

- [代码自动生成-宏带来的奇技淫巧](http://www.cppblog.com/kevinlynx/archive/2008/03/19/44828.html)

- [C++博客](http://www.cppblog.com/)

- [使用GTEST编写C++测试用例进阶教程（GTEST advanced中文译文）](https://www.jianshu.com/p/215edbfc2e0a)

- [Advanced googletest Topics](google.github.io/googletest/advanced.html)

- [有哪些值得推荐的c/c++开源框架与库]h(ttps://zhuanlan.zhihu.com/p/71707672)

- [利用vs2019编译器远程调试linux程序（走心版）](https://blog.csdn.net/foxriver_gjg1989/article/details/102854440)

## C++ 11

- [C++11 FAQ中文版](https://wizardforcel.gitbooks.io/cpp-11-faq/content/part1.html)
- [cpp11 github](https://github.com/DragonFive/cpp11)
- [搞懂C++11中的匿名函数](https://mp.weixin.qq.com/s/w_G8qy4UMucUqZ9Is22yqw)

## 内存

- [C++堆，栈，RAII](https://zhuanlan.zhihu.com/p/354611651)

- [C++ 类在内存中的存储方式(一)](https://zhuanlan.zhihu.com/p/103384358)

- [C++成员函数在内存中的存储方式](https://www.cnblogs.com/rednodel/p/9300729.html)

- [C++类的实例化对象的大小之sizeof()](https://blog.csdn.net/houqd2012/article/details/40264943)

## 代碼分析工具

- [RunSnakeRun](https://segmentfault.com/a/1190000000356018)
  
  RunSnakeRun有一个内存调试模式（需要 Meliae ）。遗憾的是我还没有试过。但是如果你想看到内存的使用情况，它看起来确实很有用。就像[这样](http://www.vrplumber.com/programming/runsnakerun/meliae-sample.png）。

```
KCachegrind
你可以用apt-get install kcachegrind 来安装或者从源码安装。

鲜为人知的秘密：有windows版本的KCachegrind二进制包。 只需要安装windows版本的KDE，然后在选项“开发者工具”选中它，很可能要取消选择其他项）。

我真的很喜欢这工具！它可以向你展示调用树的图表，可排序的调用表，调用/被调用的地图，源代码，而且你还能选择过滤掉所有的东西。而且它是语言无关的 – 如果你有C/C++背景的话，你很可能听说过这个工具。

我喜欢这个工具的程度超过RunSnakeRun，因为它比后者功能要强大的多：

在调用数的图表上，你可以排序，改变布局/用很多方法来渲染或者导出成点图/png，RunSnakeRun甚至不能显示调用树的图表
你可以看见源码
你可以得到被调用地图
更容易安装（不需要依赖wxPython）
在大项目里面哪些需要关注并不是那么显而易见，或者有很多的相关函数时，KCachegrind比RunSnakeRun更值得一用。
可生成KCachegrind 分析文件的工具
我想这是唯一的缺点，你需要用这种特殊的格式导出。但是它也有很好的支持：

pyprof2calltree
django扩展的 runprofileserver
装饰器
kcachegrind 转换器
repoze.profile – 只需要设置cachegrind_filename
```

## 你见过哪些令你瞠目结舌的C/C++代码技巧？

- [The International Obfuscated C Code Contest](www.ioccc.org)
- [你见过哪些令你瞠目结舌的C/C++代码技巧？](https://www.zhihu.com/question/37692782/answer/74409370)
