# Go环境变量
Go环境变量是Go在下载依赖及执行和编译的时候所依赖的环境，通过配置环境变量，可以使我们准确拉取代码及为目标平台编译可执行文件。

#### 查看环境变量
```bash
# 使用如下命令可查看当前环境变量
go env

# 修改环境变量
go env -w GOPROXY=https://goproxy.cn,direct

# 查看环境变量是否生效
go env GOPROXY
```

#### 环境变量解析
**GO111MODULE**\
GO模块管理策略，基于go1.1.1以上版本：可选参数：off、on、auto，默认为 auto

**GOARCH**\
编译的目标平台架构或处理器，通过 go tool dist list 命令可查看允许的所有平台架构

**GOBIN**\
程序生成的可执行文件的路径，默认为 ~/go/bin，使用 go install 来链接编译后的包

**GOCACHE**\
GO编译时缓存的保存路径

**GOENV**\
GO的环境变量保存的文件地址，无法使用 go env -w 进行配置

**GODEBUG**\
是否开启GO的一些列调试工具

**GOEXE**\
可执行文件的后缀，windows为.exe其余为空

**GOFLAGS**\
为GO执行的命令添加而外的参数，GO的命名识别该参数时则会添加进去，配置方式为："-flag1=value1 -flag2=value2"

**GOHOSTARCH**\
GO的宿主机的平台架构或处理器

**GOHOSTOS**\
GO的宿主机的平台

**GOINSECURE**\
配置允许使用http拉取包的域名，多个域名用逗号分隔，配置此项并不会阻止数据库的包检查，可同时配置 GOPRIVATE 或 GONOSUMDB 来达到想要的效果

**GOMODCACHE**\
使用go mod下载的包的保存路径

**GONOPROXY**\
不进行代理的域名，多个域名用逗号隔开

**GONOSUMDB**\
不进行数据库的包合法性检查的域名，多个域名用逗号隔开

**GOOS**\
编译的目标平台，通过 go tool dist list 命令可查看允许的所有平台架构

**GOPATH**\
可以有多个，若干个工作区目录的路径，自定义的工作空间，默认为 ~/go

**GOPRIVATE**\
配置私有仓库地址，配置后会同时配置 GONOPROXY 及 GONOSUMDB，以达到访问私有仓库不走代理且不做数据库校验的效果

**GOPROXY**\
下载包时的代理，国内可配置为：[https://goproxy.cn,direct](https://goproxy.cn,direct)，direct是个特殊标识符，意味着代理拉不到包时回源下载

**GOROOT**\
GO语言的安装根目录，也是GO的安装路径

**GOSUMDB**\
拿来校验包合法性的数据库名称，以及可选的其公钥和URL配置

**GOTMPDIR**\
执行GO的命令时临时存放源文件、包、二进制文件的路径

**GOTOOLDIR**\
GO相关工具的存放路径

**GCCGO**\
当执行编译命令 go build -compiler=gccgo 时所执行的gccgo命令

**AR**\
当使用gccgo编译器进行编译时用来操作归档文件的命令，默认为ar

**CC**\
用来编译C语言代码的命令，mac上默认为clang

**CXX**\
用来编译C++语言代码的命令，，mac上默认为clang++

**CGO_ENABLED**\
是否开启cgo命令，默认为1开启，可选值为0或1

**GOMOD**\
项目主包的go.mod路径

**CGO_CFLAGS**\
编译C语言代码时，传递给编译器的参数

**CGO_CPPFLAGS**\
预处理C语言代码时，传递给编译器的参数

**CGO_CXXFLAGS**\
编译C++语言代码时，传递给编译器的参数

**CGO_FFLAGS**\
编译Fortran语言代码时，传递给编译器的参数

**CGO_LDFLAGS**\
编译器链接库时，传递给编译器的参数

**PKG_CONFIG**\
pkg-config工具的路径

**GOGCCFLAGS**\
使用CC编译C语言代码时，传递给编译器的参数
