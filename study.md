* 20190820<br>
命令行执行go bulid命令，会将exe文件生成在当前命令执行的目录
```
C:\Users\Administrator> go build D:\GOCode\helloWolrd.go
会将exe文件生成在C:\Users\Administrator目录下,和GOPATH路径无关
```
* 20190910<br>
```
go和c的联系：<del>就是将变量名与类型说明符调换</del>
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
