# ssh-key
在使用ssh拉取Git仓库时，使用ssh-key可以达到免密拉取的效果，即具备安全性，也具备易用性，推荐使用。

#### ssh-key生成方式
```bash
# Git安装完成后会自带 ssh-keygen 工具，参数 t 指定类型为 rsa 类型，参数 C 指定描述为邮箱地址
# 之后会提示输入秘钥文件保存路径、文件名以及密码（也可不设置密码直接按 Enter 跳过）
ssh-keygen -t rsa -C "name@test.com"
```

#### 配置域名和秘钥配对关系
```yml
# 文件路径 ~/.ssh/config，若不存在该文件可自行创建
host git.test.com
    identityfile ~/.ssh/id_rsa
```

#### 验证秘钥是否工作正常
需要在目标服务器将 id_rsa.pub 公钥内容填入个人ssh-key中
```bash
ssh -T git@git.test.com
```
