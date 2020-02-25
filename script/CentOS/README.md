# 基于CentOS6&7的Shadowsocks安装过程
最后更新于2020年2月25日。推荐系统版本CentOS7。shadowsocks-libev脚本作者：秋水逸冰。SPAM封禁脚本作者：doubi。bbrplus作者：cx9208，原项目地址：https://github.com/cx9208/bbrplus
## 1.安装wget

```bash
yum install wget
```

## 2.BBR-Plus安装指令

```bash
wget "https://raw.githubusercontent.com/Zhicheng2914/-/master/script/CentOS/bbrplus/ok_bbrplus_centos.sh" && chmod +x ok_bbrplus_centos.sh && ./ok_bbrplus_centos.sh
```

## 3.shadowsocks-libev安装指令

```bash
wget --no-check-certificate -O shadowsocks-libev.sh https://raw.githubusercontent.com/Zhicheng2914/-/master/script/CentOS/shadowsocks-libev.sh && chmod +x shadowsocks-libev.sh && ./shadowsocks-libev.sh 2>&1 | tee shadowsocks-libev.log
```

## 4.封禁SPAM端口（防止服务器被封）

```bash
wget -N --no-check-certificate https://raw.githubusercontent.com/Zhicheng2914/-/master/script/CentOS/ban_iptables.sh && chmod +x ban_iptables.sh && bash ban_iptables.sh
```
