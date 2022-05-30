## decltype
```cpp
decltype解决了auto只能对变量进行类型推导的缺陷。decltype的用法：

decltype(表达式)
如果需要计算某个表达式的类型，可以利用decltype

//decltype计算表达式类型
auto a = 1; auto b = 2;
decltype(a + b) c;    //c的类型为(a + b)和的类型
拖尾返回类型
C++11引入了拖尾返回类型

template<typename T, typename U>
//首先可能想到使用下面代码，但在编译器读到decltype(x+y)时，x和y尚未被定义。 
decltype(x+y) add(T x, U y);    //无法通过编译
//C++11引入了拖尾返回类型，解决了上述问题。
auto add(T x, U y) -> decltype(x + y) {
    return x + y;
}
```