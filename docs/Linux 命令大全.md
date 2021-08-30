# 命令大全

## XShell 命令

### 用户组与用户

#### 新增用户

##### useradd [opt] [username]

```shell
# 新增普通用户 xiaoyimi
useradd xiaoyimi

# 新增全局用户 xiaoyimi
useradd -g xiaoyimi

# 新增系统用户 xiaoyimi
useradd -r xiaoyimi

# 新增全局用户 xiaoyimi 到用户组 team
useradd -g root xiaoyimi
```



#### 设置密码

#####  passwd [options] [username] 



【options】

- -d 删除密码
- -f 强迫用户下次登录时必须修改口令
- -w 口令要到期提前警告的天数
- -k 更新只能发送在过期之后
- -l 停止账号使用
- -S 显示密码信息
- -u 启用已被停止的账户
- -x 指定口令最长存活期
- -g 修改群组密码
- 指定口令最短存活期
- -i 口令过期后多少天停用账户



#### 修改用户

#####  chage [option] [username] 



【option】

- -d：指定密码最后修改日期
- -E：密码到期的日期
- -l：列出用户以及密码的有效期
- -m：密码能够更改的最小天数
- -M：密码保持有效的最大天数



#### 删除用户

#####  userdel  [option] [username]





### 文件与目录权限





## Vim 命令
