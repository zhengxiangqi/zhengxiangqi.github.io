# 安装Node.js
一种简单的做法是直接从 [官方](https://nodejs.org/zh-cn/download/) 下载对应平台安装包进行安装，但是大多数前端开发者会使用另一种方式进行安装，即通过一个Node.js的版本管理器进行安装，这样做的好处是可以安装多个版本的Node.js进行环境隔离，同时又能方便的进行Node.js的版本切换，所以我们要介绍的也是第二种方式。

#### NVM
Node Version Manager简称nvm，是一个Node.js的版本管理器，[GitHub地址](https://github.com/nvm-sh/nvm)。

**安装方式**
```bash
# 通过curl安装
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.37.2/install.sh | bash

# 通过wget安装
wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.37.2/install.sh | bash

# 上述脚本会将nvm工程克隆至~/.nvm文件夹
```

**配置环境变量**
```bash
# 根据系统配置编译对应文件：~/.bash_profile, ~/.zshrc, ~/.profile, or ~/.bashrc
# 添加如下脚本来加载nvm
export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
```

**验证是否安装成功**
```bash
# 正常的话会输出nvm
command -v nvm
```

**使用方式**
```bash
# 安装最新版本的Node.js
nvm install node

# 安装指定版本的Node.js
nvm install v14.15.1

# 列出本地可用的Node.js版本
nvm ls

# 列出远端可用的Node.js版本
nvm ls-remote

# 列出远端可用的Node.js版本，只查看稳定版本
nvm ls-remote --lts

# 设置本地默认的Node.js版本
nvm alias default v14.15.1

# 切换到默认Node.js版本
nvm use default

# 切换到指定版本Node.js版本
nvm use v14.15.1

# 使用默认版本Node.js执行
nvm run default --version

# 使用指定版本Node.js执行
nvm run v14.15.1 --version

# 获取指定版本的Node.js的安装地址
nvm which v14.15.1

# 卸载指定版本Node.js
nvm uninstall v14.15.1
```
