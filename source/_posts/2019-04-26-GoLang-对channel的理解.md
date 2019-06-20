---
layout:     post
title:      GoLang 对channel的理解
subtitle:   Go 基础知识笔记
date:       2019-03-20
tags:
  - GoLang
---


1 解释上回学习的谜团

```
# src/main/blocking3.go
package main

import (
  "fmt"
)

func f1(in chan int) {
  number := <- in
  fmt.Println(number)
}

func main() {
  out := make(chan int)
  out <- 2
  go f1(out)
  fmt.Println("end")
}
```
> go run blocking3.go

```bash
fatal error: all goroutines are asleep - deadlock!
goroutine 1 [chan send]:
main.main()
```

只需要把 ```go f1(out) ``` 和 ```out <-2 ``` 对换一下就可以了

```
# src/main/blocking3.go
package main

import (
  "fmt"
)

func f1(in chan int) {
  number := <- in
  fmt.Println(number)
}

func main() {
  out := make(chan int)
  go f1(out)
  out <- 2  
  fmt.Println("end")
}
```
 编译运行

```
2
end
```

???? 究竟为什么

GO的channel具有两个特点

* 发送方。对于同一个通道，发送操作（协程或者函数中的），在**接收者准备好之前是阻塞的;**如果ch中的数据无人接收，就无法再给通道传入其他数据**;新的输入无法在通道非空的情况下传入。所以发送操作会等待 channel 再次变为可用状态：就是通道值被接收时（可以传入变量）。

* 接收方。对于同一个通道，接收操作是阻塞的（协程或函数中的），直到发送者可用：如果通道中没有数据，接收者就阻塞了。

因此可以针对上述现象解释

```
 1) 第一种情况
 会造成死锁
   out <- 2  # 接受者阻塞，因为没有发送者（此时f1还没启动）
   go f1(out) 在f1中，通道 in 没有传送，所以一直等待；

 2) 不会造成死锁
   go f1(out)
   out <- 2
  f1在等待传送， main相当于一个协程（因为利用了out），等待out传送才关闭；
也因为如此f1的打印才得以输出
```

换一个方式看
```
package main

import (
  "fmt"
  "time"
)

func f1(in chan int) {
  fmt.Println( <- in)
  time.Sleep(1e9)
  fmt.Println('delay')
}

func main() {
  out := make(chan int)
  go f1(out)
  out <- 2
  fmt.Println("end")
}
```
输出
```
 2
end
```
可见f1 的delay已经来不及输出了，因为main协程已经结束了。


总结
channel：
从动态来看： channel是一种消息传递的承载体
从静态来看：是一种锁（无法传递时，所在的协程无法向下运行）

延伸：
信号量
生产者、消费者

