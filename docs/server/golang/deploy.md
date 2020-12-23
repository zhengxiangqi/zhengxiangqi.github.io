# 项目部署

#### 编译项目
```bash
# 在项目根路径下执行编译命令，该命令会自动寻找 main.go 文件并进行编译
go build -o app

# 若要指定编译的包入口，跟上路径及文件名即可
go build main.go -o app
```

#### 执行二进制文件
```bash
# 执行程序并将其挂在后台
./app &
```

#### 查看后台进程
```bash
ps aux | grep app
```

#### 使用Docker Compose编译运行
在项目路径下新建`docker-compose.yml`和`Dockerfile`两个文件，然后执行启动命令。

```yml
# docker-compose.yml
version: "3.7"

services:
  test:
    build: .
    container_name: test
    ports:
      - "8080:8080"
      - "8088:8088"
```

```dockerfile
# Dockerfile
FROM library/golang:1.15.3

ENV GOPROXY https://goproxy.cn
ENV GOARCH amd64
ENV GOOS linux
ENV APP_DIR $GOPATH/src/app

WORKDIR $APP_DIR
COPY go.mod go.sum $APP_DIR/
RUN go mod download
COPY . $APP_DIR
RUN go build -o app
ENTRYPOINT ["app"]
```

```bash
# 执行启动命令，参数 d 使其运行在后台，参数 build 使其每次执行都重新编译镜像
docker-compose up -d --build
```
