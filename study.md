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

* 20191110<br>
```
//文件属性
package main

import (
	"fmt"
	"io/ioutil"
	"os"
)

func main() {
	info, ok := os.Stat("C:\\Users\\Administrator\\Desktop\\EasyX_Help.chw")
	if ok != nil {
		fmt.Printf("读取失败:%v\n", ok)
	}

	// Name() string       // base name of the file
	// Size() int64        // length in bytes for regular files; system-dependent for others
	// Mode() FileMode     // file mode bits
	// ModTime() time.Time // modification time
	// IsDir() bool        // abbreviation for Mode().IsDir()
	// Sys() interface{}   // underlying data source (can return nil)
	fmt.Printf("文件信息:%v\n", info)
	fmt.Printf("文件名:%v\n", info.Name())
	fmt.Printf("文件大小(byte):%v\n", info.Size())
	fmt.Printf("文件模式:%v\n", info.Mode())
	fmt.Printf("文件修改时间:%v\n", info.ModTime())
	fmt.Printf("是否目录:%v\n", info.IsDir())

	fmt.Printf("\n")
	files, err := ioutil.ReadDir("c:\\")
	if err != nil {
		fmt.Printf("获取失败:%v\n", err)
	}

	for _, file := range files {
		fmt.Printf("文件名:%v\n", file.Name())
	}
}

/**/
//allData，usedData
//返回一个切片，内包含allData中的所有数据，
//allData，usedData中都不可以有重复值，且allData中不可出现0值
//思路:关键词#交换。因为所有值只出现一次(唯一性),将allData和usedData做对比
//若a[0]是非零值，则在b中找出该值，确定该值所在下标n，然后将b[n]与b[0]交换，然后判断a[1]，···。
//遍历完成后，a中的非零值均在b中有同样位置
func fillSlice(usedData []byte, allData []byte) []byte {
	retVal := make([]byte, len(allData))
	//检查入参合法性
	//...
	if len(usedData) > len(allData) {
		return retVal
	}
	//检查是否有重复值
	if checkRepeat(usedData) || checkRepeat(allData) {
		return retVal
	}

	copy(retVal, allData)
	//遍历并交换其值
	for ind, val := range usedData {
		if val != 0x0 {
			curVal := val
			changeVal := retVal[ind]

			changeValByVal(retVal, curVal, changeVal)
			retVal[ind] = curVal
		}
	}

	return retVal
}

func changeValByVal(data []byte, searchVal byte, replaceVal byte) {
	index := 0
	for ind, val := range data {
		if val == searchVal {
			index = ind
			break
		}
	}
	data[index] = replaceVal
}

func checkRepeat(data []byte) bool {
	return false
}
```
* 20191111<br>
```
//go获取文件属性
fileInfo, _ := os.Stat("test.log")
fileSys := fileInfo.Sys().(*syscall.Win32FileAttributeData)
fileAttributes:= fileSys.FileAttributes
fmt.Println(fileAttributes)

//准备一个集合m,m是allData的副本，将usedData和m做差集运算，得到集合n,然后将n和usedData组合成新的集合x，len(x)==len(m)
func fillSlice2(usedData []byte, allData []byte) []byte {
	retVal := make([]byte, 0, len(allData))
	temp := make([]byte, len(allData))
	//检查入参合法性
	//...
	if len(usedData) > len(allData) {
		return retVal
	}
	//检查是否有重复值
	if checkRepeat(usedData) || checkRepeat(allData) {
		return retVal
	}

	copy(temp, allData)
	//fmt.Printf("before:%v\n", temp)
	for _, val := range usedData {
		if val != 0x0 {
			temp = cutValFromSlice(temp, val)
		}
	}
	//fmt.Printf("after:%v\n", temp)

	retVal = append(retVal, usedData...)
	retVal = append(retVal, temp...)

	return retVal
}
func cutValFromSlice(data []byte, val byte) []byte {
	retVal := make([]byte, 0, len(data)-1)
	index := -1
	for ind, valLocal := range data {
		if valLocal == val {
			index = ind
		}
	}
	//fmt.Printf("index:%v\n", index)
	if index == -1 {
		return nil
	} else {
		if index == 0 {
			retVal = append(retVal, data[1:]...)
		} else if index == len(data)-1 {
			retVal = append(retVal, data[:len(data)-1]...)
		} else {
			retVal = append(retVal, data[:index]...)
			//fmt.Printf("retVal:%v\n", retVal)
			retVal = append(retVal, data[index+1:]...)
			//fmt.Printf("retVal:%v\n", retVal)
		}
	}
	//fmt.Printf("retVal:%v\n", retVal)
	return retVal
}
```
* 20191110<br>
```
//当前分支master
$ git branch
//从当前分支创建一个dev分支
$ git branch dev
//切换到dev分支
$ git checkout dev
//修改提交文件
$ git add -A
$ git commit -m "xxx"
//发现远程仓库的master分支有新文件被push
//转到master分支
$ git checkout master
//pull拉取新文件
$ git pull origin master
//这时候想将dev分支的文件和master同步(但不合并到master)
//不使用merge命令，这会使commit历史多出一条merge提交
$ git checkout dev
$ git rebase master
//将dev分支合并到master分支并push
$ git checkout master
$ git merge dev
$ git push origin master
//这时的commit历史是一条线
$ git log --graph
```
