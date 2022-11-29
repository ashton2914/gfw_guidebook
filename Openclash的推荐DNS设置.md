# Openclash的推荐DNS设置

# Openclash的推荐DNS设置

- 本地DNS劫持: 此项默认启动，因为Clash内核自带DNS服务器，需要进行本地DNS劫持以实现各项功能。

> 请确保OpenClash是Dnsmasq的唯一上游DNS服务器，如您使用了会劫持(通过iptables)路由器53端口或者转发DNS查询的其他插件，请选择以下任意一个选项操作：
> 
1. 【停用本地DNS劫持选项时】将冲突插件的上游DNS服务器设置删除并只保留OpenClash的DNS监听地址，如:127.0.0.1:7874，再做一次转发，使DNS请求最后通过OpenClash向外部查询
2. 【启用本地DNS劫持选项时】关闭冲突插件的DNS劫持，使用OpenClash的自定义DNS功能，只启用冲突插件的监听地址，让其作为OpenClash的上游服务器

---

- 自定义上游DNS服务器：一般情况下直接使用配置文件的DNS服务器即可。

但一些机场可能会提供不包含DNS设置的托管或者您需要自定义、使用本地的其他DNS查询插件，比如：smartdns。此时您可以启用此项功能，Openclash会在每次启动时自动修改对应配置，保证订阅托管的配置文件按照您的要求在本地运行。

![https://github.com/vernesong/OpenClash/raw/master/img/set9.png](https://github.com/vernesong/OpenClash/raw/master/img/set9.png)

1. Fake-IP（增强）模式推荐DNS设置

```yaml
  nameserver:
  - 114.114.114.114
  - 119.29.29.29

```

1. Redir-Host（兼容）模式推荐DNS设置

```yaml
  nameserver:
  - 114.114.114.114
  - 119.29.29.29
  fallback:
  - tls://1.1.1.1:853
  - tcp://1.1.1.1:53
  - tcp://208.67.222.222:443
  - tls://dns.google

```

---

- IPV6类型DNS解析：默认关闭，Clash支持进行DNS的`IPV4/IPV6`双栈查询，如你拥有公网IPV6地址并在内网配置了NAT，可以启用此项。
1. 固件需要安装`ip6tables-mod-nat`,并保证内网NAT正常。
2. 在没有公网IPV6时，如你启用了IPV6的DHCP功能，此时客户端可能会使用IPV6发起DNS请求而导致DNS查询失败，导致网络连接异常。如你没有IPV6公网地址或未进行内网NAT配置，建议您关闭IPV6的DHCP功能。

---

- 高级设置：此项用于设置在Fake-IP模式下返回真实DNS结果的域名，`根据设置的DNS服务器，可能会影响对该域名的代理。`目前仅内置了常用的NTP服务器域名和网易云音乐域名，防止其他服务和插件异常。

如您在使用Fake-IP的过程中遇到某些连接异常无法访问，或者某项服务功能失效，可以尝试设置此选项，否则，请保持默认，并不要删除内置的域名。