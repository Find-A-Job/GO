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
编译的程序在开始时会去下载依赖包，但部分网站因为被墙而无法访问下载。
这时候需要设置一下代理，来翻墙
1.cd进入工程目录。
2.命令行输入export GOPROXY=https://goproxy.io来设置代理。
3.接着进行编译go run xxx.go

相关信息，xxx.mod文件，所依赖的包在这个文件中可以查看

--go中的各种符号
".":点符号，使用(带接收器的)函数，结构体，包等都会用到这个符号
//
typedef str2 struct{}
func (s str2) f1() int{return 0}
var val1 str2
val1.f1()//调用(被绑定到str2结构体的)函数

//
typedef str1 struct{name string}
var val2 str1
val2.name//调用结构体内变量

//
import "time"
import "fmt"
var val3 time.Time//调用包内定义的类型
fmt.Println()//调用包内函数

//
import . "fmt" //加一个点表示调用此包的函数时，可以省略前缀的包名
Println()

"*":
"&"
"<-"管道
"...":可变参数
"[]":方括号，数组，切片
"()":圆括号，
"{}"
"_":下划线
//
import _ "fmt" //加下划线表示，导入此包时，该包内所有的init9()函数都将被执行，但无法通过包名来调用包内其他函数

//
func f1() (a int, b int) {return 1,2}
_ , a := f1() //该下划线是匿名变量，不可被用作右值

```
* 20190916<br>
```
--go变量生命周期（区别于作用域）
取决于是否有外部指针指向这个变量

使用var和new关键字来声明变量，其生命周期是一样的
并不是定义在函数内部的局部变量在访问退出函数后就会被回收
函数退出时，没有指向这块内存的指针了，变量就会被垃圾回收

//不会被回收
var p *int
func f(){
    var i int
    i = 1
    p = &i
}

//会被回收
func f(){
    p := new(int)
    *p = 1
}

--嵌入类型
可以理解为继承

--
```
