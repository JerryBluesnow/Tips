## 比较跨语言通讯框架：thrift和Protobuf
```
OkidoGreen 2016-09-21 11:01:37   15218   收藏 2
分类专栏： RPC-ProtoBuf RPC-Thrift RPC-Grpc&Graphql&对比
前两天想在微博上发表一个观点：在现在的技术体系中，能用于描述通讯协议的方式很多，xml,json,protobuf，thrift，如果在有如此众多选择的基础上，在设计系统时，还自造协议，自己设计协议类型和解析方式，那么我只能说，您真的落后了，不是技术上，而是思想上。对于xml,和json我们不做过多描述了，参考相关文档就可以了。特别是json，如今在 web系统，页游系统的前后台通讯中，应用非常广泛。本文将重点介绍两种目前在大型系统中，应用比较普遍的两种通讯框架，thrift和Protobuf，为什么叫通讯框架，而不叫通讯协议，因为这两种技术，如果仅仅当作协议解析用，对于其强大的功能，就大打了折扣。 
对于两种利器而言，首推的应该是thrift，因为其不仅有对于协议封装和解析的处理，而且有完备的通讯框架的实现，完全封装了底层通讯，对于使用者，只要在框架的客户端和服务器接口回调中，处理逻辑就可以了。对于其确切的描述，我们还是引用官方的说法吧，这样更准确些，以免由于我自己的想法，影响了大家的理解。 
Thrift是一个跨语言的服务部署框架，最初由Facebook于2007年开发，2008年进入Apache开源项目。Thrift通过一个中间语言(IDL, 接口定义语言)来定义RPC的接口和数据类型，然后通过一个编译器生成不同语言的代码（目前支持C++,Java, Python, PHP, Ruby, Erlang, Perl, Haskell, C#, Cocoa, Smalltalk和OCaml）,并由生成的代码负责RPC协议层和传输层的实现。 
Thrift实际上是实现了C/S模式，通过代码生成工具将接口定义文件生成服务器端和客户端代码（可以为不同语言），从而实现服务端和客户端跨语言的支持。用户在Thirft描述文件中声明自己的服务，这些服务经过编译后会生成相应语言的代码文件，然后用户实现服务（客户端调用服务，服务器端提服务）便可以了。其中protocol（协议层, 定义数据传输格式，可以为二进制或者XML等）和transport（传输层，定义数据传输方式，可以为TCP/IP传输，内存共享或者文件共享等）被用作运行时库。 
Thrift支持二进制，压缩格式，以及json格式数据的序列化和反序列化。这让用户可以更加灵活的选择协议的具体形式。更完美的是，协议是可自由扩展的，新版本的协议，完全兼容老的版本！ 
那么thrift有没有缺点呢，有，而且很严重！但不影响使用，因为此框架的缺点不在于其程序本身，而在于其文档的缺失！包括中文的，及英语的相关文档。要彻底的理解和熟练的把握thrift只有一个办法，读thrift的代码，通读！我很乐于做这样的事，因为读代码，而且是高质量的代码，是一件非常有乐趣的事情，这对于一个程序员来讲，没有什么。当然，本着普及的需要，文档的完善化，确实需要大大的加强。当然，我也相信，thrift的文档，会日渐完善，因为现在国内的热心程序开发者，开源软件的爱好者越来越多，使用的同时，着手thrift文档的补充工作，是很多人愿意做的，而且也是很有成就感的事情。 
相比于，thrift文档的匮乏，protobuf的文档可称非常完备。这一点，我认为google开源出来的东西，确实要比facebook，在使用指南和文档完备上高出一个档次。 
protobuf是google提供的一个开源序列化框架，类似于XML，JSON这样的数据表示语言，其最大的特点是基于二进制，因此比传统的 XML表示高效短小得多。虽然是二进制数据格式，但并没有因此变得复杂，可以很方便的对其基于二进制的协议进行扩展，并且很方便的能让新版本的协议兼容老的版本。如果说xml太臃肿，json易解析，比xml更高效，易扩展，那么protobuf可以说，相对于json更高效，更易扩展，而且协议的保密性更强。并且protobuf是跨语言的，可以支持c(c++),java，python等主流语言，非常方便大系统的设计。protobuf号称也有service，可以基于其service的接口和回调，来完成客户端和服务器的逻辑。但是，目前版本service还仅仅停留在接口层，其底层的通讯，还需要自己实现，这点确实远不如thrift完备。

protobuf + netty ？

protobuf在google中是一个比较核心的基础库，承担着google海量服务器间的通讯协议设计。在多功能，跨语言的海量系统中，如此高效，简洁，可自由扩展的通讯框架，可谓经典。 
说了归齐，有如此好利器，我们再设计大系统时，真的应该尽量避免造轮子了，给一个建议，在您设计大系统之前，不妨先关注一下google code上，以及互联网上诸多的开源项目，很多时候，又快又好的设计，很多同行已经做好了，拿来用就可以了。
```

- [thrift应用举例(c/c++作为服务端、java作为客户端)](https://blog.csdn.net/zzhongcy/article/details/85854019?utm_medium=distribute.pc_relevant.none-task-blog-title-10&spm=1001.2101.3001.4242)

- [Thrift or gRPC ？Alluxio RPC框架的深度实践总结](https://cloud.tencent.com/developer/article/1469122)

- [ProtoBuf入门指南笔记摘录](https://www.jianshu.com/p/a098ef2c9f67?utm_campaign=maleskine&utm_content=note&utm_medium=seo_notes&utm_source=recommendation)

- [分布式通讯中三大框架protobuf,thrift,fast比较](https://baijiahao.baidu.com/s?id=1653883193167850965&wfr=spider&for=pc)

- [三种通用应用层协议protobuf、thrift、avro对比,完爆xml,json,http](https://www.cnblogs.com/lnlvinso/p/9781055.html)

- [Thrift C++ 服务器和客户端开发实例--学习笔记](https://blog.csdn.net/feng973/article/details/70160571)

- [Thrift C++ 服务器和客户端开发实例--学习笔记二](https://blog.csdn.net/feng973/article/details/80434855?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-5.channel_param&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-5.channel_param)

- [跨语言通信方案的比较—Thrift、Protobuf和Avro](https://blog.csdn.net/hebeind100/article/details/84774764)

- [Thrift框架，java作为客户端，c++作为服务端（visual studio）](https://blog.csdn.net/blockbtc/article/details/86596300)

- [Thrift之visual studio c++实例](https://blog.csdn.net/byxdaz/article/details/74479671)

- [Windows 10 Visual Studio 2017 安装配置 Apache Thrift （C++）](https://www.cnblogs.com/49er/p/7193829.html)

- [Thrift初探：简单实现C#通讯服务程序](https://www.cnblogs.com/liping13599168/archive/2011/09/15/2176836.html)

- [Apache Thrift](https://thrift.apache.org/)

- [由浅入深了解Thrift（一）——Thrift介绍与用法](https://xiaoyaozi.blog.csdn.net/article/details/42778335)

- [由浅入深了解Thrift（三）——Thrift server端的几种工作模式分析](https://blog.csdn.net/houjixin/article/details/42779915)

- [Thrift初探：简单实现C#通讯服务程序](https://www.cnblogs.com/liping13599168/archive/2011/09/15/2176836.html)

- [RPC学习--C#使用Thrift简介，C#客户端和Java服务端相互交互](https://www.cnblogs.com/amosli/p/3948342.html)

- [C#实现Thrift服务端与客户端](https://blog.csdn.net/lwwl12/article/details/77116253)

- [Thrift与其说他传输方式的比较](https://blog.csdn.net/yczz/article/details/52450398)

- [Apache Thrift系列详解(三) - 序列化机制](https://zhuanlan.zhihu.com/p/45206710)