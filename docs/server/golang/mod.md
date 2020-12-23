# Go依赖管理
Go里面组织管理依赖的方式从早期版本到1.1.1之后的版本，有一定的差异。早期使用go get及vendor或者第三方包的方式管理依赖，在go1.1.1版本之后，官方推出了go modules方式来管理依赖，此处主要讨论`go mod`方式管理依赖。

#### 初始化项目
```bash
# 在项目路径下执行如下命令初始化项目，使其被go mod管理，执行完成后项目根路径会生成一个 go.mod 文件
go mod init <PACKAGE_NAME>

# 若要将这个包发布到私有仓库，则包名会长一些，例如
go mod init git.test.com/username/packagename
```

#### 添加项目依赖
```go
// go.mod
module Learning

go 1.15

require (
    github.com/satori/go.uuid v1.2.0
)
```

#### 代理设置
Go官方的镜像拉取速度较慢，可配置国内代理来提速，更多细节参考 [Go环境变量](/server/golang/env.md)
```bash
go env -w GOPROXY=https://goproxy.cn,direct
```

#### 配置私有仓库
拉取包时，当拉取的是私有仓库时，此时proxy中无法找到对应的包，则会下载报错，需要进行对应的配置：
1. 方法一：配置其余私有proxy来获取对应的包
2. 方法二：配置direct使其回源下载，但未配置 GONOPROXY 和 GONOSUMDB 则获取源地址时仍未绕开proxy，
    此时可配置 GOPRIVATE=`<HOSTNAME>` 指定此为私有仓库地址，此举会自动配置上 GONOPROXY 和 GONOSUMDB

**使用ssh-key拉取私有仓库**
1. 在本地生成ssh公钥私钥对，具体如何生成参考 [生成ssh-key](/source/git-ssh-key.md)
2. 在私有仓库上的个人中心设置中新建ssh-key，并填入刚刚生成的公钥
3. 在本地.ssh目录配置config文件，加入对应的host及私钥地址信息等配置，例如：
    ```txt
    host git.test.com
        identityfile ~/.ssh/id_rsa
    ```
4. 配置私有仓库地址
    ```bash
    # 若私有仓库支持的是https协议，则配置如下
    go env -w GOPRIVATE=git.test.com

    # 若私有仓库支持的是http协议，则配置如下
    go env -w GOPRIVATE=git.test.com GOINSECURE=git.test.com
    ```
5. 配置Git协议及域名替换规则，例如（若只想配置当前项目而不想全局配置Git，可以移除 --global 参数）：
    ```bash
    # 若私有仓库支持的是https协议，则配置如下
    git config --global url.git@git.test.com:.insteadOf https://git.test.com/

    # 若私有仓库支持的是http协议，则配置如下
    git config --global url.git@git.test.com:.insteadOf http://git.test.com/

    # 经过Git配置后，在下载仓库时，例如： http://git.test.com/username/project.git
    # 会将其转换成使用ssh-key的方式：git@git.test.com:username/project.git
    ```
6. 执行依赖下载命令
    ```bash
    go mod download

    # 若想查看下载过程的细节，可添加 -x 参数
    go mod download -x
    ```

**使用用户名密码拉取私有仓库**
1. 配置私有仓库地址
    ```bash
    # 若私有仓库支持的是https协议，则配置如下
    go env -w GOPRIVATE=git.test.com

    # 若私有仓库支持的是http协议，则配置如下
    go env -w GOPRIVATE=git.test.com GOINSECURE=git.test.com
    ```
2. 配置系统环境变量，使`go mod`命令执行过程中允许git输入用户名密码，没配置会被执行日志覆盖而导致没机会输入
    ```txt
    export GIT_TERMINAL_PROMPT=1
    ```
3. 配置Git永久记住用户名密码，想要了解如何清除已记录的凭证，参考 [Git凭证管理](/source/git-credential.md)
   ```bash
    # 被记住的用户名和密码，会生成 ~/.gitcredential 文件，将用户名密码明文存储在文件里
    git config --global credential.helper store

    # 也可以配置记录在系统缓存里，并可以设置过期时间，默认为15分钟即900
    git config --global credential.helper "cache,timeout=3600"
    ```
4. 执行依赖下载命令
    ```bash
    go mod download

    # 若想查看下载过程的细节，可添加 -x 参数
    go mod download -x
    ```

**使用AccessToken拉取私有仓库**
1. 配置私有仓库地址
    ```bash
    # 若私有仓库支持的是https协议，则配置如下
    go env -w GOPRIVATE=git.test.com

    # 若私有仓库支持的是http协议，则配置如下
    go env -w GOPRIVATE=git.test.com GOINSECURE=git.test.com
    ```
2. 在私有仓库个人中心设置中新建AccessToken并勾选全部权限，以在http或者https协议时免密拉取
3. 配置Git使用AccessToken（若只想配置当前项目而不想全局配置Git，可以移除 --global 参数）：
    ```bash
    # 若私有仓库支持的是https协议，则配置如下
    git config --global url.https://oauth2:ACCESS_TOKEN@git.test.com.insteadOf https://git.test.com

    # 若私有仓库支持的是http协议，则配置如下
    git config --global url.http://oauth2:ACCESS_TOKEN@git.test.com.insteadOf http://git.test.com

    # 经过Git配置后，在下载仓库时，例如： http://git.test.com/username/project.git
    # 会将其转换成使用AccessToken的方式：http://oauth2:ACCESS_TOKEN@git.test.com/username/project.git
    # 注意：若首次配置错误，又重复执行多次配置命令，会导致生成多条记录而拉取失败
    # 此时可执行 git config --global -e 进入文档编辑直接修改移除配置
    ```

#### 升级自定义包版本
模块标准化了一个重要原则，导入兼容原则：如果旧包和新包具有相同的导入路径，则新包必须向后兼容旧包。根据定义，包的新主版本与以前的版本不向后兼容，这意味着模块的新主版本必须具有与以前版本不同的模块路径，从 v2 开始，主版本必须出现在模块路径的末尾，例如，模块的 v2 版本需要在工程下创建 v2 文件夹来编写新版本。对主要版本后缀的要求是 Go 模块与大多数其他依赖关系管理系统的不同之处之一，需要后缀来解决钻石依赖问题。

使用者导入包时也要跟上对应路径 git.test.com/username/packagename/v2。

若引入的包为非 go mod 管理的包，而版本又大等于 v2.0，则可以在引用包版本号后加上"+incompatible"标记来兼容。


#### 项目中可能出现的包引用方式有
1. git.test.com/xxx/pkg v1.0.6
2. git.test.com/xxx/pkg/v2 v2.0.0
3. git.test.com/xxx/pkg v3.2.0+incompatible
4. git.test.com/xxx/pkg v0.0.0-20190525155149-18097f1d08f4
5. git.test.com/xxx/pkg latest
6. git.test.com/xxx/pkg master
