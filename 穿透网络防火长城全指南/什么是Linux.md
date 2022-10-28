---
template: OBJECTIVE/Knowledge-Strucuture_v1.0
created: 2022-03-28 19:34
updated: 2022-03-30 14:49
---
# 什么是Linux
#OBJECTIVE/Knowledge 
Created::2022-03-28 19:34
Category:: #Computer/OS/Linux 
Tags:: 

---

# 什么是Linux

Linux® 是一个[开源](https://www.redhat.com/zh/open-source)操作系统（OS）和 IT 基础架构平台。它由 Linus Torvalds 于 1991 年构思设计而成，最初这只是他的一项兴趣爱好。当时还在大学就读的 Linus 想要基于 Unix 的原则和设计来创建一个免费的开源系统，从而代替 MINIX 操作系统。自此，这项兴趣爱好便逐步演变成了拥有最大用户群的操作系统。如今，它不仅是[公共互联网服务器](https://en.wikipedia.org/wiki/Usage_share_of_operating_systems#Public_servers_on_the_Internet)上最常用的操作系统，还是[速度排名前 500 的超级电脑](http://www.zdnet.com/article/linux-totally-dominates-supercomputers/)上使用的唯一一款操作系统。

Linux 最大的优势当属它的开源属性。Linux 是一款基于 [GNU 通用公共许可证（GPL）](https://www.gnu.org/licenses/licenses.html)发布的操作系统。这意味着，所有人都能运行、研究、分享和修改这个软件。经过修改后的代码还能[重新分发，甚至出售](https://en.wikipedia.org/wiki/GNU_General_Public_License#Terms_and_conditions)，但必须基于同一个许可证。这一点与传统操作系统（如 Unix 和 Windows）截然不同，因为传统操作系统都是锁定供应商、以原样交付且无法修改的专有系统。

注：在谈到 Linux 时，人们对其含义的理解常有不同。特此声明，我们这里所谈的，是 Linux 内核及其捆绑的工具、应用和服务。这些要素共同构成了这个功能强大的操作系统，大多数人称之为“Linux”。[自由软件基金会](https://www.fsf.org/)则将其称为“[GNU/Linux](https://www.gnu.org/gnu/linux-and-gnu.en.html)”，因为其中的部分工具、应用和服务是 GNU 系统的组件。这些组件已与 Linux 内核捆绑，所以我们所熟知的 Linux 所指的不仅仅是 Linux 内核本身。

# Linux 能做些什么？

Linux 能为几乎所有类型的 IT 计划奠定基础，包括容器、[云原生应用](https://www.redhat.com/zh/topics/cloud-native-apps)和安全防护。许多全球规模最大的行业和企业都仰赖于 Linux——从 [Wikipedia](https://meta.wikimedia.org/wiki/Wikimedia_servers) 等知识共享网站，到[纽约证券交易所](https://www.redhat.com/zh/about/press-releases/nyse-0)，再到运行 [Android](https://www.howtogeek.com/189036/android-is-based-on-linux-but-what-does-that-mean/)（一个包含免费软件的 Linux 内核专用发行版） 的移动设备，它无处不在。经过多年的发展，Linux 已成为了行业广泛接受的标准，成为在数据中心和云部署中运行高可用性、高可靠性和关键工作负载的首选。它有适合不同目标系统和设备的多种用例和发行版本，而且功能丰富 ，完全可根据您的需求和工作负载进行调整。

微软也以其他方式引入了 Linux 和开源技术，其中包括：推出 [Linux 版 SQL Server](https://www.microsoft.com/en-us/sql-server/sql-server-2017)；将 .NET 框架 ([.NET Core/Mono](http://www.mono-project.com/)) 打造成一种开源技术，以便在任意平台上运行；允许使用 Linux 的开发人员利用这个框架来开发应用。到 2027 年，所有 SAP 客户都会[改用 SAP HANA](https://www.redhat.com/zh/solutions/digital-transformation/sap)，这是一个只能在 Linux 上运行的内存中关系数据库管理系统。但截至 2017 年，[SAP 市场上尚有 50% 的客户](https://www.ibm.com/blogs/cloud-computing/2017/07/migrating-to-s4hana-questions/)使用的是 Windows 产品。

而在云技术方面，即使是在 Microsoft 的 Azure 上，也有 的 Azure Marketplace 镜像和近三分之一的虚拟机是基于 Linux 的。与此同时，[Amazon Web Services](https://aws.amazon.com/mp/linux/) 和 [Google Cloud Platform](https://cloud.google.com/compute/docs/images#os-compute-support) 还以公用镜像的形式推出了多个 Linux 发行版。

未来，Linux 仍会是热门的操作系统之一，并且更会有越来越多的系统因其稳定性和可扩展性择木而栖。

# Linux 值得信赖吗？它安全吗？

安全从来都不是唾手可得的。所有的企业和部署策略都[必须充分考虑安全性](https://www.redhat.com/zh/technologies/guide/it-security)。

## 分层部署是确保安全性的最佳方式

安全性不是区区一项功能，而是一个全局问题。就 [IT 安全性](https://www.redhat.com/zh/topics/security)而言，操作系统扮演着非常重要的角色，因为它不仅涉及物理硬件、访问硬件的人员权限，还有硬件上所部署的应用。在更广义的概念上，安全性还涉及风险管理、合规性和监管。仅保证一个方面的安全并不代表一切都能安全无虞，您必须全面考虑。

由于 Linux 采用的是模块化架构，所以其安全性更易管理。用户可以单独审核、监控和保护 Linux 操作系统的每一个构件。Linux 包含各种内置工具和模块（如 [SELinux](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/selinux_users_and_administrators_guide/index)），有助于进一步锁定、监控、报告和修复安全问题。Linux 还能将用户空间与内核空间分隔开，这意味着用户不一定能访问整个系统中运行的所有进程（取决于角色特权），同样，系统也不能干涉用户进程。而这正是容器、虚拟化等技术的核心概念和实现要素，因为它们都需要部署不同、独立而安全的工作负载和权限。

当然，百分百安全的操作系统并不存在。但是，[您可以采取相应措施](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html-single/security_guide/index)并利用 Linux 所具备的优势来尽可能提高安全性。