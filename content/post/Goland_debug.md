+++

title = 'goland debug'
date = 2024-04-13T14:15:10+08:00
author = "Yotom"
description = "一些关于goland的debug方法"
tags = [
    "Golang",
    "goland",
    "debug",

]
categories = [
    "学习记录",
    "Golang",
]

+++

# goland debug

## 本地调试

写一个简单的应用程序

```go
package main
import "flag"

var j = flag.Int("j", 0, "")
func main(){
    flag.Parse()
    var i = 0;
    for {
        fmt.Println("demo print ", i, *j))
        i++
        time.Sleep(time.Second)
    }
}
```

终端执行命令

```go
go run .\main.go --j 10
```

结果如下

```go
(base) PS D:\GoWorks\src\demo> go run .\main.go --j 10
demo print 0 10
demo print 1 10
demo print 2 10
demo print 3 10
demo print 4 10
demo print 5 10
demo print 6 10
demo print 7 10
.....................................................
//不断循环打印
```

在goland界面设置断点进行调试即可

---

## 附加到进程

golang附加到进程首先需要安装一个插件

```go
go install github.com/google/gops@latest
```

仅安装插件是不够的，需要进行编译：

``` go
go build -gcflags="all=-N -l" -o demo.exe
```

生成可执行文件后运行即可通过goland附加到进程来进行调试：

```go
.\demo.exe --j 100
```

---

## 远程调试

在本地编译好demo

```go
go build -gcflags="all=-N -l" -o demo
```

然后通过网络工具发送到linux上。

通过语句修改demo的权限：

```go
sudo chmod +x demo
```

安装插件：

```go
go install github.com/go-delve/delve/cmd/dlv@latest
```

输入指令：

```go
dlv --listen=:2345 --headless=true --api-version=2 --accept-multiclient exec ./demo
```

之后在goland配置中go remote连接到linux主机。

通过下面指令可以在linux看到进程信息

```go
ps aux | grep demo
```

再通过下面指令同样可以进行远程调试

```go
dlv --listen=:2345 --headless=true --api-version=2 attach 进程编号
```

---
