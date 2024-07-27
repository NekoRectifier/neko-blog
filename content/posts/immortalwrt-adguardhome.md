---
date: '2024-07-27T12:36:39Z'
draft: false
title: 'Immortalwrt 安装/配置 Adguardhome'
# weight: 1
# aliases: ["/first"]
tags: ["OpenWRT", "Software"]
author: "NekoRect"
showToc: false
TocOpen: false
draft: false
hidemeta: false
comments: false
description: ""
#canonicalURL: "https://canonical.url/to/page"
disableHLJS: true # to disable highlightjs
disableShare: false
hideSummary: false
searchHidden: false
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: false
cover:
    image: "<image path/url>" # image path/url
    alt: "<alt text>" # alt text
    caption: "<text>" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: true # only hide on current single page
#editPost:
#    URL: "https://github.com/<path_to_repo>/content"
#    Text: "Suggest Changes" # edit text
#    appendFilePath: true # to append file path to Edit link
---

# 前言

在默认安装的 ImmortalWRT 中，没有像 Pandavan 一样的 Adguardhome 配置页面。取而代之的是统一化的 opkg 软件源。因此需要手动安装并且配置 ADG。该配置方案参照 openwrt 官方的 ADG 配置指南。

# 安装

使用 SSH 登录路由器，执行 `opkg update` 进行软件包信息拉取。

> 默认情况下 ImmortalWRT 已经设置了国内镜像源（Vsean），不需要手动换源。

执行完成后，执行 `opkg install adguardhome` 进行 ADG 二进制文件的安装。二进制文件会被安装在 `/sr/bin/adguardhome`。

# 启动

默认情况下，通过 `service` 命令控制 ADG 服务。可以仅执行 `service adguardhome` 查看可用执行。

```bash
service adguardhome enable # 激活（自启动）
service adguardhome start # 启动
service adguardhome status # 查看当前服务情况
```

在执行激活／启动命令后，`status` 命令应返回 `running`。如果检查发现 `adguardhome` 实际并未运行，则参考以下部分。

使用 nano 编辑 `/etc/config/adguardhome` 文件，更改 enabled 项的值为 1。具体参考以下内容：

```plain
config adguardhome config
        option enabled '1'
        # Where to store persistent data by AdGuard Home
        option workdir /etc/ADG
```

> 此处也推荐将 `workdir` 的默认值修改为其他路径，避免路由器断电重启后 ADG 数据丢失的问题。

在更改完该配置文件后，再次使用 `service` 启用并启动 ADG。此时二进制文件应该正常启动并运行。

# 配置

## ADG 配置文件

检查 `/etc/` 目录下是否存在 `adguardhome.yaml` 配置文件，不存在则在[这里]()下载并复制到指定位置。

> 不要手动修改配置文件中的 password 字段，密码已经经过加密。

## 配置 ADG 

在 ADG 启动后访问：http://<路由器地址>:3000，并根据其指南进行 DNS 和过滤器配置。

对于过滤器，推荐使用秋风的[过滤清单](https://github.com/TG-Twilight/AWAvenue-Ads-Rule)。

## DNS 配置

将以下指令写入文件中并执行：

```bash
# Get the first IPv4 and IPv6 Address of router and store them in following variables for use during the script.
NET_ADDR=$(/sbin/ip -o -4 addr list br-lan | awk 'NR==1{ split($4, ip_addr, "/"); print ip_addr[1] }')
NET_ADDR6=$(/sbin/ip -o -6 addr list br-lan scope global | awk 'NR==1{ split($4, ip_addr, "/"); print ip_addr[1] }')
 
echo "Router IPv4 : ""${NET_ADDR}"
echo "Router IPv6 : ""${NET_ADDR6}"
 
# 1. Enable dnsmasq to do PTR requests.
# 2. Reduce dnsmasq cache size as it will only provide PTR/rDNS info.
# 3. Disable rebind protection. Filtered DNS service responses from blocked domains are 0.0.0.0 which causes dnsmasq to fill the system log with possible DNS-rebind attack detected messages.
# 4. Move dnsmasq to port 54.
# 5. Set Ipv4 DNS advertised by option 6 DHCP 
# 6. Set Ipv6 DNS advertised by DHCP
uci set dhcp.@dnsmasq[0].noresolv="0"
uci set dhcp.@dnsmasq[0].cachesize="1000"
uci set dhcp.@dnsmasq[0].rebind_protection='0'
uci set dhcp.@dnsmasq[0].port="54"
uci -q delete dhcp.@dnsmasq[0].server
uci add_list dhcp.@dnsmasq[0].server="${NET_ADDR}"
 
#Delete existing config ready to install new options.
uci -q delete dhcp.lan.dhcp_option
uci -q delete dhcp.lan.dns
 
# DHCP option 6: which DNS (Domain Name Server) to include in the IP configuration for name resolution
uci add_list dhcp.lan.dhcp_option='6,'"${NET_ADDR}" 
 
#DHCP option 3: default router or last resort gateway for this interface
uci add_list dhcp.lan.dhcp_option='3,'"${NET_ADDR}"
 
#Set IPv6 Announced DNS
for OUTPUT in $(ip -o -6 addr list br-lan scope global | awk '{ split($4, ip_addr, "/"); print ip_addr[1] }')
do
	echo "Adding $OUTPUT to IPV6 DNS"
	uci add_list dhcp.lan.dns=$OUTPUT
done
uci commit dhcp
/etc/init.d/dnsmasq restart
```

## 配置局域网反向查询

在 ADG 设置>DNS 设置中找到"私人反向DNS“，并填写：`<路由器局域网地址>:54`，该端口是在先前的脚本中配置的，并指向路由器原本的DNS服务。  
且在“上游服务器”中填入：

```plain
[/lan/]127.0.0.1:54
```

以引导本地的 DNS 查询至路由器 DNS 服务。

到这里的话，在 ImmortalWRT 中配置 Adguardhome 的工作就完成了，在 ADG 网页中可以看见来自局域网的设备发送的 DNS 请求与解析结果。