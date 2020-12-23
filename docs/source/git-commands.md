# Git常见命令
掌握Git的常见命令对于日常工作中非常有帮助，除了命令行模式，也可以使用IDE提供的可视化工具，道理都是一样的。以下仅列出了常用操作，但实际上每个命令都还有很多内容值得深挖，配合 git help 命令可以查看更多细节，或者阅读 [GitBook](https://git-scm.com/book/zh/v2)。

**初始化仓库**
```bash
# 将你的项目作为Git仓库，加入源码管理
cd path/to/you/repository
git init
```

**克隆代码**
```bash
# 将远程代码克隆到本地
git clone git@hostname.com:username/repository.git
```

**拉取远程代码更新并合并**
```bash
# 拉取远程仓库分支的更新到本地分支并直接合并，使其同步，可能存在冲突，需要手动解决
git pull origin <BRANCH_NAME>
```

**拉取远程代码更新到本地**
```bash
# 拉取远程仓库的更新到本地仓库，但并不执行合并，本地依然是原来的代码
git fetch origin
```

**合并代码**
```bash
# 将某个分支合并到指定分支
git merge <SRC_BRANCH> <TARGET_BRANCH>
```

**推送代码更新**
```bash
# 将本地分支的更新推送到远程代码的分支，使其同步
git push origin <BRANCH_NAME>
```

**添加改动的文件到本地暂存环境**
```bash
# 添加指定的文件到本地暂存环境
git add <FILE_NAME>

# 添加所有文件到本地暂存环境
git add -A
```

**提交本地暂存的更新到本地分支**
```bash
# 执行后会进入交互式的文本编辑器来输入更新日志
git commit

# 使用简短更新信息，不进入交互式的文本编辑器
git commit -m "Update message"

# 重新编写此次的更新日志，需在push之前操作
git commit --amend
```

**查看当前仓库的状态**
```bash
# 将列出当前分支的所有改动
git status
```

**检出代码**
```bash
# 检出仓库的某个分支
git checkout <BRANCH_NAME>

# 检出仓库的某个标签
git checkout <TAG_NAME>

# 检出仓库的某个提交节点
git checkout <HASH_NAME>

# 检出分支的同时在本地创建同名分支
git checkout -b <BRANCH_NAME>
```

**分支管理**
```bash
# 列出本地所有分支
git branch

# 列出包含远程在内的所有分支
git branch -a

# 删除分支
git branch -d <BRANCH_NAME>

# 删除远程分支（谨慎操作）
git push origin branch -d <BRANCH_NAME>
```

**标签管理**
```bash
# 列出本地所有标签
git tag

# 列出包含远程在内的所有标签
git tag -a

# 新建标签
git tag <TAG_NAME>

# 删除标签
git tag -d <TAG_NAME>

# 删除远程标签（谨慎操作）
git push origin tag -d <TAG_NAME>
```

**临时暂存本地改动**
```bash
# 此命令并非将文件改动保存到暂存区，而是单独的一个存储空间，用于存放临时的改动，以便后续再提取出来

# 将当前分支的改动保存到临时暂存，不包含新增的文件
git stash

# 将当前分支的改动保存到临时暂存并包含描述，不包含新增的文件
git stash -m "hello world"

# 列出所有临时暂存
git stash list

# 将最近的一个临时暂存应用到当前分支
git stash apply

# 将指定临时暂存应用到当前分支
git stash apply <STASH_NAME>

# 将最近的一个临时暂存应用到当前分支并删除该临时暂存
git stash pop

# 将指定临时暂存应用到当前分支并删除该临时暂存
git stash pop <STASH_NAME>

# 删除指定临时暂存
git stash drop <STASH_NAME>

# 清除临时暂存
git stash clear
```

**查看当前分支提交记录**
```bash
# 列出当前分支的所有提交记录
git log

# 列出指定数量的提交记录，n可以是任意数字
git log -n

# 列出当前分支的所有提交记录并显示变更的文件
git log --stat

# 列出当前分支的所有提交记录，以单行方式显示，此处 pretty 可选参数有好几个，使用 help 命令查看更多细节
git log --pretty=oneline

# 根据提交者查找提交记录
git log --author=<USER_NAME>

# 根据提交者时间范围查找提交记录，使用 help 命令查看更多细节
git log --before="2020-12-22 17:00:00" --after="2020-12-21 00:00:00"
```

**对比差异**
```bash
# 查看尚未缓存的改动
git diff

# 查看指定文件尚未缓存的改动
git diff <FILE_NAME>

# 查看已缓存的改动
git diff --cached

# 查看已缓存的与未缓存的所有改动
git diff HEAD

# 显示摘要而非整个详情
git diff --stat

# 显示暂存区和上一次提交(commit)的差异
git diff --cached <FILE_NAME>
# 或者
git diff --staged <FILE_NAME>

# 显示两次提交之间的差异
git diff <HASH_NAME> <HASH_NAME> <FILE_NAME>
```

**重置**
```bash
# 保留工作目录，并清空暂存区
git reset HEAD

# 保留工作目录，并把重置 HEAD 所带来的新的差异放进暂存区
# 以下命令会产生一个最近一次提交与上一次提交之间的差异，该差异会被放进暂存区
git reset --soft HEAD^

# 重置stage区和工作目录到最新一次commit节点，即没有commit的修改都将被清除
git reset --hard HEAD

# 重置stage区和工作目录到上一次commit节点
git reset --hard HEAD^

# 重置stage区和工作目录到指定分支
git reset --hard <BRANCH_NAME>

# 重置stage区和工作目录到指定标签
git reset --hard <TAG_NAME>

# 重置stage区和工作目录到指定提交节点
git reset --hard <HASH_NAME>
```

**获取帮助**
```bash
# 查看帮助
git help

# 查看指定命令的帮助
git help <COMMAND_NAME>
```
