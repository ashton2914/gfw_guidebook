# Debian & Ubuntu 服务器设置 IPv6 Tunnel

本文以在 Bandwagonhost 上的 Debian 10 系统为例，介绍如何设置 IPv6 Tunnel。本文同样适用于 Ubuntu 20.04。

# **注册 HE 帐号**

首先前往 [Hurricane Electric 的网站](https://tunnelbroker.net/)，注册一个帐号。

# **创建 Tunnel**

注册后，在 `User Functions` 中，点击 `Create Regular Tunnel`，输入服务器地址，并选择合适的 Tunnel Server。选择完毕后，点击最下方 `Create Tunnel` 按钮。

# **服务器设置**

点击网页上的 `Example Configurations`选项卡，找到 `Debian / Ubuntu`，选择后会在文本框中显示配置，例如：

```bash
auto he-ipv6
iface he-ipv6 inet6 v4tunnel
        address xxxx:xxx:xxxx:xxx::x
        netmask 64
        endpoint yy.yyy.yy.y
        local zzz.zz.z.z
        ttl 255
        gateway aaaa:aaa:aaaa:aaa::a

```

复制网页上显示的配置，编辑服务器文件：

```bash
sudo vim /etc/network/interfaces

```

在文件的末尾粘贴复制的配置，保存并退出。

重启服务器，验收成果。

配置文件中 `xxxx:xxx:xxxx:xxx::x` 即为服务器分配到的 IPv6 地址，亦可通过 `ip a` 命令查看。

---