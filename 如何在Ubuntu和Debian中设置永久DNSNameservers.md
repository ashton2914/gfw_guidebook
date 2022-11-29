# 如何在Ubuntu和Debian中设置永久DNSNameservers

/**etc/resolv.conf**是**DNS**名称解析器库的主要配置文件。 解析程序是C库中的一组函数，提供对**Internet域名系统** （ **DNS** ）的访问。 这些功能被配置为检查**/ etc / hosts**文件或多个DNSNameservers中的条目，或者使用主机的**网络信息服务** （ **NIS** ）数据库。

在使用**systemd** （系统和服务管理器）的现代Linux系统上， **DNS**或**名称解析**服务通过**systemd解析的**服务提供给本地应用程序。 默认情况下，此服务具有四种用于处理域名解析的模式，并在默认操作模式下使用systemd DNS存根文件（ **/run/systemd/resolve/stub-resolv.conf** ）。

DNS存根文件包含本地**存根127.0.0.53**作为唯一的DNS服务器，并且重定向到**/etc/resolv.conf**文件，该文件用于添加系统使用的Nameservers。

如果在**/etc/resolv.conf**上运行以下[ls命令](https://www.howtoing.com/15-basic-ls-command-examples-in-linux/) ，您将看到该文件是**/run/systemd/resolve/stub-resolv.conf**文件的符号链接。

```bash
$ ls -l /etc/resolv.conf

lrwxrwxrwx 1 root root 39 Feb 15  2019 /etc/resolv.conf -> ../run/systemd/resolve/stub-resolv.conf
```

不幸的是，由于**/etc/resolv.conf**是由**systemd解析的**服务间接管理的，在某些情况下是由网络服务（通过使用**initscripts**或**NetworkManager** ）间接管理的，因此，用户手动进行的任何更改都无法永久保存或仅保存持续一会儿。

在本文中，我们将展示如何安装和使用**resolvconf**程序在**Debian**和**Ubuntu** Linux发行**版的** **/etc/resolv.conf**文件中设置永久DNSNameservers。

# 为什么要Ddit /etc/resolv.conf文件？

主要原因可能是因为系统**DNS**设置配置错误，或者您更喜欢使用特定的Nameservers或您自己的Nameservers。 以下[cat命令](https://www.howtoing.com/13-basic-cat-command-examples-in-linux/)在我的Ubuntu系统上的**/etc/resolv.conf**文件中显示默认Nameservers。

```bash
$ cat /etc/resolv.conf
```

![https://www.howtoing.com/wp-content/uploads/2019/10/Check-DNS-Name-Servers.png](https://www.howtoing.com/wp-content/uploads/2019/10/Check-DNS-Name-Servers.png)

检查DNSNameservers

在这种情况下，当[APT程序包管理器之](https://www.howtoing.com/apt-advanced-package-command-examples-in-ubuntu/)类的本地应用程序尝试访问本地网络上的**FQDN** （ **完全合格的域名** ）时，结果是“ **名称解析临时失败** ”错误，如下图所示。

![https://www.howtoing.com/wp-content/uploads/2019/10/Temporary-failure-resolving.png](https://www.howtoing.com/wp-content/uploads/2019/10/Temporary-failure-resolving.png)

临时故障解决

当您运行[ping命令](https://www.howtoing.com/linux-ping-command-examples/)时，也会发生同样的情况。

```bash
$ ping google.com
```

![https://www.howtoing.com/wp-content/uploads/2019/10/Temporary-Failure-in-Name-Resolution.png](https://www.howtoing.com/wp-content/uploads/2019/10/Temporary-Failure-in-Name-Resolution.png)

名称解析暂时失败

因此，当用户尝试手动设置Nameservers时，更改不会持续很长时间，也不会在重新启动后被撤销。 要解决此问题，您可以安装并使用**reolvconf**实用程序使更改永久**生效** 。

要安装下一部分中所示的**resolvconf**软件包，首先需要在**/etc/resolv.conf**文件中手动设置以下Nameservers，以便在Internet上访问Ubuntu仓库服务器的FQDM。

```bash
nameserver 8.8.4.4
nameserver 8.8.8.8
```

**另请参阅** ： [如何在Linux中使用/ etc / hosts文件设置本地DNS](https://www.howtoing.com/setup-local-dns-using-etc-hosts-file-in-linux/)

# 在Ubuntu和Debian中安装resolvconf

首先，更新系统软件包，然后通过运行以下命令从官方系统信息库安装**resolvconf** 。

```bash
$ sudo apt update
$ sudo apt install resolvconf
```

一旦**resolvconf**安装完成， **systemd**将触发**resolvconf.service**自动启动和启用。 要检查它是否已启动并正在运行，请发出以下命令。

```bash
$ sudo systemctl status resolvconf.service
```

如果由于某种原因未启动并自动启用该服务，则可以按以下步骤启动并启用它。

```bash
$ sudo systemctl start resolvconf.service
$ sudo systemctl enable resolvconf.service
$ sudo systemctl status resolvconf.service
```

![https://www.howtoing.com/wp-content/uploads/2019/10/start-enable-and-view-resolvconf-service-status.png](https://www.howtoing.com/wp-content/uploads/2019/10/start-enable-and-view-resolvconf-service-status.png)

检查Resolvconf服务状态

### 在Ubuntu和Debian中设置永久DNSNameservers

接下来，打开**/etc/resolvconf/resolv.conf.d/head**配置文件。

```bash
$ sudo nano /etc/resolvconf/resolv.conf.d/head
```

并在其中添加以下几行：

```bash
nameserver 8.8.8.8 
nameserver 8.8.4.4
```

![https://www.howtoing.com/wp-content/uploads/2019/10/set-name-servers-in-resolvconf-head-config-file.png](https://www.howtoing.com/wp-content/uploads/2019/10/set-name-servers-in-resolvconf-head-config-file.png)

在Resolvconf中设置永久DNSNameservers

保存更改，然后重新启动**resolvconf.service**或重新引导系统。

```bash
$ sudo systemctl start resolvconf.service
```

现在，当您检查**/etc/resolv.conf**文件时，Nameservers条目应永久存储在此处。 从今以后，您将不会在系统上遇到任何有关名称解析的问题。

![https://www.howtoing.com/wp-content/uploads/2019/10/permanent-name-servers-set.png](https://www.howtoing.com/wp-content/uploads/2019/10/permanent-name-servers-set.png)

永久DNSNameservers