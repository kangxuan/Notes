# 生成rsa

```
ssh-keygen -t rsa

# 为.ssh目录设置权限
chmod 600 ~/.ssh/config
```

# config配置

```
# 主机别名，自己配置的，通过ssh work连接
Host work
# 主机名称
Hostname 192.168.33.10
# 账号
User root
# 端口
Port 22
# 私钥文件，通过ssh-keygen生成的
IdentityFile ~/.ssh/id_rsa
```

#### 目标主机配置

1. 修改sshd_config
   
   ```
   vim /etc/ssh/sshd_config
   // 打开选项
   AuthorizedKeysFile      .ssh/authorized_keys
   // 重启
   /etc/init.d/sshd restart
   ```
   
   ```
   # 将本地的~/.ssh/id_sra.pub内容拷贝
   ```

# 在目标主机的对应User的家目录中

# 写入校验keys，将id_sra.pub的内容拷贝进去即可

vim ~/.ssh/authorized_keys

```
#### 快速连接
```

ssh work

```

```
