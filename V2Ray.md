# V2Ray

# 综述

V2Ray(简称V2)，是Victoria Raymond开发的 Project V 下的一个工具。Project V 是一个工具集合，号称可以帮助其使用者打造专属的基础通信网络。Project V 的核心工具称为V2Ray，其主要负责网络协议和功能的实现，与其它 Project V 通信。V2Ray 可以单独运行，也可以和其它工具配合，以提供简便的操作流程。开发过程主要使用Go语言，Core采用MIT许可协议授权并开放源代码。

# **运行原理**

V2Ray的运行原理与其他代理工具基本相同，使用特定的中转服务器完成数据传输。例如，用户无法直接访问Google，YouTube等网站，但代理服务器可以访问，且用户可以直接连接代理服务器，那么用户就可以通过特定软件连接代理服务器，然后由代理服务器获取网站内容并回传给用户，从而实现代理上网的效果。服务器和客户端软件会要求提供密码和加密方式，双方一致后才能成功连接。连接到服务器后，客户端会在本机构建一个本地Socks5代理（或VPN、透明代理等）。浏览网络时，客户端通过这个Socks5（或其他形式）代理收集网络流量，然后再经混淆加密发送到服务器端，以防网络流量被识别和拦截，反之亦然。其他代理工具定位只是一个简单的代理工具，而 V2Ray 定位为一个平台，任何开发者都可以利用 V2Ray 提供的模块开发出新的代理软件。

# **主要特性**

多入口多出口：一个 V2Ray 进程可并发支持多个入站和出站协议，每个协议可独立工作。

定制化路由：入站流量可按配置由不同地出口发出。轻松实现按区域或按域名分流，以达到最优的网络性能。

多协议支持：V2Ray 可同时开启多个协议支持，包括 Socks、HTTP、Shadowsocks 和 VMess 等。每个协议可单独设置传输载体，比如 TCP、mKCP 和 WebSocket 等。

隐蔽性：V2Ray 的节点可以伪装成正常的网站（HTTPS），将其流量与正常的网页流量混淆，以避开第三方干扰。

反向代理：通用的反向代理支持，可实现内网穿透功能。

多平台支持：原生支持所有常见平台，如 Windows、macOS 和 Linux，并已有第三方支持移动平台。