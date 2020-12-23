# Git文件忽略
对于不同的操作系统及不同的项目，在工程目录下总会有一些文件是不需要加入Git源码管理的，这个时候我们就需要配置Git来忽略这些文件夹或文件，我们通过配置项目下的`.gitignore`文件来达到这个效果。

例如以下配置忽略了 node_modules 及 .vscode 两个文件夹，更多详情参考 [GitBook](https://git-scm.com/book/zh/v2)
```txt
node_modules
.vscode
```

推荐使用GitHub官方推出的 [gitignore](https://github.com/github/gitignore) 工程下的对应文件，包含了众多类型的忽略文件，使用方法为：
```bash
# 克隆工程
git clone git@github.com:github/gitignore.git

# 拷贝对应的 .gitignore 文件到项目根目录下
cp gitignore/Unity.gitignore path/to/your/project/.gitignore

# 进入项目地址
cd path/to/your/project

# 查看添加忽略文件配置后的Git项目状态
git status
```
