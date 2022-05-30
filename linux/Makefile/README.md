# tips for make cmake Makefile CMakefileLists.txt

## make和cmake命令的关系和区别
```
make工具通过调用makefile文件中的命令便可以对大型程序进行编译，而makefile文件中就包含了调用gcc去编译多个源文件的命令。

但是，很快又出现了一个问题，如果我们的程序是跨平台的，如果换个平台makefile又要重新修改，这会很麻烦，所以就出现了cmake这个工具，通过cmake我们就可以快速创建出不同平台的makefile文件。

而cmake又是根据CMakeLists.txt来生成makefile文件，这里你可能觉得有点儿绕，我来总结一下，就是为了编译一个大型程序，你首先编写CMakeLists.txt。然后，通过cmake命令就可以生成makefile文件。然后通过make命令就可以使用这个makefile文件从而生成可执行文件。
```

- [CMake和Make之间的区别](https://blog.csdn.net/zpf_nevergiveup/article/details/86242806)

- [CMake 入门实战](https://www.hahack.com/codes/cmake/) - 非常好的文章，讲解详细，清晰易懂！

- [CMakeLists.txt 语法介绍与实例演练](https://blog.csdn.net/afei__/article/details/81201039)