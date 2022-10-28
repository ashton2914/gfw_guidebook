---
template: OBJECTIVE/Knowledge-Strucuture_v1.0
created: 2022-03-28 19:36
updated: 2022-03-30 14:50
---
# OpenWrt 安装配置 Samba 实现网络文件共享
#OBJECTIVE/Knowledge 
Created:: 2022-03-28 19:36
Category:: #Computer/Software/Samba 
Platform:: #Openwrt 
Tags:: #Network #Share #Openwrt 

---

OpenWrt 局域网文件共享实现主要有两个软件选择，[Samba](https://openwrt.org/docs/guide-user/services/nas/cifs.server) 和 [NFS](https://openwrt.org/docs/guide-user/services/nas/nfs.server)。Samba 系统兼容性较好，NFS 则性能表现占优。对于新手或者需要使用 Windows 设备的用户来说，建议选择 Samba。

以下是 Samba 具体安装配置步骤，其中涉及到命令运行的操作需要[登录 SSH](https://openwrt.org/docs/guide-quick-start/sshadministration)。

## **安装 USB 设备**

根据[官方帮助文档](https://openwrt.org/docs/guide-user/storage/usb-drives)操作即可，很详细。

## **安装 Samba**

安装 `luci-app-samba`，它会同时安装 `samba-server` 核心程序。

```bash
# 更新软件索引
opkg update

# 安装 Samba3 版本
opkg install luci-app-samba luci-i18n-samba-zh-cn

# 安装 Samba4 版本
opkg install luci-app-samba4 luci-i18n-samba4-zh-cn
```

Samba3 是陈旧版本，虽然 OpenWrt 有提供安全更新，但支持力度有限。在硬件资源允许情况下（至少 128 M 内存及 64 M可用存储空间），优先使用 Samba4（更安全，支持高版本 SMB 协议）。两者更多比较可参考[这个回答](https://forum.openwrt.org/t/53303/3)。

本文教程配置环境为 Samba3，Samba4 未做测试，估计也差不多。

## **创建系统用户**

为了能以用户验证方式共享文件，需要创建一个系统用户（Samba 用户必须同时以系统用户身份存在，因为拥有共享和用于验证的是系统用户）。

```bash
# 安装用户工具
opkg install shadow-groupmod shadow-useradd

# 创建用户并设置密码
useradd -m smbusers && passwd smbusers

# 修改用户所属组名称（创建用户同时会创建同名用户组。Windows 使用环境不允许用户和组使用相同名称）
groupmod -n smbgroups smbusers
```

## **创建 Samba 用户**

这是访问共享文件时要求登录的 Samba 帐号，用户名和之前创建的系统用户一样（密码可不同）。

```bash
# 创建用户并设置密码
smbpasswd -a smbusers

# 修改用户密码
smbpasswd smbusers
```

更多 `smbpasswd` 参数用法[见此文档](https://www.samba.org/samba/docs/current/man-html/smbpasswd.8.html)。

## **Samba 参数配置**

登录 LuCI 控制台，在“服务”下拉菜单点击“网络共享”，点击“编辑模板”可以添加自定义参数设置。

例如笔者在默认规则基础上加了下面几个参数，分别为仅允许 192.168.1.0/24 主机连接，加密认证密码，不要加载打印机，设置日志文件（`%m` 表示客户端主机名），日志文件最大 50 KB，设置 Samba 用户名映射（可防止登录用户知道存在同名系统用户）。

```bash
# 自定义添加参数
hosts allow = 192.168.1.
username map = /etc/samba/usernamemap
encrypt passwords = yes
load printers = no
log file = /var/log/samba/log.%m
max log size = 50

# 创建 Samba 用户名映射文件
vi /etc/samba/usernamemap
# 内容如下。例如将 smbusers 映射到 admin，之后可用 admin 用户名登录
smbusers = admin
```

更多自定义参数可查看[此文档](https://www.samba.org/samba/docs/current/man-html/smb.conf.5.html)。

## **添加共享目录**

填写共享目录名称/路径，用户/权限等内容。如下图。

![https://www.hostarr.com/wp-content/uploads/samba-on-openwrt-1.png](https://www.hostarr.com/wp-content/uploads/samba-on-openwrt-1.png)

完成设置后点击“保存并应用”，转到“系统”下拉菜单“启动项”里重启 samba 服务生效。

将共享目录所有者改为之前创建的系统用户/用户组，不然无法读取写入数据。

```bash
chown smbusers:smbgroups /mnt/sda1
```

## **Samba 客户端连接**

如果你使用 Windows 10 系统，默认无法创建连接，因为禁用了 SMBv1 过时协议。需要在“启用或关闭 Windows 功能”里选中“SMB 1.0/CIFS File Sharing Support”下的“SMB 1.0/CIFS Client”，添加后重启系统生效。

然后打开系统“凭据管理器”界面，点击添加“添加 Windows 凭据”，填写 NETBIOS 地址（默认 OpenWrt），Samba 用户名密码。

设置后就可以在“文件管理器”里右键添加网络位置，或者点击左侧“网络”进入设备共享目录。别忘了需要开启“网络发现”和“文件共享”。

其它设备，iOS 或 Android 建立连接就简单了，一操作便明就不赘述了。

本来想使用 SMBv2 协议创建连接，理论上 Samba 3.6 最高支持 SMBv2。但在[确认 SMBv2 启用](https://docs.microsoft.com/en-us/windows-server/storage/file-server/troubleshoot/detect-enable-and-disable-smbv1-v2-v3)的情况下， Windows 10 怎么弄都不成功，可能与系统 SMBv2 版本过高有关系。Windows 7 支持 SMB2_10 版本，这与 Samba 3.6 最高支持版本相符，或许 Windows 7 下可以使用 SMBv2 连接。