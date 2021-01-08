* `go get -v -u xxx` 安装xxx包，`-v`显示执行的命令，`-u`强制使用网络去更新包和它的依赖包。若在安装`包1`时提示`依赖的其他包`访问超时或连接失败，则使用clone方式下载该`依赖包`，然后重新`go get 包1`

* golang在github上建立了一个镜像库，如https://github.com/golang/net就对应是 https://golang.org/x/net的镜像库。 要下载golang.org/x/net包，可以在本地创建包的目录后使用git clone来拉取相应包的源代码文件
```
mkdir -p $GOPATH/src/golang.org/x
cd $GOPATH/src/golang.org/x
git clone https://github.com/golang/net.git
有些时候（所需要的包的）名称在github上不一样，应该先去github上搜索一下这个包，然后
git clone https://github.com/golang/go-xxx.git xxx
修改为所需要的名称
```

* `package go.micro.srv.consignment;` 指定package, 是为了避免和其他的.proto文件的message名称冲突
`参考https://juejin.cn/post/6865126893063471112`

* `option go_package=".;consignment";` 作用是去指定生成的go文件的package的完整导入路径。分号后面是指定包名

* `/e/proto/bin/protoc.exe --go_out=plugins=grpc:. consignment.proto` 将$gopath/bin加入环境变量path，$gopath/bin中可以看得到protoc-gen-go.exe
