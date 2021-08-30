# Node 版本控制



## nvm 版本管理

### 下载

- 包地址: https://github.com/coreybutler/nvm-windows/releases
- 点击 releases, 选择相应的包 nvm_setup.zip



### 安装

- 解压所下载的包
- 双击安装,建议 都安装在 d 盘,不占 c 盘空间.



### 测试

- 通过命令: nvm version 查看版本



### 配置

- 由于 nodejs 的包来自国外,我们下载时很慢,甚至会中断.
- 为解决这一弊端,建议修改 nvm 的包源地址.执行以下命令.
- nvm node_mirror https://npm.taobao.org/mirrors/node/
- nvm npm_mirror https://npm.taobao.org/mirrors/npm/



### 基本命令

- nvm arch    ----    当前 nvm 版本位数
- nvm on    ----    打开版本控制
- nvm off    ----    关闭版本控制
- nvm list available    ----    查看可下载的 node 版本(关注 LTS)
- nvm list    ----    查看已下载的 node 版本
- nvm use [version]    ----    切换为指定版本 node



## Node 源码包

### 下载/安装

- 包地址: https://nodejs.org/en/
- 淘宝镜像地址: https://npm.taobao.org/mirrors/node/

- nvm install [version]   ----    下载/安装制定版本 node



### 测试

- node -v    ----    查看 node 版本
- npm -v    ----    查看 npm 版本



## Npm 源码包

### 下载/安装

包地址: https://github.com/npm/npm/releases

淘宝镜像地址: https://npm.taobao.org/mirrors/npm/



### 配置

- 由于 npm 的包源有可能来自国外,下载/安装时可能很慢,甚至中断.
- 为解决这一弊端,建议修改 npm 的包源地址，执行以下命令.
- npm config set registry https://registry.npm.taobao.org;
- npm config get registry



### 常用命令

- 环境

  - 全局环境 -g

  - 生产环境 --save

  - 开发环境 --save-dev



- npm help [command]    ----    查看 npm 帮助.

- npm -v    ----    查看 npm 版本.
- npm root -g    ----    查看 npm 全局安装地址.
- npm run    ----    列出项目所有脚本命令参数.

- npm init [-y]    ----    在当前目录生成 package.json 文件.



- npm install [-g] [*module*]    ----    安装指定模块
- npm install [-g] [*module*]@[*version*] 降/升级安装指定版本模块
- npm uninstall [-g] [*module*]    ----    移除指定模块
- npm update[-g]    ----    更新所有模块
- npm update [-g] [*module*]    ----    更新指定模块



- npm list [-g] [--depth=0]   ----    查看已安装模块,依赖包层级目录 --depth
- npm list [-g] [*modnpmule*]    ----    查看 npm 依赖包的版本
- npm show [*module*]    ----    查看模块详情



- npm outdated 查看当前过时依赖.
- npm search [*keyword*]    ----    根据关键字查看依赖包.
- npm prune    ----    移除不在 package.json 定义的依赖包.



- npm repo [*module*]    ----    在浏览器打开模块包地址.

- npm into [*module*]    ----    查看模块 package.json 文件.

- npm view [*module*]    ----    查看模块 package.json 文件(等同 npm info).
- npm view [*module*] version    ----    查看模块的最新版本号.
- npm view [*module*] versions    ----    查看模块的所有版本号.

- npm dedupe    ----    移除冗余模块.





# Git 操作

## 配置用户名与邮箱

### 查看 git 用户名与邮箱

```js
git config user.name
git config user.email
```



### 修改全局 git 用户名与邮箱

```js
git config --global user.name 'username'
git config --global user.email 'useremail'
```



## 配置 SSH

### 检测是否配置 ssh

```cmd
cd ~/.ssh
```



### 通过邮箱生成公钥

```cmd
ssh-keygen -t rsa -C 'you_email'
```



### 查看已生成的公钥

```cmd
cat ~/.ssh/id_rsa.pub
```



### 在 github 中配置 ssh

- 复制已生成的公钥.
- 进入 guthub 官网,点击 settings, 进入 SSH and GPG keys.
- 点击 New SSH Keys, title 随便填, key 就填 已复制的公钥.
- 检测是否配置成功,输入以下命令:

```cmd
ssh -T git@github.com


# 可能会出现以下问题
The authenticity of host 'github.com (13.229.188.59)' can't be established.
RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
Are you sure you want to continue connecting (yes/no/[fingerprint])?

# 解决: 输入 yes

```



### 在 gitee 中配置ssh

- 复制已生成的公钥.
- 进入 gitee 官网,点击 ''设置'', 进入 'SSH 公钥'.
- 标题随便填, 公钥就填已复制的公钥.
- 检测是否配置成功,输入以下命令:

```cmd
ssh -T git@gitee.com


# 可能会出现以下问题
The authenticity of host 'github.com (13.229.188.59)' can't be established.
RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
Are you sure you want to continue connecting (yes/no/[fingerprint])?

# 解决: 输入 yes

```





## 命令操作概念

git add .    ----    将修改后的文件提交到版本库(暂存区).

git commit -m 'info'    ----    将版本库的所有内容提交到当前分支.







## 命令大全

### Git GUI 面板

gitk    ----    打开 GUI 面板



### 文件相关命令

mkdir [folder]    ----    创建本地(版本)库|(工作区)

echo 123 >> [file]    ----     在 file文件内容的下一行写入 123

cat  [file]    ----     读取 file 文件内容

rm [file]    ----    移除 file 文件

rm -r [folder]    ----    移除 folder 文件夹

pwd    ----    读取当前路径

ls    ----     查看当前文件夹下的文件目录内容

ls [folder]    ----     查看 folder 文件夹下的文件目录内容

ls -l    ----    查看 git 提交记录及所修改的文件名

ls -l -a     ----    查看 git 提交记录及所修改的文件名(包括隐藏文件)

​    

### 目录切换相关命令

cd ~    ----    切换到 Root 目录

cd /    ----    切换到根目录

cd -    ----    切换到上一状态

cd [folder]    ----    切换到某目录

cd ..    ----    切换到上一级目录



### vim 模式相关命令

vim [file]  新建文件 file

i    ----    插入内容

esc -> :wq     ----    保存并退出

esc -> :q    ----    直接退出



### 文件修改历史记录相关

git blame [file]    ----    查看 file 文件修改历史记录

git blame -L [row-number] [limit-number] [file]    ----    查看 file 文件 row-number到 (row-number + limit-number) 之间的修改信息







### 常用 Git 命令

git clone [link]    ----    克隆指定仓库到本地

git clone -b [breach] [link]    ----    克隆指令仓库的分支到本地

git status    ----    查看状态

git add [file1] [file2]   ----    将 file1, file2 文件存入暂存区

git add .    ----    将所有文件存入暂存区

git add -p [file]    ----    将 file 文件多次提交到暂存区

git commit -m '[info]'    ----    备注推送的信息

git push    ----    将暂存区的代码推送至远程仓库

git push origin --tag    ----    将所有打好的标签推送到远程仓库

git push orgin [tag]    ----    将指定标签推送到远程仓库

git push origin :refs/tags/[tag]    ----    删除远程仓库的标签

git push -u origin [breach]    ----    将本地分支推送到 origin 主机,且指定 origin 为默认主机

git checkout -- [file]  撤销对 [file] 文件的修改;未提交到暂存区, 返回工作区版本；已提交到暂存区,返回暂存区版本



git branch  查看本地分支

git branch -r 查看远程分支





## GIT 命令

```bash
git init  项目初始化
git clone 'git_address'
git remote -v  查看远程仓库地址

git branch  查看当前分支
git branch -a 查看所有分支
git branch -r 查看所有远程分支

git pull 更新(拉取最新)代码到本地
git status  查看本地文件信息
git add .  添加文件到缓存区
git commit -m ‘info’  在缓存区备注信息
git push origin dev  提交代码到当前分支(dev)
git pull origin dev  拉取 dev 分支的代码包

git checkout dev  切换为dev分支(可以是远程)

git checkout -b localBranch origin/remoteBranch 拉取远程分支代码并创建本地分支
git push origin newRemoteBranch 创建远程新分支
git branch --set-upstream-to=origin/newRemoteBranch  将本地分支映射到远程新分支

git fetch  将本地分支与远程保持同步

git log  查看提交日志
git tag  查看版本号
git diff  查看未提交的更新

```









## 实现本地上传至 github 指定仓库

- 在 github 官网创建一个仓库,并勾选 Initialize this repository with a README 选项.
- 创建|在一个本地库(工作区),文件夹命名随便.
- 进入工作区,生成 git 仓库.

```cmd
git init
```

- 查看仓库状态

```cmd
git status
```

- 将修改的内容推送至缓存区

```cmd
git add .
```

- 将缓存区的内容生成唯一记录

```cmd
git commit -m 'upload info'
```

- 与 github 官网创建的远程仓库关联

```cmd
# git_package.git 新创建的仓库
git remote add origin https://github.com/XiaoYimi/git_package.git
```

- 把本地库内容推送到远程仓库

```cmd
# 把本地库内容推送到远程仓库
git push -u origin master

# 可能出现的问题 (由于本地库没有 readme.md,而远程仓库有)
To https://github.com/XiaoYimi/vue-source-code.git
 ! [rejected]        master -> master (fetch first)
error: failed to push some refs to 'https://github.com/XiaoYimi/vue-source-code.git'

# 解决,执行以下命令 (合并远程仓库)
git pull --rebase origin master

# 把本地库内容推送到远程仓库
git push -u origin master


# 本地库可能没有 readme.md 文件,可执行以下两套命令:
git pull --rebase origin master
git push -u origin master


# 可能出现的问题，分支权限不够
remote: Resolving deltas: 100% (44/44), done.
remote: GitLab:
remote: A default branch (e.g. master) does not yet exist for James/milu_miniapp_merchant
remote: Ask a project Owner or Maintainer to create a default branch:
remote:
remote:   http://120.24.159.7:8089/James/milu_miniapp_merchant/-/project_members
remote:
To http://120.24.159.7:8089/James/milu_miniapp_merchant.git
 ! [remote rejected] master -> master (pre-receive hook declined)
error: failed to push some refs to 'http://120.24.159.7:8089/James/milu_miniapp_merchant.git'

# 解决，让超级管理员设置开发者权限

```





## Git 常见冲突



### 拉取其它代码出现的冲突

```bash
当我们在拉取分支时, 通过 git pull 命令出现错误; (currentBranch|Merging)

一般情况下是代码上存在差异所导致,所以对于这种冲突的处理，一般通过手动选择代码差异上所要保留的代码,对于不需要保留的代码可选择删除或者注释.

然后这时候确定全部已修改,即可再次提交代码(git 操作).

当我们保存到暂存区并备注时, 就可消除 (merging) 状态,恢复到当前分支 (currentBranch).

~(currentBranch)
$ git pull origin remoteBranch

/* 手动修改 冲突文件 */

~(currentBranch|Merging)
$ git status
$ git add .
$ git commit -m 'fix conflit file'

~(currentBranch)
$ git push origin currentBranch

/* 冲突修改完毕,并且成功拉取其它分支代码 */
```







































