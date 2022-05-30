# C++ excetion 异常处理

## C++异常处理

```
任何事情都是两面性的，异常有好处就有坏处。如果你是C++程序员，并且希望在你的代码中使用异常，那么下面的问题是你要注意的。

1. 性能问题。这个一般不会成为瓶颈，但是如果你编写的是高性能或者实时性要求比较强的软件，就需要考虑了。
（如果你像我一样，曾经是java程序员，那么下面的事情可能会让你一时迷糊，但是没办法，谁叫你现在学的是C++呢。）

2. 指针和动态分配导致的内存回收问题：在C++中，不会自动回收动态分配的内存，如果遇到异常就需要考虑是否正确的回收了内存。在java中，就基本不需要考虑这个，有垃圾回收机制真好！

3. 函数的异常抛出列表：java中是如果一个函数没有在异常抛出列表中显式指定要抛出的异常，就不允许抛出；可是在C++中是如果你没有在函数的异常抛出列表指定要抛出的异常，意味着你可以抛出任何异常。

4. C++中编译时不会检查函数的异常抛出列表。这意味着你在编写C++程序时，如果在函数中抛出了没有在异常抛出列表中声明的异常，编译时是不会报错的。而在java中，eclipse的提示功能真的好强大啊！

5. 在java中，抛出的异常都要是一个异常类；但是在C++中，你可以抛出任何类型，你甚至可以抛出一个整型。（当然，在C++中如果你catch中接收时使用的是对象，而不是引用的话，那么你抛出的对象必须要是能够复制的。这是语言的要求，不是异常处理的要求）。

6. 在C++中是没有finally关键字的。而java和python中都是有finally关键字的。
```

- Every exception within the C++ standard library(including this) has, at least, a copy constructor that preserves the string representation returned by member what when the dynamic types match.

- compiler always creates a temporary copy when throwing an exception, even if the exception specifier and catch blocks specify a reference


### 关于用不用异常

- [对使用 C++ 异常处理应具有怎样的态度？](https://www.zhihu.com/question/22889420)

- 如果不用异常，函数签名都可能复杂，首先要靠返回值来区分成功与否，然后靠out参数来接受真正的返回值。也就是你的 multiplies 函数。 太复杂了

- 在能够处理异常的地方处理异常。没有定规，但肯定不会层层处理异常。

- 最常见的情况，出现异常意味着某个特定的任务彻底失败。一般在这个特定的任务处理代码里处理这个异常，向界面/日志输出信息，和/或回退其他流程。

- 异常不是一个错误码，而是一个对象。这个对象里你可以存储很多信息，包括错误出现的原因、位置等等。通常异常继承自 std 里的 logic_error 或 runtime_error，可以在标准的字符串信息外加入你自己的异常信息。外面要捕获你的特定异常类型就可以获得这些额外信息，偷懒也可以只捕获标准异常，至少可以获得一个字符串信息，肯定比一个错误码更能够判断错误的原因吧？

- 没有异常的 RAII 是不完整的，没有 RAII 的异常是不安全的

- 二段式构造 ??->构造函数里什么都不干，用一个 init() 成员函数做初始化，返回成功还是失败。-> 最烦二段式构造

```
异常安全应该也是C++里的难点了。这里的难点有：三种类型的异常保证本身就比较难理解。除了那个no-throw保证以外。所有的资源都应该使用RAII进行管理。而RAII对于很多C++程序员来说，其实好像还是很陌生的(是么？还是不是？这纯粹是我对身边人的感觉)。即便是用了RAII，代码写的不好也是枉然。例子后面会说。析构函数坚决不能抛异常。这就是那个no-throw。很多人知道，但是不知道为什么。

作者：知乎用户 / 链接：https://www.zhihu.com/question/22889420/answer/23323294
```

```
链接：https://www.zhihu.com/question/22889420/answer/48802023

C++ 引入 exception 的同时，也引入了一个十分模糊的概念 —— exception-safe 。很多人认为有 RAII 就足够 exception-safe，其实不然。比如 std::vector 对 move 的处理就不止 RAII。Exception-safe 是一个没有明确定义的概念。换句话说，exception-safe 是对数据 consistency 的一个拙略的描述（Wiki 上所谓的 weak-exception-safe 其实没什么用。你连 consistency 都不保证了，你的程序都处于 unpredictable behavior 了，还 leak 不 leak 的？）。GC 在纯内存资源的限制下可以做到 eventual-consistent。RAII 在单个资源的情况下可以模拟 transaction。但是没有一个简单的方法能真正做到在使用 exception 的同时保持 consistency。关系数据库的真正的 two-phase commit transaction 可以保持 exception 下数据库的 consistency。所以 exception 只在三中情况下好用：有 GC 并且只处理内存数据。一次只处理一个数据。只处理有严格 two-phase commit transaction 的数据，例如 RDBMS 管理所有数据。鉴于 C++ 称自己为「通用语言」，而且没有 GC，并不是像当年 Sun 说的 Java 适合 DB-driven 的 server-side 开发。我觉得引入 exception 是很差的决定。Error code 是用 return-safe 来代替 exception-safe。虽然保证 return-safe 也不容易，但比 exception-safe 容易太多（如果不是「feasible vs. infeasible」的区别）。因为 return-safe 的逻辑和整个代码的 flow 是统一的。而 exception 要求程序员在无法清晰表述 error 发生的位置的时候用统一的 handling 从错误中恢复（否则你就每一行代码都要 encompass 一个 try-catch block）。在没有理论支持的基础上，ad-hoc 比 superficially elegant 更实用更有效。
```
- [CppBaseTest](https://github.com/fengbingchun/Messy_Test/tree/master/demo/CppBaseTest)
- [cplusplus_tutorialspoint](https://www.tutorialspoint.com/cplusplus)
- [cpp_exceptions_handling](https://www.tutorialspoint.com/cplusplus/cpp_exceptions_handling.htm)
- [Constructor Delegation in C++](https://www.geeksforgeeks.org/constructor-delegation-c/): Sometimes it is useful for a constructor to be able to call another constructor of the same class. This feature, called Constructor Delegation, was introduced in C++ 11.

- 异常捕获中也可以向上转型, 即子类型可以自动被转为父类型
- catch(...)，这个代码块代表捕获所有类型异常，这个不能写在最前面，不然的话会将后续的catch块全屏蔽，一般写在最后面做备用

- throw和catch的是什么类型的对象？这种对象的类的继承体系是什么样的？如果遇到异常，我在程序中应该throw什么呢？
- catch接收throw的对象，是值拷贝、引用拷贝还是个指针呢？我应该使用哪种方式去catch呢？
- 一个try block后面经常跟着多个catch语句（携带不同的参数），那么throw的对象由哪个catch语句匹配？它的依据的是什么？
- 什么是rethrow？什么是catch-all handler？
- 构造函数中发生异常该怎么办？构造函数初始化列表发生异常怎么办？
- 析构函数中发生异常该怎么办？
- noexcept关键字有什么意义？合成的拷贝构造函数上有吗？析构函数是noexcept的吗？

- [C++的异常处理](https://zhuanlan.zhihu.com/p/276337588) 
- - 有质量的文章，讲了各种要注意的事项

```
我们在程序中throw的东西，就是Exception object（throw的对象）。它必须具备完整的类型，并且当其类型是class的时候，这个class必须具备可访问的析构函数、拷贝构造函数或者移动构造函数。如果throw的表达式（对象）是个数组或者函数类型，那么这个表达式会被转换为对应的指针类型。

Exception object会存放在由编译器管理的某个空间下，以此确保任何一个被匹配到的catch语句都可以访问的到。当异常被处理后，Exception object会被释放。当Exception object是个指针的时候，必须确保该指针指向的不是一个局部的内存，不然在try代码块结束的时候，该内存已经被释放，那么在catch中访问到的指针，指向的就是一个无效的地址了。

值得说明的是，throw表达式中的对象的类型为静态编译期决定的类型，比如，当throw一个基类修饰的指针的dereference，该指针指向子类的对象，那么当处在throw表达式中的时候，该dereference只会解出来基类部分然后throw。

而在catch语句的接收部分，当catch语句的参数类型是non reference类型的时候，那么这个参数就会被值拷贝，所有针对这个参数所做的修改都是局部有效的，而不是针对throw的Exception object的；当catch参数类型是引用的时候，不说了，其效果就和正常的函数传参是一样的。
```
