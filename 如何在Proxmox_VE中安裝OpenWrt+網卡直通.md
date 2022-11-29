# 如何在Proxmox VE中安裝OpenWrt + 網卡直通

[00:00](https://www.youtube.com/watch?v=3G3kutSRSrs&list=PLcYK2_vDtcAN7Ezk9XFUqzwVDLAu1TVov&index=20&t=0s) Intro
[00:22](https://www.youtube.com/watch?v=3G3kutSRSrs&list=PLcYK2_vDtcAN7Ezk9XFUqzwVDLAu1TVov&index=20&t=22s) 下載Proxmox VE 6.3 & 寫入到USB隨身碟
[00:39](https://www.youtube.com/watch?v=3G3kutSRSrs&list=PLcYK2_vDtcAN7Ezk9XFUqzwVDLAu1TVov&index=20&t=39s) 安裝Proxmox VE 6.3 
[01:45](https://www.youtube.com/watch?v=3G3kutSRSrs&list=PLcYK2_vDtcAN7Ezk9XFUqzwVDLAu1TVov&index=20&t=105s) SSH鏈接Proxmox VE & 編輯sources.list 
[02:12](https://www.youtube.com/watch?v=3G3kutSRSrs&list=PLcYK2_vDtcAN7Ezk9XFUqzwVDLAu1TVov&index=20&t=132s) 編輯Grub & 啓用iommu &添加直通模塊 
[02:45](https://www.youtube.com/watch?v=3G3kutSRSrs&list=PLcYK2_vDtcAN7Ezk9XFUqzwVDLAu1TVov&index=20&t=165s) 新建OpenWrt虛擬機 
[04:28](https://www.youtube.com/watch?v=3G3kutSRSrs&list=PLcYK2_vDtcAN7Ezk9XFUqzwVDLAu1TVov&index=20&t=268s) 把 OpenWrt.img 轉換成虛擬磁碟 
[05:01](https://www.youtube.com/watch?v=3G3kutSRSrs&list=PLcYK2_vDtcAN7Ezk9XFUqzwVDLAu1TVov&index=20&t=301s) 直通網卡 
[05:32](https://www.youtube.com/watch?v=3G3kutSRSrs&list=PLcYK2_vDtcAN7Ezk9XFUqzwVDLAu1TVov&index=20&t=332s) 啓動虛擬機 & 配置 
[07:46](https://www.youtube.com/watch?v=3G3kutSRSrs&list=PLcYK2_vDtcAN7Ezk9XFUqzwVDLAu1TVov&index=20&t=466s) 更新源 
[07:58](https://www.youtube.com/watch?v=3G3kutSRSrs&list=PLcYK2_vDtcAN7Ezk9XFUqzwVDLAu1TVov&index=20&t=478s) Linux Bridge 
[08:58](https://www.youtube.com/watch?v=3G3kutSRSrs&list=PLcYK2_vDtcAN7Ezk9XFUqzwVDLAu1TVov&index=20&t=538s) Thanks for watching ! 
Proxmox Virtual Environment，通常簡稱為PVE、Proxmox，是一個開源的伺服器虛擬化環境Linux發行版。Proxmox VE基於Debian，使用基於Ubuntu的客製化核心，包含安裝程式、網頁控制台和命令行工具，並且向第三方工具提供了REST API，在Affero通用公眾授權條款第三版下發行。Proxmox VE支援兩類虛擬化技術：基於容器的LXC（自4.0版開始，3.4版及以前使用OpenVZ技術）和硬體抽象層全虛擬化的KVM 
=== 準備工作 === 
【1】 Proxmox VE 6.3 官網下載地址 [https://www.proxmox.com/en/downloads/...](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbllwaHpnSDRsaWprVXQxREd5R3ZJVTNpbnhOQXxBQ3Jtc0ttTGp0RkpaanoyNFZWSnRRRTg4N1QxY3B1djY2b3JfdFhWaXUxWWFPeGxLZmk1RGlCRFkzYUFMOTdRTE5FMklUbDZudzdJMzFXRzNPOXN5aWcxS20tWHBlYXc5ajU4YWlYX1Vra2IxQ2wxWWs5VmRSMA&q=https%3A%2F%2Fwww.proxmox.com%2Fen%2Fdownloads%2Fcategory%2Fiso-images-pve) 
【2】 最新精簡版ssrpOpenWrt固件下載地址 [https://t.me/ssrpOpenWRT](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbEZTY24zUXlNNDM3SGlPOWVPT1dUUjFNTWw4d3xBQ3Jtc0ttSWhfUER1V3YweEFxdnVhdmYxS1hTUGNmODZMTEVsbDYxTXc5NXJNaEloU1lVUzREWGh1ZG9OeEkxSnEwcktXS0xqZnZ0cHNsQlRhQm1HSFlvNGthS0p2dEVuSjFKRjFKSE9IM3RUcEZBckotRlBOZw&q=https%3A%2F%2Ft.me%2FssrpOpenWRT) 
【3】 BalenaEther 官網下載地址 [https://www.balena.io/etcher/](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbjNJTHVJa3V0WkdNbVF5TWZrdWc0NFU0UWEtZ3xBQ3Jtc0tsZ0RyYW16RGdqUzhUT0QzRkw5RjRWRU9fS1cyWTVmSTFNY3NPM0MzLWZpZ2ZKUHp6SGdMLV9mNHoyZ0UzZ01kQktfOEVadTVCcTViZ21wZWp0V2gwRlQ1b0hJd3Y4Z3VDLVQ5dlZ4ZDJYeFFHQXQtNA&q=https%3A%2F%2Fwww.balena.io%2Fetcher%2F) 
【4】 Xshell [https://www.netsarang.com/zh/free-for...](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbExDdTBWNWFXblBnbWZXQ2NrS0p3azRwVHJiQXxBQ3Jtc0tuam1meGdRZkM2STh2TndvTnNqXzlpMjZkTmMtTFdHcFZnWTlYQkRXdExpV25zakJDRnlBajlnWGtPdGVCMmVLZ2g4VTlDeTE5eXk1WGt6ME1sblpuSjdVSFlzN1JlSXItQlZRYkFxNkRyN0I4LWhTOA&q=https%3A%2F%2Fwww.netsarang.com%2Fzh%2Ffree-for-home-school%2F) 

=== 安裝步驟 ==== 
【1】使用 BalenaEther 把 Proxmox-ve_6.3-1.iso 鏡像文件寫入空白的USB隨身碟。 
【2】在軟路由的BIOS中，開啓英特爾虛擬化技術 Intel (VMX) Virtualization Technology，如果是AMD處理器開啓SVM 
【3】開機從USB隨身碟啓動，按照向導安裝 ProxmoxVE 
【4】安裝完成之後，用Xshell或者Putty，SSH連接到Proxmox，IP地址就是你設定的，端口是22。 
【5】編輯 sources.list，輸入以下命令 nano /etc/apt/sources.list 打開後在最下面添加 deb [http://download.proxmox.com/debian](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbTY4U1oyOUlGbzB6N0VYSkdEaEw1SVpOaU9Ld3xBQ3Jtc0tuWjNwNkpadnVlWGRkT25rUTJnMUxkZm1yR1VSVmZvSzhQR3p3TkRDbldMUTBvT2hlaDlzMVMxdUFoZDZ6ZWtzNGxRaEg3cnlEXzNVemlqN2lxTUtfZmYtWDJJNk4wQ09USnZtRHVOQXA1eFN0bGhwZw&q=http%3A%2F%2Fdownload.proxmox.com%2Fdebian) buster pve-no-subscription 接著按 Ctrl+O 保存，按Ctrl+X退出 
【6】編輯下pve-enterprise.list，輸入以下命令 nano /etc/apt/sources.list.d/pve-enterprise.list 在 deb [https://enterprise.proxmox.com/debian...](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbnlsVTZDcTk5QUI0d3hkVDF2bWtxcG1TUVZEZ3xBQ3Jtc0tsV1dNc25hZmVOekdGYnRaSmRUbW56STVxQUFCc3ZYMm5HMEp1Yl9Rb0E0ZG50VHg2SkJfTjNNZVF0X19yZ3h6V29PaXBqMmZmZzdHVktCaEFaNDlSQ0RIeWdoeERPc04ycmJ4aG5TNVFDSVVGS1J6OA&q=https%3A%2F%2Fenterprise.proxmox.com%2Fdebian%2Fpve) buster pve-enterprise 前面加入 # 注釋掉 就像這樣 # deb [https://enterprise.proxmox.com/debian...](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbUxUTkhBUzJGcFA1aFFMbjVKeFIwaUR1am4xZ3xBQ3Jtc0trb3RUY0pZZy0wM05uN2JFWmhndWVjemNESFBJN1RpbjlUcEpocUxZNWg0NmZ6VlR6b3FKMEM2YVp6bEFrdjdJdEN0dFBXRFhQOFYtV0RSQkNjNWlMMmptSmZ0bkpFbXlEckRHWUprZk1oalZONWFoVQ&q=https%3A%2F%2Fenterprise.proxmox.com%2Fdebian%2Fpve) buster pve-enterprise 然後保存退出 兩個都編輯好了之後 我們先不更新 等聯網了再更新，（如果你現在已經聯網了，可以更新） 
【7】接著編輯下grub nano /etc/default/grub 在 GRUB_CMDLINE_LINUX_DEFAULT="quiet" 的quiet後面加入 intel_iommu=on 就像這樣 GRUB_CMDLINE_LINUX_DEFAULT="quiet intel_iommu=on" 如果你是AMD處理器，把 intel 改爲 amd 即可 編輯完了之後 保存退出 然後更新下grub update-grub 
【8】編輯模塊 nano /etc/modules 在裏面加入這四行 vfio vfio_iommu_type1 vfio_pci vfio_virqfd 保存退出 接著重啓 reboot
【9】瀏覽器輸入 [https://你設定的IP:8006](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqazhidEl4Nk1UQlR3b0t2ZUo1ajNHdWFRM2c4QXxBQ3Jtc0ttNkJ0WjV1bW5pcEdPYVZzYzY4WUxKTV8wX3FGUW9SVXBEWWJ5RXlJaktwbE9EZHQ2enVJOWllZDBTbnpGbjFxQ2NmNkQ3VjNPeGU0OTJEYWJZWWxoSFZOZmFHZU55Z2lWMzJDWFZXZkNyaDVCU214Yw&q=https%3A%2F%2F%E4%BD%A0%E8%A8%AD%E5%AE%9A%E7%9A%84IP%3A8006) 進入proxmox VE管理界面、
【10】把下載好的OpenWrt固件解壓開來，解壓非EFI版本，并且重命名為OpenWrt.img 接著把OpenWrt.img 上傳進PVE - 儲存Local - ISO Images 
【11】創建一臺虛擬機，創建完之後 刪掉創建的1GB虛擬磁碟 接著把上傳的OpenWrt.img轉換成虛擬磁碟 qm importdisk 100 /var/lib/vz/template/iso/OpenWrt.img local-lvm 轉換好了之後 添加進虛擬機裏 修改啓動順序 從新增的磁碟啓動
【12】啓動虛擬機 並配置好OpenWrt 
【13】聯網之後更新之前沒更新的 apt-get update apt dist-upgrade 不過貌似用 pveupgrade 代替 apt dist-upgrade 會好一些