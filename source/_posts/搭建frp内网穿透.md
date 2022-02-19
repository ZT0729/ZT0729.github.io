---
title: 搭建frp内网穿透
abbrlink: 33662dc9
date: 2021-12-21 22:44:04
tags:
---
利用frp内网穿透
# frp介绍
frp 是一个开源、简洁易用、高性能的内网穿透和反向代理软件，支持 tcp, udp, http, https等协议。frp 项目官网是 https://github.com/fatedier/frp。
# 我的环境
* 处于内网的win11
* 阿里云轻量级服务器(CentOS7.6 x64 系统)

# frp服务器的搭建

## frp版本选择
ssh进服务器，运行以下命令查看操作系统和CPU版本以选择对应的frp对应版本（一般是选择 linux_amd64 版本）
```
uname -a #或 arch
```
## 下载frp程序
使用ssh进入服务器
```
#切换为以系统管理者的身份执行指令 
sudo -i
# 下载最新版 frp
wget --no-check-certificate https://github.com/fatedier/frp/releases/download/v0.38.0/frp_0.38.0_linux_amd64.tar.gz
# 解压
tar -xzvf frp_0.38.0_linux_amd64.tar.gz
# 文件夹名改成 frps，不然目录太长了不方便
mv frp_0.26.0_linux_amd64 frps
# 进入 frps 目录
cd frps
# 确保 frps 程序具有可执行权限
chmod +x frps
```
然后试着运行一下` frps help `
```
./frps --help
```
正常情况下会输出一串帮助信息，那么就说明你下载了正确架构的版本。如果提示-bash: ./frps: cannot execute binary file: Exec format error 就说明你下错版本了

## 配置frp服务器
用vi 打开配置文件（vi 的具体使用方法 google 搜索,这里推荐使用宝塔面板，在线修改）
```
vi frps.ini
```
配置文件内容解释
```
# 配置开始
[common]

# frp 服务端端口（必须，客户端和它连接的端口）
bind_port = 7000

# frp 服务端密码（可以不设置）
token = 12345678

# 认证超时时间，由于时间戳会被用于加密认证，防止报文劫持后被他人利用
# 因此服务端与客户端所在机器的时间差不能超过这个时间（秒）
# 默认为 900 秒，即 15 分钟，如果设置成 0 就不会对报文时间戳进行超时验证
authentication_timeout = 900

# 仪表盘端口，只有设置了才能使用仪表盘
dashboard_port = 7500

# 仪表盘访问的用户名密码，如果不设置，则默认 admin
dashboard_user = admin
dashboard_pwd = admin
```
启动frps
```
./frps -c frps.ini
```
提示 Start frps success，这表示服务端启动成功.
ps: 服务端的这个命令行窗口不要关，关了服务就挂了。。。。
## 防火墙开放端口
```
# 添加监听端口
sudo firewall-cmd --permanent --add-port=7000/tcp
# 添加管理后台端口
sudo firewall-cmd --permanent --add-port=7500/tcp
sudo firewall-cmd --reload
```
7000和7500两个端口分别对应frps.ini配置中的bind_port和dashboard_port
## 验证服务端是否启动成功
访问：http://服务器IP:后台管理端口” ，输入用户名和密码可以查看连接状态。
如：http://123.123.123.123:7500/，用户名和密码分别对应frps.ini文件中的dashboard_user和dashboard_pwd。
登录后界面如下：
![](https://zt0729-picture-bed.oss-cn-beijing.aliyuncs.com/pictureuTools_1640100187225.png)
如果上述步骤没有问题，则说明frp的服务端配置成功了，也就意味着内网穿透你已经成功了一半！！！
# frp客户端的搭建
## 下载frp客户端
下载frps客户端文件: https://github.com/fatedier/frp/releases
选择frp_0.38.0_windows_amd64.zip 64位文件(选最新的)
然后解压，把frpc.exe和frpc.ini拷贝至c盘，目录结构如下图所示，当然你拷贝至其他盘也是一样的，看个人喜好
![](https://zt0729-picture-bed.oss-cn-beijing.aliyuncs.com/pictureuTools_1640100855944.png)
## 配置frp客户端
修改frpc.ini文件
```
[common]
# 服务器公网地址
server_addr = xxx.xx.xxx.xx
# 端口
server_port = 7000
# frp 服务端密码（和frp服务器设置的一样）
token = 12345678

[ssh] #名字,自定义
# 类型
type = tcp
# 本地地址
local_ip = 127.0.0.1
# 本地端口(3389远程默认端口)
local_port = 3389
# 线上对外暴露端口(自定义但别和有用的端口冲突)
remote_port = 33891
```
根据不同,需求可一起开放多个端口，例：
```
[common]
server_addr = x.x.x.x
server_port = 7000
token = 12345678
[rdp]  #RDP，即Remote Desktop 远程桌面，Windows的RDP默认端口是3389，协议为TCP
type = tcp
local_ip = 127.0.0.1           
local_port = 3389
remote_port = 33891 
[smb]  #SMB，即Windows文件共享所使用的协议，默认端口号445，协议TCP，本条规则可实现远程文件访问。
type = tcp
local_ip = 127.0.0.1
local_port = 445
remote_port = 33892
```
配置完成frpc.ini后，就可以运行frpc了
*** frpc程序不能直接双击运行！ ***
使用命令提示符或Powershell进入该目录下
` cd C:\frpc `并执行` ./frpc -c frpc.ini `
或直接运行
` c:\frpc\frpc.exe -c c:\frpc\frpc.ini `
运行frpc程序，窗口中输出如下内容表示运行正常。
```
2021/12/22 00:04:18 [I] [service.go:301] [7c86f47a473d] login to server success, get run id [7c86f47a473d700b], server udp port [0]
2021/12/22 00:04:18 [I] [proxy_manager.go:144] [7c86f47a473d] proxy added: [ssh]
2021/12/22 00:04:18 [I] [control.go:180] [7c86f47a473d] [ssh] start proxy success
```
不要关闭命令行窗口，此时可以在局域网外使用相应程序访问 x.x.x.x:xxxx （IP为VPS的IP，端口为自定义的remote_port）即可访问到相应服务。
# frp内网穿透应用：远程桌面
电脑A（win11）
电脑B（win11）
利用`电脑B`控制`电脑A`,电脑A运行frp客户端，电脑B使用原生的远程桌面访问电脑B（公网ip:开放端口）。
![](https://zt0729-picture-bed.oss-cn-beijing.aliyuncs.com/pictureuTools_1640103565226.png)

# 参考：
 <u> [搭建 frp 内网穿透以访问内网 NAS（或其他内网服务）](https://ppgg.in/10144.html) </u>
 <u> [使用FRP配置内网访问（穿透）教程（超详细，简单）
](https://www.freesion.com/article/51241407030/) </u>
 <u> [Windows客户端内网穿透工具frpc安装及使用教程](http://www.codingwhy.com/view/2504.html) </u>
 <u> [frps搭建自己的内网穿透服务器](https://blog.csdn.net/fanxl10/article/details/82381176) </u>
 <u> [搭建 frp 内网穿透以访问内网 NAS（或其他内网服务）](https://zhuanlan.zhihu.com/p/55306067) </u>
 <u> [使用frp进行内网穿透](https://sspai.com/post/52523/) </u>


