---
title: 搭建cloudreve私有云
tags:
  - cloudreve
  - 私有云
abbrlink: af971e
date: 2022-01-19 05:35:58
---
## 安装Cloudreve
本文仅描述在linux系统下的安装方式
首先需要在 GitHub Release 页面获取已经构建打包完成的主程序，并放置在自定的路径下。（例如/cloud）
然后连接到服务器，执行以下命令。
```
#转到指定路径
cd /cloud
#解压获取到的主程序
tar -zxvf cloudreve_VERSION_OS_ARCH.tar.gz
# 赋予执行权限
chmod +x ./cloudreve
# 启动 Cloudreve
./cloudreve
```
保存好管理员账号和密码。
此时已经可以通过http://服务器IP:5212访问Cloudreve。（5212端口为默认监听端口）

## 设置进程守护
这里可用宝塔服务器面板进行操作。
首先在面板中点击 文件 ，到/usr/lib/systemd/system目录下。
单击新建，选择新建空白文件，文件名填 cloudreve.service 。
将下文 PATH_TO_CLOUDREVE 更换为程序所在目录，复制到新建的文件中并保存。
```
[Unit]
Description=Cloudreve
Documentation=https://docs.cloudreve.org
After=network.target
Wants=network.target

[Service]
WorkingDirectory=/PATH_TO_CLOUDREVE
ExecStart=/PATH_TO_CLOUDREVE/cloudreve
Restart=on-abnormal
RestartSec=5s
KillMode=mixed

StandardOutput=null
StandardError=syslog

[Install]
WantedBy=multi-user.target
```
然后在服务器中先后执行下面的命令
```
# 更新配置
systemctl daemon-reload

# 启动服务
systemctl start cloudreve

# 设置开机启动
systemctl enable cloudreve
```
## 使用域名访问：反向代理
在宝塔服务器面板中，选择网站，点击添加站点，填写域名，点击提交。（域名最好事先解析到服务器上）
![](https://zt0729-picture-bed.oss-cn-beijing.aliyuncs.com/uTools_1642542610153.png)
然后选择设置——反向代理——添加反向代理，根据以下示例进行填写并提交（注：目标url中的端口号要根据cloudreve在服务器中实际占用的端口填写）。
![](https://zt0729-picture-bed.oss-cn-beijing.aliyuncs.com/2022-01-19_055612.png)
## 配置文件
首次运行后会在安装目录下生成conf.ini文件，可以调整数据库类型/使用的端口号。
在配置文件 后面添加下面代码，更换服务器。
```
[Database]
; 数据库类型，目前支持 sqlite/mysql/mssql/postgres
Type = mysql
; MySQL 端口
Port = 3306
; 用户名（使用建网站时的数据库用户名）
User = root
; 密码（使用建网站时的数据库密码）
Password = root
; 数据库地址
Host = 127.0.0.1
; 数据库名称（使用建网站时的数据库名称）
Name = v3
; 数据表前缀
TablePrefix = cd
; 字符集
Charset = utf8
```
更换数据库配置后，Cloudreve 会重新初始化数据库，原有的数据将会丢失。

## 以管理员身份登录Cloudreve
首次运行时会自动生成管理员账号+密码，打开刚刚反向代理的域名，即可跳出登录界面进行登录。
登录后，单击右上角，选择管理面板，即可进入管理员界面。
之后即可添加储存策略，进行使用。