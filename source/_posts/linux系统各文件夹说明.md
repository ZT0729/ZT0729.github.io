---
title: linux 系统各文件夹说明
abbrlink: eec358db
date: 2022-01-05 00:37:44
tags:
---
bin   dev  home  lib64       media  opt    proc  run   srv  tmp  var
boot  etc  lib   lost+found  mnt    patch  root  sbin  sys  usr  www

## /bin 二进制执行命令
`/bin` 目录，（bin 是 binary - 二进制的简称），包含了引导启动所需的命令或普通用户可能用到的命令（可能在引导启动后）。这些命令都是二进制文件的可执行程序，多是系统中重要的系统文件。

## /boot 目录
`/boot` 目录存放引导加载（bootstrap loader）使用的文件，如 lilo ,核心映像也经常放在这里，而不是放在根目录中。但是如果有很多核心映像，这个目录就可能变得很大，这时使用单独的文件系统会更好一些。还有一点要注意的是，要确保核心映像必须在   IDE硬盘的1024柱面内。

## /dev 设备特殊文件（文件系统）
`/dev` 目录包括所有设备的设备文件，即设备驱动程序，用户通过这些文件访问外部设备。设备文件用特定的约定命名，这在设备列表中有说明。设备文件在安装时由系统产生，以后可以用 `/dev/makedev` 描述。

### /dev/makedev.local 
`/dev/makedev.local` 是系统管理员为本地设备文件（或连接）写的描述文稿（即如一些非标准设备驱动不是标准 makedev 的一部分）。

### /dev/console 
`/dev/console` 系统控制台，也就是直接和系统连接的监视器（电脑屏幕）

### /dev/hd 
`/dev/hd` IDE硬盘驱动程序接口。如 /dev/hda 指的是第一个硬盘， hda1 则指的是 /dev/hda 的第一个分区。

### /dev/sd 
`/dev/sd` SCSI磁盘驱动程序接口。如有系统有SCSI硬盘，就不会访问`/dev/hd` ，而会访问`/dev/sda` 。

### /dev/fd
`/dev/fd` 软驱设备驱动程序。如：`/dev/fd0` 指系统的第一个软盘，也就是通常所说的 a盘 ，`/dev/fd1` 指第二个软盘。

### /dev/st
`/dev/st` SCSI 磁带驱动器驱动程序。

### /dev/tty
`/dev/tty` 提供虚拟控制台支持。如：`/dev/tty1` 指的是系统的第一个虚拟控制台。

### /dev/ttys
`/dev/ttys` 计算机串行接口，对于dos来说就是 `com1 ` 口。

### /dev/cua
`/dev/cua` 计算机串行接口，与调制解调器仪器使用的设备。

### /dev/null
`/dev/null` 称为“ 黑洞 ” ，所有写入改设备的信息都将消失。例如：当下能将屏幕的输出信息隐藏起来时，只要将输出信息输入到 `/dev/null` 中即可。

## /etc 系统管理和配置文件
`/etc` 目录存放着各种系统配置文件，其中包括了用户信息文件 `/etc/passwd` ，系统初始文件 `/etc/rc` 等。 linux 正是有了这些文件才得以正常运行。

### /etc/rc 或 /etc/rc.d 或 /etc/rc?.d
`/etc/rc 或 /etc/rc.d 或 /etc/rc?.d` 启动，或改变运行级时运行的脚本或脚本的目录。

### /etc/passwd
`/etc/passwd` 用户数据库，其中的域给出了用户名，真实姓名，用户起始目录，加密口令和用户的其他信息。

### /etc/fdprm
`/etc/fdprm` 软盘参数表，用以说明不同的软盘个事。可用 `setfdprm` 进行设置。

### /etc/fastab
`/etc/fastab` 指定启动时需要自动安装的文件系统列表。也包括用 `swapon -a` 启用的 `swap区` 的信息。

### /etc/group
`/etc/group` 说明的是组的信息，包括组的各种数据。类似 `/etc/passwd` 。

### /etc/inittab
`/etc/inittab` `init` 的配置文件。

### /etc/issue
`/etc/issue` 包括用户在登录提示符前的输出信息。通常包括系统的一段说明或欢迎信息。具体内容由系统管理员确认。

### /etc/magic
`/etc/magic` `file` 的配置文件。包含不同文件格式的说明，`file` 基于它猜测文件类型。

### /etc/motd
`motd` 是 `message of the day` 的缩写，用户成功登录后自动输出。内容由系统管理员确定。常用于通告信息，如计划关机时间的警告等。

### /etc/mtab
`/etc/mtab` 当前安装的文件系统列表。由脚本（scritp）初始化，并由 `mount` 命令自动更新。当需要一个当前安装的文件系统的列表时使用（例如 `df` 命令）

### /etc/shadow
`/etc/shadow` 在安装了影子（ shadow ）口令软件的系统上的影子口令文件。影子口令文件将 `/etc/passwd` 文件中的加密口令移动到 `/etc/shadow` 中，而后者只对超级用户（root）可读。这使破译口令更困难，以此增加系统的安全性。

### /etc/login.defs
`/etc/login.defs/` logfin 命令的配置文件。

### /etc/termcap
`/etc/termacap` 终端性能数据库。说明不同的终端用什么 “转义序列” 控制。写程序时不直接输出转义序列（这样只能工作于特定评判的终端），而是从 `/etc/termcap` 中查找要做的工作的正常序列。这样多住的程序可以在多数终端上运行。

### /etc/printcap
`/etc/printcap` 针对打印机，类似 `/etc/termcap` ，但语法不同。

### /etc/profile 、/etc/csh.login 、/etc/csh.cshrc
`/etc/profile 、/etc/csh.login 、/etc/csh.cshrc` 登录或启动时 `bourne` 或 `c shells` 执行的文件。这允许系统管理员为所有用户建立全局缺省环境。

### /etc/securetty
`确认安全终端` 确认安全终端，即哪个终端允许超级用户（ root ）登录。一般只列出虚拟控制台，这样就不可能（至少很困难）通过调制解调器（ modem ）或网络闯入系统并得到超级用户特权。

### /etc/shells
`/etc/shells`列出可以使用的shell。chsh命令允许用户在本文件指定范围内改变登录的shell。提供一台机器 ftp 服务的服务进程 ftpd 检查用户 shell 是否列在 `/etc/shells` 文件中，如果不是，将不允许改用户登录。