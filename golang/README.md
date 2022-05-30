
# [Download](https://golang.google.cn/dl/)

# 

- [Go 语言到底适合干什么？](https://www.zhihu.com/question/296426314)

## Windows下使用syscall.SIGUSR1报错：SIGUSR1 not declared by package syscall
在 go 的安装目录修改 Go\src\syscall\types_windows.go，增加如下代码：
```go
var signals = [...]string{
    // 这里省略N行。。。。
 
    /** 兼容windows start */
    16: "SIGUSR1",
    17: "SIGUSR2",
    18: "SIGTSTP",
    /** 兼容windows end */
}
 
/** 兼容windows start */
func Kill(...interface{}) {
    return;
}
const (
    SIGUSR1 = Signal(0x10)
    SIGUSR2 = Signal(0x11)
    SIGTSTP = Signal(0x12)
)
/** 兼容windows end */
```