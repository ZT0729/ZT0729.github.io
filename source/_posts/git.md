---
title: git
abbrlink: 518e617c
date: 2022-02-19 23:56:04
tags:
---
## git 配置
所有的配置文件都保存在本地！

查看不同级别的配置文件
```
#查看系统 config
git config --system --list

#查看当前用户（global）配置
git config --global --list
```

### git 相关的配置文件：
1. Git\etc\gitconfit : git 安装目录下的 gitconfig     --system 系统级
2. C:\Users\Administrator\ .gitconfig    只适用于当前登录用户的配置  --global 全局

### git 设置用户名与邮箱
首次安装 git 后先设置用户名和邮箱，之后每次 git 提交都会使用该信息，写入你的提交中。
```
git config --global user.name "tian"  #名称
git config --global user.email xxx@qq.com   #邮箱
```

### 生成公钥和秘钥
```
ssh-keygen -t rsa -C "xxx@qq.com"
```
生成公钥和私钥，不需要设置名称与密码，按3次Enter。

查看公钥
```
cat ~/.ssh/id_rsa.pub
```

公钥路径：C:\Users\Administrator\.ssh
