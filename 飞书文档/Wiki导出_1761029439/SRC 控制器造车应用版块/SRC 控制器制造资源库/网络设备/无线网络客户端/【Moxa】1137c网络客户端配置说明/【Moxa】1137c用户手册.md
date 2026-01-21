# 【Moxa】1137c用户手册

## 【Moxa】1137c用户手册

## 版本

## 更新日期

## 更新说明

## 文档状态

## 维护责任人

## V1.0

### 2024.4.16

## 语雀迁移飞书

## 使用中

9186c991-ebbc-4ce4-8d7f-c9a87a299678.png

## 注意事项：

① 禁止将 AMB 底盘中使用的 moxa 设置为 DHCP 获取 IP 地址。
② 禁止将 AMB 底盘中使用的 moxa 开启 Mac Clone
## 硬件介绍

## LED 指示灯

位于 AWK-1137C 前面板上和侧面板上的 LED 提供了一种快速简便的方法来确定当前的操作状态和无线设置。

SYS LED 指示系统故障和用户配置的事件。 如果 AWK-1137C 无法从 DHCP 服务器检索 IP 地址，则 SYS LED 将以一秒钟的间隔闪烁。

下表总结了如何从 LED 显示屏读取设备的无线设置。 有关更多信息，请参见“基本 WLAN 设置”部分的第 3 章。

## LED

## Color

## State

## Description

## SYS

## Green

## 常亮

## 系统启动完成，系统正在运行

## 闪烁+哔声（间隔 1 秒）

## 无线搜索程序已找到设备

## Red

## 常亮

## 系统正在引导或发生了系统引导错误

## 闪烁（间隔 0.5 秒）

## IP 地址冲突

## 闪烁（间隔 1 秒）

### 无法从 DHCP 服务器获取 IP 地址

## WLAN

## Green

## 常亮（RSSI> 35）

## WLAN 接口已连接

## 闪烁

## 通过 WLAN 进行数据通信

## Amber

## 常亮

## WLAN 接口已连接

## 闪烁

## 通过 WLAN 进行数据通信

## Beeper

系统就绪时，蜂鸣器会发出两声短促的哔声。

## Reset Button

RESET 按钮位于 AWK-1137C 的侧面板上。 您可以用尖的物体（例如展开的回形针）按下 RESET 按钮，重新启动 AWK-1137C 或将其重置为出厂默认设置。

•系统重新启动：按住 RESET 按钮 5 秒钟以下，然后松开。

•重置为出厂默认设置：按住 RESET 按钮 5 秒钟以上，直到 STATE LED 开始闪烁绿色。 释放按钮以重置 AWK-1137C。

9aad3dcc-5dcb-448b-87dc-eef21ea8aea2.png

First-time Installation and Configuration

在安装 AWK-1137C 之前，请确保包装清单中的所有物品都在包装盒中。 您需要使用配备了以太网端口的笔记本电脑或 PC 访问 moxa。 AWK-1137C 具有默认 IP 地址，首次连接到设备时必须使用该默认IP地址。

## step1 ： 连接电源

AWK-1137C 需要使用 DC 电源供电。

### step2 ： 使用笔记本电脑或者 PC 连接 moxa

由于 AWK-1137C 支持 MDI / MDI-X 自动感应，因此您可以使用直通电缆或交叉电缆将 AWK-1137C连接到计算机。 建立连接后，AWK-1137C 的 LAN 端口上的 LED 指示灯将点亮。

## step3 ：设置计算机的 IP 地址

在与 AWK-1137C 相同的子网上选择一个IP地址。 由于 AWK-1137C 的默认 IP 地址为 192.168.127.253 ，子网掩码为 255.255.255.0 ，因此您应将计算机的 IP 地址设置为 192.168.127.xxx。

## 注意：
选择 Maintenance - Load Factory Default 并单击提交按钮后，AWK-1137C 将重置为出厂默认设置，并且 IP 地址将重置为 192.168.127.253。

step4 ：使用基于 Web 的管理器配置 AWK-1137C

打开计算机的 Web 浏览器，然后在地址字段中输入 http://192.168.127.253 ，以访问基于Web的网络管理器的主页。 在打开主页之前，您需要输入用户名和密码，如下图所示。 对于首次配置，输入默认的用户名和密码，然后单击“登录”按钮。
1664abac-87fe-40ab-a24e-efd0cafe954f.png

## 注意：
## 默认的账户和密码：
## 用户名  ： admin
## 密码     ： moxa

7cc620ef-9bda-474f-83f2-892dfecb34ba.png

强烈建议您更改默认密码，以确保更高的安全级别。 为此，选择 Maintenance - Password ，然后按照屏幕上的说明更改密码。
## 注意：
单击提交以应用更改后，网页将刷新并显示（更新）状态，并在网页的右上角显示闪烁的提醒。
要激活更改，请单击“重新启动”，然后在更改设置后单击“保存并重新启动”。 AWK-1137C 需要大约 30 秒才能完成重新启动过程。

### step5： 选择 AWK-1137C 的工作模式

默认情况下，AWK-1137C 的操作模式设置为客户端。 有关配置 AWK-1137C 操作的详细信息，请参阅第3章。

## step6 ：测试连接

## 界面介绍

## Overview（概述）：
c44696f0-0b7c-4014-830d-64f7e28dc517.png

此界面主要介绍此台 moxa 的系统信息、设备信息和连接的网络信息。

## System Information

## Model name （型号）

## AWK-1137C-EU

## Device name （设备名称）

## AWK-1137C_75:0C:8D

### Serial number  （串口数量）

## TAHKQ1001566

### System uptime （系统工作时间）

## 0 days 00h:32m:04s

### Firmware version  （固件版本）

## 1.3 Build 18121212

## Device Information

Device MAC address （moxa MAC 地址）

## 00:90:E8:75:0C:8D

## IP address  (IP 地址）

### 192.168.127.253

## Subnet mask （子网掩码）

### 255.255.255.0

## Gateway （网关）

### 802.11  Information

## Country code (地区编号）

## EU （欧洲）

### Operation mode (工作模式）

## Client (客户端模式）

## Channel (信道)

## 11 信道

### RF type (802.11 协议版本）

## B/G/N Mixed

### Channel width (通道宽度）

## 20M

## SSID （无线网络名称）

## Seer-robotics

## General Setup

### System Information （系统信息）

25d323fe-5aa9-4cba-b9a8-e25f847ab0cd.png

Device name 对应上面的概述，这里可作更改。部分客户，如成都西门子加域入网时可能会对设备名称有要求，对应改这里。

Login authentication failure message 对应的内容则是对应登录界面输错密码时的显示内容，默认为 Invalid username or password。

### Interface On/Off （LAN 口开关）
e9e18e06-5a72-4d49-b92c-6a163fadbad1.png

用来选择 LAN1 或者 LAN2 打开/禁用。

### Network Settings （网络设置）
1bfe6309-7ab5-4d29-a592-8f59e2fa5dc0.png

IP address assignment  （IP 地址获取方式）

## DHCP /  Static

## IP address （IP 地址）

## Subnet mask  （子网掩码）

## Gateway （网关）

Primary DNS server （优先选择 DNS 地址）

### Secondary DNS server

## 这里有几个点需要注意：

① 默认的 IP 地址改了要做好存档和标识。但是同一个局域网中存在多个moxa的情况下，最好将默认的 IP 地址改掉。

② 禁止将 IP 改为 DHCP 模式（防止无法得知路由分配的地址，无法进入设置界面）

## System Time (系统时间）

b377fe13-e7e6-4cab-9574-418c606a7518.png

系统时间，由于一般工厂网络配置时不会连接 DNS，所以 moxa 无法获取 NTP 服务器时间，这个地方可以选择手动配置，配置时间一般用于查 log 方便。

## Wireless LAN Setup

## AeroMag

配合 MOXA AP（AWK3131A 或 AWK4131A）使用的快速设置模式，一般禁用。

Moxa 的 AeroMag 工具可以根据当前的无线环境和 APs 位置快速、自动、无误地配置基本的 Wi-Fi 设置。在 AeroMag 拓扑中，使用 AWK-1137C 作为 AeroMag 客户端，使用 AWK-3131A 或 AWK-4131A 作为 AeroMag AP。
## Operation Mode
dfe87292-78c2-4852-930d-962c1d9c049b.png

工作模式选择，1137 共有 4 种工作模式，Client、Client-Router、Slave 和 Sniffer。

## setting

## Description

## Factory Default

## Client

## AWK-1137C 扮演无线客户端角色

## Client

## Client-Router

AWK-1137C 扮演的是无线客户端角色，但包含路由器功能，将 WLAN 和 LAN 接口划分为两个子网。

## Slave

## AWK-1137C 扮演的是从站的角色

## Sniffer

将设备转换为远程 Wireshark 接口，捕获 802.11 数据包进行分析。

### WLAN / Basic WLAN Setup

b740154e-a324-4839-a6f7-85e6fb498d4b.png

这里讲一下 RF type 如果使用 2.4GHz 网络选择 B/G/N Mixed,如果使用 5GHz 网络选择 A/N Mixed.

### 通过点击 Site Survey 来发现周围网络

d18331cd-fc52-4fd8-b022-b39551d1d30e.png

## 单击需要的 SSID，然后选择确定

f1c7abca-1e22-4679-a795-9f13a961c030.png

### 如果在网络列表内找不到需要的网络，可能会存在两种情况：

① RF type 选择错误，比如需要选择的网络是 2.4G，但是设置了 A/N Mixed。

因为 802.11n 同时工作在 2.4G 和 5G，所以有时在列表中可能看到 2.4G 和 5G 信道的网络。

② 1137C-EU 为欧标设备，在欧标中，5G 信道只到 140，所以国标的信道会在列表中无法出现。

国标 5G 信道为 149、153、157、161 和 165.

f74c4de4-b71a-4590-9993-e269f8121a4a.png

WLAN Security Settings （WLAN 安全配置）

d6914279-d362-4235-a1c4-9524cf8983b3.png

Open: 无加密，开放网络。

WEP： 是 Wired Equivalent Privacy 的简称，有线等效保密（WEP）协议是对在两台设备间无线传输的数据进行加密的方式，用以防止非法用户窃听或侵入无线网络。使用一个静态的密钥来加密所有的通信，那么如果网管人员想更新密钥，就得亲自访问每台主机。

WPA： WPA 全名为 WI-FI Protected Access，有 WPA 和 WPA2 两个标准，是一种保护无线安全的系统，它是应研究者在前一代的系统有线等效加密（WEP）中找到的几个严重的弱点而产生的。与之前 WEP 的静态密钥不同，WPA 需要不断的转换密钥。WPA 采用有效的密钥分发机制。

WPA2： 实现了 802.11i 的强制性元素，特别是 Michael 算法被公认彻底安全的 CCMP（计数器模式密码块链消息完整码协议）讯息认证码所取代。

Advanced WLAN Settings （无线局域网配置）

c6efc6d6-486b-49b1-b49e-53fa8ead4b0b.png

本节将介绍其他与无线相关的参数，以帮助您详细设置无线网络。

### Transmission rate （传输速率）

## Setting

## Description

## Factory Default

## Auto

### AWK-1137C 自动感知并调整数据速率

## Auto

## Available rates

用户可以手动选择目标传输数据速率，但不支持 G/N 混合、B/G/N 混合和 a /N 混合的 RF 类型。

Minimum transmission rate （最低传输速率）

## Setting

## Description

## Factory Default

## 0 to 64 Mbps
## (0 to disable)

通过设置最低传输速率，AWK-1137C 将避免与弱信号无线链路通信，以保持整体无线性能并优化无线频率使用。

## 0 (Disable)

### Transmission power （传输功率）

## Setting

## Description

## Factory Default

### Available power（有效功率）

用户可以手动选择一个目标电源来屏蔽最大输出电源。由于不同的传输速率有各自的最大输出功率，请参考产品数据表。对于 802.11bg，可用的设置范围是 0 到 20

## 20 dBm

Fragmentation threshold （碎片阈值）

## Setting

## Description

## Factory Default

Fragment Length (256 to 2346) （片段长度（256 至 2346））

### 指定数据包拆分和创建另一个新包之前的最大大小

## 2346

### RTS threshold （RTS 阈值）

## Setting

## Description

## Factory Default

RTS/CTS Threshold (32 to 2346)

确定在接入点协调发送和接收以确保有效通信之前数据包的大小。

## 2346

注意：您可以参阅附录 A 中的相关词汇表，以获取有关上述设置的详细信息。 通过正确设置这些参数，您可以更好地调整无线网络的性能。

## Antenna

## Setting

## Description

## Factory Default

## A/B/Both

指定输出天线端口。 将“天线”设置为“自动”可在 802.11n 和传统 802.11a / b / g 模式下的 2T2R *通信下进行 2x2 MIMO 通信。

## Both

*与 802.11n 的多空间数据流（2x2 MIMO）不同（后者使吞吐量提高了一倍），2T2R 在两个天线端口上发送/接收相同的数据。

## WMM

## Setting

## Description

## Factory Default

## Enable/Disable

WMM 是 WLAN 流量的 QoS 标准。 与支持 WMM 的无线客户端一起启用时，语音和视频数据将获得优先带宽。
注意：WMM 将始终在 802.11n 模式下启用。

## Enable

## Turbo Roaming

## Setting

## Description

## Factory Default

## Enable/Disable

当作为客户端的 AWK-1137C 在一组 AP 之间漫游时，Moxa 的 Turbo Roaming 可以实现快速切换。

## Disable

### 启用 Turbo Roaming 后，将显示以下参数：

• Roaming threshold: 确定何时开始寻找新的 AP 候选者。 如果当前连接质量（SNR 或信号强度）低于指定的阈值，则AWK将开始进行后台扫描并寻找下一跳候选对象。

## 注意：
当 AWK 设备执行背景扫描时，无线性能将降低其正常性能的 1/3。

• Roaming difference: 确定是否应执行漫游。 触发后台扫描后，仅当候选 AP 提供的连接质量（基于漫游差异值）比当前连接更好时，才会发生漫游。 如果多个接入点符合条件，则 AWK 设备将选择最佳漫游点。

• Scan channels: 预定义通信和漫游通道。

• AP alive check： 允许 AeroLink Protection 对 WLAN 断开做出更快的反应。

## 注意：
启用此功能会使 AWK-1137C 在没有流量检查连接是否有效时每 10 毫秒发送一次数据包。 小型活检数据包的高传输频率可能会影响您使用同一信道的其他无线通信，因此仅当您完全控制指定的无线信道时才启用此功能。

•AP candidate threshold: 在“ AP alive check”声明当前接入点不再可用之后，周围的接入点必须具有足够好的连接质量（SNR /信号强度），才能成为关联的 AP 候选者。

882cb300-9040-44a6-b62d-36572b1fefeb.png

## 注意
产品文档中列出的 Turbo Roaming 恢复时间（<150 ms）是在配置了无干扰 20 MHz RF 通道，WPA2-PSK 安全性和默认 Turbo Roaming 参数的 AP 的优化条件下记录的测试结果的平均值 。 客户端配置了 3 通道漫游，流量负载为 100 Kbps。 但是，多种因素共同影响漫游客户端的 AP 切换恢复时间，包括但不限于以下因素：
## •现场射频干扰
## •移动客户端设备的速度
## •应用流量吞吐量
•已配置 Turbo Roaming 参数。 即漫游阈值，漫游差异和 AP 候选阈值。
因此，建议在设备部署之前进行现场调查，以评估客户端和 AP 上的理想参数设置，以便为您的应用程序制定最佳的部署计划。

## MAC clone

## Setting

## Description

## Factory Default

## Enable/Disable

启用此功能允许 AWK 客户端将 LAN 连接设备的 MAC 地址复制为自己的设备。 这克服了在 MAC 敏感网络（基于 MAC 的通信或 MAC 认证的网络）中 IP 桥接行为的限制。 限制：启用此功能后，仅允许一个设备连接到 AWK 客户端。

## Disable

• MAC clone interface: 确定 AWK 客户端应复制连接哪个 LAN 端口的设备的 MAC 地址。

WLAN Certificate Settings (For EAP-TLS in Client/Slave Mode Only)

使用 EAP-TLS 时，客户端需要 WLAN 证书才能支持 WPA / WPA2-Enterprise。AWK-1137C 可以支持 PKCS＃12（也称为个人信息交换语法标准），证书格式定义了通常用于存储私钥和随附的公钥证书的文件格式，并受基于密码的对称密钥保护。

7e97cea3-4778-457d-8ddb-a4caba1ed600.png

当前状态显示有关当前 WLAN 证书的信息，该信息已导入 AWK-1137C。 如果没有证书，则不会显示任何内容。

## 证书颁发给：显示证书用户

## 证书颁发者：显示证书颁发者

## 证书到期日期：指示证书何时到期

您可以按照以下步骤依次在“导入 WLAN 证书”中导入新的 WLAN 证书：

## 1.在“证书专用密码”字段中输入相应的密码（或密钥），然后单击“提交”以设置密码。

### 2.该密码将显示在“证书专用密码”字段中。 单击选择证书/密钥文件中的浏览按钮，然后选择证书文件。

3.单击上载证书文件以导入证书文件。 如果导入成功，您可以在“当前证书”中看到上载的信息。 如果失败，则可能需要返回到步骤 1 以正确设置密码，然后再次导入证书文件。
1a89f47c-64b4-47fa-8e1b-2d0ab7283295.png

## 注意
在 AWK-1137C 重新启动后，WLAN 证书将保留。 即使已过期，仍可以在当前证书上看到它。

### Advanced Setup （进阶设定）

若干高级功能可用于增强 AWK-1137C 和无线网络系统的功能。VLAN 是客户端和主机分组在一起的集合，就好像它们已连接到第 2 层网络中的广播域一样。DHCP 服务器可帮助您有效地部署无线客户端。 数据包过滤器在不同的网络层中提供安全机制，例如防火墙。 此外，AWK-1137C 的 SNMP 支持可以使网络管理更加容易。

0d146b05-50f6-4204-8f9c-cc284e797d8c.png

DHCP Server (For Client-Router Mode Only)

DHCP（动态主机配置协议）是一种网络协议，管理员可以通过在有限的时间内向用户“租借” IP 地址而不是分配永久 IP 地址的方式，将临时 IP 地址分配给网络计算机。

AWK-1137C 可以充当简化的 DHCP 服务器，并通过响应来自客户端的 DHCP 请求，轻松地为您的 DHCP 客户端分配 IP 地址。 您在此页面上设置的与 IP 相关的参数也将发送到客户端。

您还可以通过输入特定的客户端的 MAC 地址来为其分配静态 IP 地址。AWK-1137C 提供最多包含 16 个实体的静态 DHCP 映射列表。 提醒您选中每个实体的“活动”复选框以激活设置。

您可以在状态-DHCP 客户端列表下检查 IP 分配状态。

5f93eeca-2317-4d61-82b2-b3e5b337b97d.png

## DHCP server

## Setting

## Description

## Factory Default

## Enable

### 将 AWK-1137C 启用为 DHCP 服务器

## Disable

## Disable

## 禁用 DHCP 服务器功能

### Default gateway (网关）

## Setting

## Description

## Factory Default

## 默认网关的 IP 地址

## 连接到外部网络的路由器的 IP 地址

## None

## Subnet mask （子网掩码）

## Setting

## Description

## Factory Default

## 子网掩码

标识子网的类型（例如，对于 B 类网络为 255.255.0.0，对于 C 类网络为 255.255.255.0）

## None

### Primary/ Secondary DNS server

## Setting

## Description

## Factory Default

## 主备 DNS 服务器的 IP 地址

网络使用的 DNS 服务器的 IP 地址。 输入 DNS 服务器的 IP 地址后，您也可以使用 URL。 如果主 DNS 服务器无法连接，则将使用辅助 DNS 服务器。

## None

## Start IP address

## Setting

## Description

## Factory Default

## IP address

### 表示 AWK-1137C 可以开始分配的 IP 地址

## None

Maximum number of users （最大用户数量）

## Setting

## Description

## Factory Default

## 1 to 999

## 指定可以连续分配多少个 IP 地址

## None

测试发现这个值最大能到 128，即允许的用户最大数量为 128

## Packet Filters

AWK-1137C 包括各种过滤器，用于通过 LAN 和 WLAN 接口的基于 IP 的数据包。 您可以将这些过滤器设置为防火墙，以帮助增强网络安全性。

## MAC Filters

AWK-1137C 的 MAC 过滤器是基于策略的过滤器，可以允许或过滤出具有指定 MAC 地址的基于 IP 的数据包。AWK-1137C 提供 32 个实体，用于在您的过滤策略中设置 MAC 地址。 切记为每个实体选中“活动”复选框以激活设置。
99726774-34f1-44de-8134-0a1f1578b0d0.png

## MAC filters

## Setting

## Description

## Factory Default

## Enable

## 启用 MAC 过滤器

## Disable

## Disable

## 禁用 MAC 过滤器

## Policy

## Setting

## Description

## Factory Default

## Accept

仅允许匹配列表中实体的数据包。

## Drop

## Drop

任何适合列表中实体的数据包都将被拒绝。

## 注意
## 启用过滤器功能时请小心：
### 丢弃+“列表上没有实体被激活” =允许所有数据包
### 接受+“列表上没有实体被激活” =所有数据包均被拒绝

## IP Protocol Filters

AWK-1137C 的 IP 协议过滤器是一种基于策略的过滤器，它可以允许或过滤出具有指定 IP 协议和源/目标 IP 地址的基于 IP 的数据包。

AWK-1137C 提供 32 个实体，用于在您的过滤策略中设置 IP 协议和源/目标 IP 地址。 有四种 IP 协议可用：全部，ICMP，TCP 和 UDP。 您必须指定源 IP 或目标 IP。 通过组合 IP 地址和网络掩码，可以指定一个 IP 地址或一个 IP 地址范围以接受或删除。 例如，“ IP 地址 192.168.1.1 和网络掩码 255.255.255.255”是指唯一的 IP 地址 192.168.1.1。 “ IP 地址 192.168.1.1 和网络掩码 255.255.255.0”是指 IP 地址从 192.168.1.1 到 192.168.255 的范围。 切记为每个实体选中“活动”复选框以激活设置。

d0ffcc61-a7d5-4fc2-8902-b8e1f7c17280.png

## IP protocol filters

## Setting

## Description

## Factory Default

## Enable

## 启用 IP 过滤器

## Disable

## Disable

## 禁用 IP 过滤器

## Policy

## Setting

## Description

## Factory Default

## Accept

仅允许匹配列表中实体的数据包。

## Drop

## Drop

任何适合列表中实体的数据包都将被拒绝。

## 注意
## 启用过滤器功能时请小心：
### 丢弃+“列表上没有实体被激活” =允许所有数据包
### 接受+“列表上没有实体被激活” =所有数据包均被拒绝

### TCP/UDP Port Filters

AWK-1137C 的 TCP / UDP 端口过滤器是基于策略的过滤器，可以允许或过滤具有指定源或目标端口的基于 TCP / UDP 的数据包。

AWK-1137C 提供 32 个实体，用于设置特定协议的源/目标端口范围。 除了选择 TCP 或 UDP 协议，您还可以设置源端口，目标端口或同时设置两者。 如果仅指定单个端口，则可以将末端端口留空。 当然，结束端口不能大于开始端口。

应用程序名称是一个文本字符串，描述了最多 31 个字符的相应实体。 切记为每个实体选中“活动”复选框以激活设置。
0e2d6c3c-b7ac-4733-bac8-30bc3d92b17e.png

### TCP/UDP port filters

## Setting

## Description

## Factory Default

## Enable

## 启用 TCP/UDP 过滤器

## Disable

## Disable

## 禁用 TCP/UDP 过滤器

## Policy

## Setting

## Description

## Factory Default

## Accept

仅允许匹配列表中实体的数据包。

## Drop

## Drop

任何适合列表中实体的数据包都将被拒绝。

## 注意
## 启用过滤器功能时请小心：
### 丢弃+“列表上没有实体被激活” =允许所有数据包
### 接受+“列表上没有实体被激活” =所有数据包均被拒绝

Static Route (For Client-Router Mode Only)

“静态路由”页面用于配置 AWK-1137C 的静态路由表。
c2e8c407-cccf-44ee-ab55-1f59c5287743.png

## Active

单击复选框以启用“静态路由”。

## Destination

指定目的 IP 地址。

## Netmask

指定此 IP 地址的子网掩码。

## Gateway

指定将 LAN 连接到外部网络的路由器的 IP 地址。

## Metric

指定访问相邻网络的“费用”。

## Interface

指定此路由规则的指定网络接口。

NAT Settings/Port Forwarding (For Client-Router Mode Only)

支持网络地址转换（NAT），或者更具体地说，是一对多 NAT，NAPT 或 PAT，以促进Client-Router操作模式。 此功能将传出的通信从多个专用 IP 转换为单个外部 IP（WLAN IP），该 IP 具有随机分配的用于返回流量的端口。
8070fd79-f730-4cb6-bc30-cfad2c58581e.png

为了允许外部设备启动通信，端口转发用于在外部端口（WAN 端口）和内部 IP /端口组合（LAN IP / LAN 端口）之间指定静态映射。

37ead0e4-fc87-4935-ae5a-54da60ec4b2c.png

### 启用 NAT 和端口转发可带来以下好处：

•使用 NAT 功能隐藏关键网络或设备的内部 IP 地址，以提高工业网络应用程序的安全级别。

•对不同或相同的以太网设备组使用相同的专用 IP 地址。 例如，一对多 NAT 使复制或扩展相同的生产线变得容易

f9fa2f14-a648-4026-bd89-1795cf3e27f3.png

## NAT

## Setting

## Description

## Factory Default

## Enable/Disable

## 启用或禁用 NAT 转换

## Disable

## 转发端口

活动：单击复选框以启用端口转发规则。

协议：指定通信协议。

WAN 端口：指定要转发到的外部端口。

LAN IP：指定“转发到” LAN IP。

LAN 端口：指定“转发到” LAN 端口。

Link Fault Pass-through (For Client/Slave Mode Only)

此功能意味着如果以太网端口已断开连接，则将强制断开无线连接。 一旦以太网链接恢复，AWK 将尝试连接到 AP。

如果无线断开连接，AWK 将重新启动以太网端口上的自动协商，但始终保持链路故障状态。 恢复无线连接后，AWK 将尝试恢复以太网链接。

除了原始的链接打开/关闭事件之外，系统日志还将指示链接故障通过事件。

734bc0a0-3e78-4f44-9c1e-3957f4f35cb5.png

### Logs and Notifications

由于工业级设备通常位于系统的端点，因此这些设备将不会总是知道网络上其他地方正在发生什么。 这意味着这些设备（包括无线 AP 或客户端）必须向系统维护人员提供实时警报消息。 即使系统管理员长时间不在控制室中，当发生异常时，他们仍几乎可以立即获悉设备状态。

## System Logs

### System Log Event Types

## 系统日志事件类型

下表显示了分组事件的详细信息。 选中启用日志记录复选框以启用分组事件。 所有默认值均已启用（选中）。 系统事件的日志可以在状态-系统日志中查看。

c7da6125-dcf7-49d1-ba19-d5662f993b4d.png

## 与系统相关的事件

## 在以下情况下触发事件：

## System warm start

AWK-1137C 将重新启动，例如更改其设置（IP 地址，子网掩码等）时。

## System cold start

AWK-1137C 通过断电重启。

### Watchdog triggers reboot

## AWK-1137C 由监视程序重新启动

## 与网络相关的事件

## 在以下情况下触发事件：

## LAN link on

LAN 端口已连接到设备或网络。

## LAN link off

端口已断开连接（例如，电缆被拔出或对面的设备关闭）。

### WLAN connected to AP
### (for Client/Slave mode)

AWK-1137C 与 AP 链接。

## WLAN disconnected
### (for Client/Slave mode)

AWK-1137C 与 AP 断开。

Client Roaming from previous AP to current AP (for Client/Slave mode)

如果当前 AP 的信号强度比先前 AP 的信号强度大某个值，则客户端从先前 AP 漫游到当前 AP。

## IP address conflict

AWK-1137C 具有与连接到同一子网的另一台设备相同的 IP 地址。

Link fault pass-through LAN/WLAN connected because of WLAN/LAN up

WLAN / LAN 链接已建立，并且链接故障通过（LFPT）启用了 LAN / WLAN 功能。

Link fault pass-through LAN/WLAN disconnected because of WLAN/LAN down

WLAN / LAN 链接已断开，并且链接故障通过（LFPT）禁用了 LAN / WLAN 功能。

## Status

## Wireless LAN Status

### 802.11 信息参数的状态，例如“工作模式”和“信道”，显示在“无线状态”页面上。 如果选中“自动更新”框，则状态每 5 秒钟刷新一次。

使用此页面上不断更新的信息（例如信号强度，本底噪声和 SNR）来监视客户端模式下 AWK-1137C 的信号强度会很有帮助。

35c5047e-d6b6-44f0-b421-fe1deda0f802.png

DHCP Client List (For Client-Router Mode Only)

DHCP 客户端列表显示所有需要且已成功接收 IP 分配的客户端。 您可以单击刷新按钮来刷新列表。

6f2ff06b-95a6-4731-a03a-20dacca5ee5e.png

您可以按“全选”按钮以选择列表中的所有内容以进行进一步编辑。

4f7d4b56-3802-4a59-840a-16fb98e4eff9.png

## System Logs

触发事件记录在系统日志中。 您可以通过单击“导出日志”将日志内容导出到可用的查看器。 您可以使用“清除日志”按钮清除日志内容，并使用“刷新”按钮刷新日志。

03ad7d29-2412-446b-8fcb-b7e2f5749ca6.png

## 注意
系统日志中提供的原因码信息仅供 Moxa 工程师用于诊断和故障排除目的。

## System Status

系统状态部分指示当前设备中设备内存的状态和 CPU 使用情况

## 注意
CPU 过载可能导致看门狗触发系统重启。 大量防火墙规则（IP / MAC /协议过滤器）和流量 PPS（每秒数据包）等因素导致 CPU 使用率上升。

ba4abe25-e912-40ef-ba79-9d8b8171188d.png

## Network Status

网络状态部分根据 ARP，网桥状态，LLDP，RSTP 和路由表指示设备的网络状态。

## ARP Table

地址解析协议（ARP）表-指示设备当前的 IP 到 MAC 地址的映射。

c316f7af-54e0-4688-9384-3258f024b053.png

## Bridge Status

指示设备上网桥的当前状态。 本节中的接口和相应的 MAC 地址是入口流量的入口点。

b557444e-e815-4a2c-9610-b5e425a00680.png

## LLDP Status

显示有关通过 LLDP（链路层发现协议）收集的相邻设备的信息。

b6345a29-0745-4873-be8f-b25b749435fc.png

## 注意
AWK-1137C 的 LLDP 功能不支持 IEEE 802.3。

## Routing Table

显示当前设备的路由信息。
d030e949-8093-46d2-b7cc-fe75f1f1377f.png

## Maintenance （维护）

维护功能为管理员提供了管理 AWK-1137C 和有线/无线网络的工具。

## Console Settings

您可以启用或禁用对设备和 Moxa 服务（例如 MXstudio 和 Wireless Search Utility）的访问权限。 为了提高安全性，我们建议仅允许访问两个安全控制台 HTTPS 和 SSH。

c7d3b634-b660-4a0b-bc95-df9baaf5939b.png

## Ping

Ping 有助于诊断有线或无线网络的完整性。 通过在“目标”字段中输入节点的 IP 地址，您可以使用 ping 命令来确保其存在以及访问路径是否可用。

6f565dd2-cb3c-4dec-84a1-cfab4af6b1d9.png

如果节点和访问路径可用，您将看到所有数据包均已成功传输而没有丢失。 否则，某些甚至全部数据包可能会丢失，如下图所示。

95df69de-bbec-4793-bf46-f1f474f44f33.png

### Firmware Upgrade （固件更新）

通过安装固件升级，可以通过更多增值功能增强 AWK-1137C 的功能。 最新的固件可从 Moxa 的下载中心获得。

在运行固件升级之前，请确保 AWK-1137C 处于离线状态。 单击浏览按钮以指定固件映像文件，然后单击固件升级和重新启动以启动固件升级。 进度条达到 100％后，AWK-1137C 将自行重启。

升级固件时，AWK-1137C 的其他功能被禁止。

5dbccb9a-71d2-489f-8c2a-ba6fc7335dae.png

## 注意
升级固件时，请确保电源稳定。 电源意外中断可能会损坏您的 AWK-1137C。

Configuration Import and Export （配置导入/导出）

### 您可以使用“配置导入和导出”页面来备份或还原以下内容：

## •AWK-1137C 上的配置设置

## •MIB

在“配置导入”部分中，单击“选择文件”以选择一个配置文件，然后单击“导入配置”按钮以开始导入配置设置。 密码最多 31 个字符。

要将配置文件保存到存储介质，请单击“导出配置”。 配置文件是一个文本文件，您可以使用常规的文本编辑工具进行查看和编辑。

单击“导出 MIB”以将 MIB 文件保存到存储介质。 配置文件是一个* .my 文件，您可以使用常规 SNMP 工具导入该文件，并将其用于远程控制或配置 AWK-1137C。

52991b4e-16a3-4180-95c8-24582d93d8c8.png

在“配置导出”部分中，单击“导出配置”按钮，然后将配置文件保存到本地存储介质上。 配置文件是一个文本文件，您可以使用常规的文本编辑工具进行查看和编辑。

## image.png

SNMP MIB 文件也可以从“ SNMP MIB 文件导出”部分下载。
## image.png

### Load Factory Default （恢复出厂设置）

使用此功能可重置 AWK-1137C，并将所有设置还原为出厂默认值。 您还可以通过按 AWK-1137C 顶部面板上的重置按钮来重置硬件。

c178df81-89b9-4462-b1eb-5d70dbf3c869.png

### Account Settings （账户设置）

为确保位于远程站点的设备不受黑客的攻击，建议您在首次配置设备时设置一个高强度密码。

ebdf6bda-f940-40d7-b031-f8fe8e12e767.png

*帐户名称中唯一允许使用的字符是字母数字字符，“ at”符号（@），句点（。）和下划线（_）。

## Field

## Description

## Default setting

### Minimum password length

默认情况下，密码可以在 4 到 16 个字符之间。 为了提高安全性，建议您在首次配置设备时将最小密码长度更改为至少 8 个字符。

## 4

### Password strength check

启用密码强度检查选项，以确保要求用户选择高强度密码。
注意：有关详细信息，请参见下面的“更改密码”部分。

## Disable

## Password validity

必须更改密码的天数。 密码应定期更新，以防止黑客入侵。

## 90 天

### Password retry count

在锁定设备的登录功能之前，用户在登录时连续输入错误密码的次数。

## 5

## Lockout time

连续 n 次不成功的登录尝试后，设备登录功能将被锁定的秒数，其中 n =密码重试次数。

## 600 秒

9745d536-cee8-48d6-b24d-de762f6c80e8.png

## Field

## Description

## Default setting

## Active

选择启用以启用用户帐户。

## 4

## User level

管理员：允许用户访问 Web UI，更改设备的配置以及使用设备的导入/导出功能。
用户：允许用户访问 Web UI，但用户将无法更改设备的配置或使用设备的导入/导出功能。

## Admin

## Account name

帐户的用户名。

## Admin

## New Password

登录设备的密码。

## moxa

## Confirm Password

重新输入密码。 如果“确认密码”和“新密码”字段不匹配，将要求您重新输入密码。

## N/A

### Change Password （更改密码）

使用更改密码功能来更改现有用户帐户的密码。 首先输入当前密码，然后在“新密码”和“确认密码”输入框中键入新密码。
33f1d949-2e33-4c6e-a870-fcbd19800382.png

## 注意
为了维护更高级别的网络安全性，请不要使用默认密码（moxa），并确保定期更改所有用户帐户密码。

如果启用了密码强度测试选项，系统将提示您使用遵循以下密码策略的密码：
•密码必须至少包含一位数字：0、1、2，…，9。
•密码必须同时包含大小写字母：A，B，…，Z，a，b，…，z。
•密码必须至少包含以下特殊字符之一：〜！@＃$％^ -_：，。<> [] {}
## •密码不能包含以下特殊字符：
## ` ' " | ; &
•密码的字符数必须超过最小密码长度（默认值= 4）。

### Miscellaneous Settings

此页面上提供了有助于您管理 AWK-1137C 的其他设置。

b1ae803f-95c3-4fe0-ae86-12497ed6a103.png

## 选择以下“重置”按钮选项之一：

•始终启用–设置重置按钮以在 AWK-1137C 上执行出厂还原。 这是默认选项。

•60 秒后禁用工厂重置功能–在 AWK-1137C 重新启动后 60 秒钟，禁用重置按钮的工厂重置功能。

## Troubleshooting

此功能使您可以快速获取当前系统状态，并向 Moxa 工程师提供诊断信息。

要导出当前设备信息，请单击“导出”。

e5072c7b-5fbd-4290-8f39-15e04b9e8770.png

如果需要高级故障排除，请联系 Moxa 工程师，该工程师可以为您提供加密的脚本文件。 加密的脚本文件可以捕获系统上的其他详细信息。

要运行脚本，请填写以下详细信息，然后使用“浏览”浏览并选择脚本文件，然后单击“运行脚本”：

97ea9831-cf53-41ed-af19-424ffdc54eac.png

## Setting

## Description

## Diagnostic script

使用浏览按钮选择 Moxa 诊断脚本文件。

### Export diagnostic results

## 选择是否要导出：
## •到文件
## •到TFTP服务器

## TFTP server IP

如果选择了 TFTP 选项，请指定 TFTP 服务器的 IP 地址。

### Diagnostic script name

## 显示脚本文件的名称

## Last start time

## 显示上一次脚本执行的开始时间

## Last end time

## 显示上一次脚本执行的结束时间

## Diagnostic status

## 显示系统诊断的进度

## Diagnostic result

显示系统诊断的结果。
如果选择了导出到文件选项，则系统日志将被加密并打包到文件中。 日志文件大小的限制为 1 MB。 当日志文件的大小达到 1MB 时，将创建另一个文件。 最多将保留 5 个文件（5MB）供下载。 当文件数超过五个时，最早的文件将被删除。

### Save Configuration （保存设置）

下图显示了 AWK-1137C 如何将设置更改存储到易失性和非易失性存储器中。 除非是 y，否则当 AWK-1137C 关闭或重新启动时，易失性存储器中存储的所有数据都将消失。 由于 AWK-1137C 会启动并使用存储在闪存中的设置进行初始化，因此，在重新启动 AWK-1137C 之前，所有新更改都必须保存到闪存中。

这也意味着除非您运行“保存配置”功能或“重新启动”功能，否则新更改将不起作用。

d7f9a4d4-a63f-4251-8856-9d5154c839d9.png

在左侧菜单框中单击“保存配置”后，将出现以下屏幕。 如果您希望此时更新闪存中的配置设置，请单击保存。 或者，您可以选择运行其他功能并推迟保存配置，直到以后。 但是，新设置更改将保留在非易失性存储器中，直到您保存配置。

afdc94e5-6c55-4d05-a2e8-0a03fc04d97c.png

## Restart （重启）

如果您提交了配置更改，则会在屏幕的右上角找到一个闪烁的字符串。 进行所有更改后，单击左侧菜单框中的重新启动功能。 将出现两个不同屏幕之一。

如果您最近进行了更改但没有保存，则会有两个选择。 单击此处的重启按钮将直接重启 AWK-1137C，所有设置更改将被忽略。 单击“保存并重新启动”按钮将应用所有设置更改，然后重新启动 AWK-1137C。

b7122e9c-8b98-4f88-85ef-7ccd649d2126.png

如果您在不更改任何配置或保存所有更改的情况下运行“重启”功能，则屏幕上只会显示一个“重启”按钮。

02e3ab8e-4ef6-4907-b553-2f54c41d1c69.png

系统重启时，您将无法运行 AWK-1137C 的任何功能。

## Logout （切换账户）

注销可帮助用户断开当前的 HTTP 或 HTTPS 会话并转到“登录”页面。 出于安全原因，我们建议您在退出控制台管理器之前先注销。

4d97182d-5c32-4bfd-9e02-d8e7dd4f40e7.png

## 附录

### 附录 A  References（参考文献）

本章提供有关无线相关技术的更多详细信息。 本章中的信息可以帮助您管理 AWK-1137C 和更好地规划工业无线网络。

## Beacon （信标）

信标是 AP 广播的一个分组，用于保持网络同步。 信标包括无线 LAN 服务区域，AP 地址，广播目标地址，时间戳，传递流量指示符映射（DTIM）和流量指示符消息（TIM）。 信标间隔指示 AP 的频率间隔。

DTIM （数字传输接口模块(Digital Transmission Interface Module)）

信标帧中包含传递流量指示图（DTIM）。 它用于指示 AP 缓冲的广播和多播帧将在短期内交付。 较低的 DTIM 设置可防止 PC 进入省电的睡眠模式，从而使联网效率更高。 较高的设置可让您的 PC 进入睡眠模式，从而节省电量。

## Fragment （碎片）

较低的设置表示较小的数据包，这将为每次传输创建更多数据包。 如果降低了该值并遇到较高的数据包错误率，则可以再次提高它，但可能会降低整体网络性能。 建议仅对该值进行较小的修改。

### RTS Threshold （RTS 阈值）

RTS 阈值（256-2346）– RTS 代表“请求发送”。 此设置确定在接入点协调发送和接收以确保有效通信之前数据包可以有多大。 此值应保持其默认设置 2346。 当您遇到不一致的数据流时，建议仅进行较小的修改。

### 关于 Client-Router 模式的使用以及注意事项

在更改模式之前建议 reset 一次 MOXA.

aedc22ea-39b3-48f0-9b26-e9bbb467edd1.png

①在 Operation Mode 处将模式改为 Client-Router，修改后请保存后重启 MOXA。

注：修改为 Client-Router 模式后,MOXA 的 WLAN 口的 IP 地址默认为:192.168.128.253

### LAN 口将会默认为：192.168.127.254

9595727a-e475-405a-9af8-c24356cab1c0.png

②打开 MOXA 后台，修改 WLAN 口的 IP 地址（这个地址为最后全部设备对外的唯一 IP 地址，可以设置为 DHCP，或固定的静态 IP，具体需要看客户的网络策略），

修改 LAN 口的 IP 地址，该地址需要与内部网络的 IP 地址处于同个段内。

比如：车体内的以太网地址修改为 192.168.0.xxx,这个时候 LAN 口的 IP 地址也应为 192.168.0.xxx

### 禁止将以太网地址设置为 192.168.192.xxx

### 联网步骤略，Save and restart
a3745b26-a4d8-4b1a-a7bb-e73e04e2e10e.png

③将所需要的 IP 地址以及端口号在 NAT/Port Forwarding 处写出来，LAN 的 IP 地址为小车的内部以太网地址（也就是 Roboshop 上的那个地址）

注：所有的端口号只能互斥。

附：如果配置完之后在 Roboshop 无法找到 WLAN 口设置的那个 IP 地址的机器人，请 RESET SYSTEM MOXA 再配置一遍。（该问题配置过程中经常遇到，估计为 MOXA 的 BUG，待 MOXA 修复）

## 附录 B  关于信道

信道也称作通道(Channel)、频段，是以无线信号（电磁波）作为传输载体的数据信号传送通道。无线网络（路由器、AP 热点、电脑无线网卡）可在多个信道上运行。在无线信号覆盖范围内的各种无线网络设备应该尽量使用不同的信道，以避免信号之间的干扰。

目前主流的无线 WIFI 网络设备 802.11a/b/g/n/ac：

6c3ff836-002f-40d7-b36e-3268ef8a46cc.png

从下图很容易看到其中 1、6、11 这三个信道之间是完全没有交叠的，也就是人们常说的三个不互相重叠的信道。每个信道 20MHz 带宽。图中也很容易看清楚其他各信道之间频谱重叠的情况。

d1989132-8cbf-4072-852e-b284be9ba07e.png

另外，如果设备支持，除 1、6、11 三个一组互不干扰的信道外，还有 2、7、12；3、8、13；4、9、14 三组互不干扰的信道。

## DFS 信道

目前在 802.11 系列标准中，涉及物理层的有 4 个标准：802.11、802.11b、802.11a、802.11g。根据不同的物理层标准，无线局域网设备通常被归为不同的类别，如常说的 802.11b 无线局域网设备、802.11a 无线局域网设备等。802.11a 工作于 5GHz 频带(在美国为 U-NII 频段：5.15-5.25GHz、5.25-5.35GHz、5.725－5.825GHz)，它采用 OFDM(正交频分复用)技术。802.11a 支持的数据速率最高可达 54Mbps。

这个标准在美国没有问题，但是在欧洲却遇到强烈的抵制。因为欧洲军方的雷达系统广泛运用这一频率(其中探测隐型飞机的雷达就使用这一频率)。如果民用的无线产品也使用这一频率，很可能会对军事雷达和通讯产生干扰。为了解决这一安全顾虑，在欧洲出售的 WLAN 产品必须具备 TPS 和 DFS 这两个功能，即发射功率控制和动态频率选择。TPS 是为了防止无线产品发放过大的功率来干扰军方雷达。DFS 是为了使无线产品主动探测军方使用的频率，并主动选择另一个频率，以避开军方频率。通过这种方式，也可以避免其它 WLAN，最高效地利用波长。这两个功能是属于强制性的，不符合标准的产品将不会获得欧盟的上市许可。

8aa61fd8-a4e2-4425-b387-c94ca937ae44.png

中国大陆规定的 DFS 信道为 52，54，56，60，62，64，由于 moxa 的漫游需要固定信道，所以尽量避免使用 DFS 信道。

## 附录 C  关于同频干扰问题

## 1、什么是同频干扰

在无线 WiFi 覆盖工程中，同频干扰是一个不能回避的问题，同频干扰是指两个 AP 工作频率如果相同，同时收发数据时会产生干扰和延时。

acd4dd74-8b5a-4926-b09a-aa04416dac2c.png

### 2、无线 WiFi 同频干扰的历史根源

在 WiFi 设计之处没有想到会这么普及，原来规划了 11 个信道，一个区域 11 个无线设备足够了。但之后 WiFi 两次升级提速，一个设备同时使用 2 个信道或 4 个信道进行传输，这样就造成了今天的局面，一个设备使用 4 个信道，两个设备要相隔 5 个信道才不会相互干扰，即 1、6、11 相互隔开，致使一个区域只能有 3 个无线设备，显然有点少，容易发生同频干扰。
a6c379e8-aef7-4a3f-8c97-56ad769818e2.png

### 3、无线 WiFi 信道应该如何规划

在无线 WiFi 覆盖工程中，为了减少信号盲区，不得不部署多个 AP 来完成无线覆盖的重叠区域。要注意合理的规划和使用无线信道，相邻的 AP 按 1、6、11 相互隔开。左边图同频干扰严重，右边图改善一些，无线信号重叠部分会导致轻频干扰。

332332c5-41c5-480f-ab87-1916d3951415.png

### 4、无线 WiFi 同频干扰会造成什么影响

无线网络同频干扰会造成网络丢包和延时，导致无线网络质量变得相当差，网速变得很慢，由于两个 AP 工作频率相同互相干扰，导致频繁重复的发送数据，网络使用越频繁，同频干扰越严重。

e6bdec71-9161-48f2-a13c-2afbe04e81eb.png

## 附录 D 关于信号强度

## 了解信号强度

WiFi信号强度非常棘手。最精确的表示方法是毫瓦（mW），但由于 WiFi 的超低发射功率，最终会导致吨位的小数位数，使其难以阅读。例如，-40 dBm 为 0.0001 mW，信号强度下降得越多，零点变得越强。

RSSI  （接收信号强度指示器）是一种常见的测量方法，但是大多数 WiFi 适配器供应商对它的处理方式不同，因为它没有标准化。某些适配器的刻度为 0-60，而另一些适配器的刻度为 0-255。

最终，表达信号强度的最简单，最一致的方法是  dBm，它表示相对于毫瓦的分贝。由于 大多数 WiFi 适配器对 RSSI 的处理方式不同，因此通常将其转换为 dBm，以使其一致且易于阅读。

### mW- 毫瓦（1 mW = 0 dBm）

RSSI- 接收信号强度指示器（通常为 0-60 或 0-255）

### dBm的 - 分贝相对于一毫瓦（通常为 -30〜-100）

## 读数 dBm

了解 dBm 的第一件事是，我们正在研究负数。-30 是比- 80 高的信号，因为 -80 是低得多的数字。

接下来，重要的是要知道 dBm 不会以您期望的线性方式扩展，而是对数扩展。这意味着信号强度的变化不是平滑且渐进的。3s 和 10s 规则强调了 dBm 的对数性质：

### 3 dB 的损耗 = -3 dB = 一半的信号强度

### 3 dB 的增益 = +3 dB = 两倍的信号强度

### 10 dB 的损耗 = -10 dB = 信号强度降低 10 倍（0.1 mW = -10 dBm，0.01 mW = - 20 dBm 等）

### 10 dB 的增益 = +10 dB = 信号强度的 10 倍（0.00001 mW = -50 dBm，0.0001 mW = -40 dBm等）

## 理想信号强度

那么，您应该拍摄什么信号强度？对于简单的低吞吐量任务，例如发送电子邮件，浏览网页或扫描条形码，-70 dBm 是一个很好的信号强度。对于 IP 语音或流视频等更高吞吐量的应用，-67 dBm 更好，如果您计划支持 iPhone 和 Android 平板电脑等移动设备，则一些工程师建议使用 -65 dBm。

注意：此图表中的数字仅供参考。所需的信号强度将根据网络要求而有所不同。

## 信号强度

## TL; DR

## 需要

## -30 dBm

## 惊人

最大可达到的信号强度。客户只能距离 AP 几英尺远才能实现这一目标。在现实世界中并不典型或不理想。

## 不适用

## -67 dBm的

## 很好

对于要求非常可靠，及时地传送数据包的应用，其信号强度最低。

## VoIP / VoWiFi，流视频

## -70 dBm的

## 好的

最小信号强度，可实现可靠的数据包传输。

## 电子邮件，网络

## -80 dBm的

## 不好

基本连接的最小信号强度。数据包传递可能不可靠。

## 不适用

## -90 dBm的

## 无法使用

在本底噪声中接近或淹没。任何功能极不可能。

## 不适用
