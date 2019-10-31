* 20190820<br>
命令行执行go bulid命令，会将exe文件生成在当前命令执行的目录
```
C:\Users\Administrator> go build D:\GOCode\helloWolrd.go
会将exe文件生成在C:\Users\Administrator目录下,和GOPATH路径无关
```
* 20190910<br>
```
-- go和c --

普通变量(normal value)
c:int a = 0;
go:var a int = 0;

指针(pointer)
c:int *a=NULL;
go:var a *int = nil;

数组(array)
c:int a[10];
go:int a [10]int;//一定要指定长度，才是声明数组

切片(slice)
c:
go:int a []int;//不指定长度，就是声明切片
```
* 20190911<br>
```
go中没有类的概念，为了模拟类的效果，将命名类型或者结构体类型的一个值或者是一个指针绑定在一个函数上，使其调用形如a.func()
func (val1 type) function_name() [return_type]{
   /* 函数体*/
}
常规调用函数是func(),当绑定之后调用方法就是a.func()

数据类型：
指针类型（Pointer）
数组类型（array）
结构化类型(struct)
Channel 类型
函数类型（func）
切片类型（slice）
接口类型（interface）
Map 类型

var a *int			//指针
var a []int			//切片
var a map[string] int		//
var a chan int			//
var a func(string) int		//
var a error 			// error 是接口
```
* 20190912<br>
```
--使用现有项目时，包依赖问题。
编译的

```
* 20191030<br>
```
通道channel
无缓冲的通道，写入和读取应该放在两个协程中，防止堵塞
带缓冲的通道可以在同一协程进行读写操作
```

* 20191031<br>
```
//go获取线程ID
package main

import (
	"fmt"
	"runtime"
	"strconv"
	"strings"
)

func GetGoid() int64 {
	var (
		buf [64]byte
		n   = runtime.Stack(buf[:], false)
		stk = strings.TrimPrefix(string(buf[:n]), "goroutine ")
	)

	idField := strings.Fields(stk)[0]
	id, err := strconv.Atoi(idField)
	if err != nil {
		panic(fmt.Errorf("can not get goroutine id: %v", err))
	}

	return int64(id)
}

```
