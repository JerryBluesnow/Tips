
# Golang异常控制处理/程序恢复
- [Golang异常控制处理程序错误流程](http://www.cppcns.com/jiaoben/golang/574511.html)
```
利用recover处理panic指令，recover需要定义在defer匿名函数内
defer需要在panic之前声明，否则当panic时，recover无法捕获到panic
panic无recover情况下，程序会直接崩溃

如果 panic 和 recover 发生在同一个协程，那么 recover 是可以捕获的，如果 panic 和 recover 发生在不同的协程，那么 recover 是不可以捕获的
也就是哪个协程有panic，哪个协程里必须要有recover，否则会把整个程序弄崩溃

在使用 golang 进行开发时，遇到 panic 是非常常见的情况。但是，panic 对于性能的影响是相对较小的，尤其是在实际使用中。

首先，Golang 在运行时会维护一个 panic 堆，用于存储栈中的 panic 对象。当程序遇到 panic 时，会将该 panic 对象添加到 panic 堆中。panic 堆的大小是有限的，如果堆中的对象过多，可能会导致 panic 堆溢出，从而影响程序的性能
```
- [Go语言的宕机恢复，防止程序奔溃](https://www.modb.pro/db/97643)