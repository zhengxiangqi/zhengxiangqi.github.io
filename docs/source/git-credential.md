# Git凭证管理
通过Git凭证，我们可以记录仓库的用户名及密码，避免Git操作时频繁的输入用户名和密码。

#### 配置Git记住用户名密码
在访问http服务器时，或者拉取私有仓库时，如果使用的不是ssh-key方式，就需要输入用户名及密码，可以配置Git将用户名密码记录下来。

**允许Git命令行进行授权**
```bash
# 配置系统环境变量，在命令行初始化文件中添加以下语句，允许命令行执行过程中输入用户名密码
# 路径为 ~/.bashrc，配置过zsh的则是 ~/.zshrc
export GIT_TERMINAL_PROMPT=1
```

**永久记录**
```bash
# 被记住的用户名和密码，会生成 ~/.gitcredential 文件，将用户名密码明文存储在文件里
# 后续要编辑或者删除直接修改 ~/.gitcredential 文件即可
git config --global credential.helper store
```

**缓存记录**
```bash
# 也可以配置记录在系统缓存里，并可以设置过期时间，默认为15分钟即900
git config --global credential.helper "cache,timeout=3600"
```

**关闭缓存记录**
```bash
# 已保存的秘钥并不会因此而直接失效，还是会等待过期时间
git credential-cache exit
```

**手动添加凭证**
```bash
# 执行后输入 protocol=<http/https> 然后换行
# 继续输入 host=<HOSTNAME> 换行两次
# 若不存在凭证则提示输入用户名密码，若已存在则显示对应凭证信息
git credential fill
```

**手动删除凭证**
```bash
# 执行后输入 protocol=<http/https> 然后换行
# 继续输入 host=<HOSTNAME> 换行两次
git credential reject
```

**清除已记录的域名秘钥配对信息**
```bash
rm ~/.ssh/known_hosts
```

**添加http头部凭证**
```bash
git config --global http.<HOSTNAME>.extraheader "PRIVATE-TOKEN: YOUR_PRIVATE_TOKEN"

# 例如
git config --global http.http://git.test.com.extraheader "Authorization: AKwmrypz7UqtqWR5sawq2"
```
