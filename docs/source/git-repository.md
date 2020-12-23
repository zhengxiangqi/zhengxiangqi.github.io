# Git仓库搭建
大多公司都会搭建内部Git仓库，可以统一管理公司各个项目的源码，对源码管理有重要意义。

#### 使用Docker Compose搭建Gogs
Gogs是使用Go编写的一款开源免费的Git仓库管理工具，易安装、跨平台、轻量级，喜欢的可以尝试。

配置Gogs环境变量，后续对应的Gogs配置及日志等都将存储于该目录下，更多详情参考 [GitHub Gogs](https://github.com/gogs/gogs/tree/master/docker)。
```bash
# Linux环境
export GOGS_HOME=/srv/gogs

# MacOSX环境
export GOGS_HOME=$HOME/gogs
```

在 GOGS_HOME 目录下新建 docker-compose.yml 文件
```yml
# docker-compose.yml
version: "3.7"

services:
    gogs:
        image: "gogs/gogs:latest"
        container_name: gogs
        restart: always
        ports:
            - "10022:22"
            - "10080:3000"
        volumes:
            - "$GOGS_HOME:/data"
        depends_on:
            - mysql
    mysql:
        image: "mysql:latest"
        container_name: mysql
        restart: always
        ports:
            - "3306:3306"
        environment:
            - MYSQL_ROOT_PASSWORD=123456
```

```bash
# 在GOGS_HOME目录下，执行启动命令，参数 d 使其运行在后台
docker-compose up -d

# 启动后访问本地ip地址或localhost
http://localhost:10022
# 首次访问需要进行配置
http://localhost:10080
```

#### 使用Docker Compose搭建GitLab
GitLab是大多数公司都在使用的一款Git仓库管理工具，社区内容丰富，深受广大开发者喜爱。此处使用的是社区版，企业版需要购买额外的服务，更多详情参考 [GitLab Products](https://about.gitlab.com/products/)。

配置GitLab环境变量，后续对应的GitLab配置及日志等都将存储于该目录下，更多详情参考 [GitLab Docker images](https://docs.gitlab.com/omnibus/docker/)。
```bash
# Linux环境
export GITLAB_HOME=/srv/gitlab

# MacOSX环境
export GITLAB_HOME=$HOME/gitlab
```

在 GITLAB_HOME 目录下新建 docker-compose.yml 文件
```yml
# docker-compose.yml
version: "3.7"

services:
    gitlab:
        image: "gitlab/gitlab-ce:latest"
        container_name: gitlab
        restart: always
        hostname: "gitlab.example.com"
        environment:
            GITLAB_OMNIBUS_CONFIG: |
            # 配置为有效的外部或内部域名，若没有亦可不配置，配置后访问机器ip会跳转到此url
            external_url "https://gitlab.example.com"
            # Add any other gitlab.rb configuration here, each on its own line
        ports:
            - "80:80"
            - "443:443"
            - "22:22"
        volumes:
            - "$GITLAB_HOME/config:/etc/gitlab"
            - "$GITLAB_HOME/logs:/var/log/gitlab"
            - "$GITLAB_HOME/data:/var/opt/gitlab"
```

```bash
# 在GITLAB_HOME目录下，执行启动命令，参数 d 使其运行在后台
docker-compose up -d

# 首次启动时间较长，可使用如下命令查看启动日志
docker logs -f gitlab

# 启动后访问本地ip地址，初次可能出现502错误
http://localhost

# 刷新等待后会进入修改密码的界面，修改密码后即可使用root账号加刚才的密码登录
```
