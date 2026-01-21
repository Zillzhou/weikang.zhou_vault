# 库位管理功能应用操作系统环境的安装和搭建 SOP

### 库位管理功能应用操作系统环境的安装和搭建 SOP

## 版本

## 更新日期

## 更新说明

## 文档状态

## 维护责任人

## V1.0

### 2024.8.29

## 初版

## 使用中

## 1. 服务器推荐参考型号和硬件配置说明
服务器配置参考联想LEGON REN9000K-34IRZ。
CPU型号：Intel Core(TM) i7-13700KF，16核心24线程，频率3.40GHz。
GPU型号：NVIDIA GeForce RTX4080，显存16G。
内存：Samsung， DDR5 5600Hz，16G*2个。
硬盘：Samsung MZVL21T0HCLR-00BL7，1T。
有线网卡：Realtek Gaming 2.5GbE Family Controller。
无线网卡：Intel(R) Wi-Fi 6E AX211 160MHz。
1697094522359-11a41170-39a3-4bff-a7f5-4dcf8013d729.png

### 2. 服务器系统安装
列库管理系统运行在Ubuntu22.04系统上，所以首先要在服务器上安装指定版本系统。
## a. 下载系统镜像
镜像下载地址： Ubuntu22.04 .
打开地址后，找到Ubuntu-22.04.5-desktop-amd54.iso文件，并下载到本地。
## image.png

## b. 制作启动盘
### 准备一个至少8G的空U盘，下载镜像写入工具： refus
1696928550664-1bb9764c-d211-4044-bc5c-28e9c413fc4b.png

插入U盘后，打开refus软件，按照图中所示制作启动盘。制作完成后关闭软件，拔出U盘，准备工作就绪，准备安装系统。
1696928724601-c3f2a12b-4f3b-4e30-b98d-fbca500c6ed4.png

## c. 关闭Secure Boost
安装Ubuntu系统以及后续的有线网卡驱动，都必须关闭BIOS中的secure boot，关闭方法如下：
主机开机， 按下开机键后迅速不间断按F12（不同电脑进入BIOS的按键不同，一般为F2、F10、F12或delete键）进入BIOS界面，然后按照下述步骤关闭secure boot。

## security boost.mp4

## d. U盘启动
关闭主机，插入U盘，主机开机，按下开机键后迅速不间断按F12（不同电脑进入BIOS的按键不同，一般为F2、F10、F12或delete键）进入BIOS界面，进入Boot Menu，进入Boot Manager后选择插入的U盘为启动项，按【回车】进入U盘系统的安装界面，然后立即按住向下方向键，选择 Install Ubuntu。
1697003245785-8f020610-07d2-4841-b2df-50bd351e4344.png

1697003443793-8e4a6389-d073-4e9c-ab43-3df4cb44fe3e.png

## e. 系统安装
## 系统安装参考以下视频：

## Ubuntu install.mp4
## f. 查看网络状态
### 安装完系统后，查看有线网络和无线网络状态：
## 查看无线网络：
1697099232481-b8921cf1-d3bf-43d8-91c9-731f6dabbc75.png

若中间出现“No Wi-Fi Adapter Found”，表示无线网卡驱动不匹配，需要手动安装无线网卡驱动。 若无线网卡驱动正常，则会显示当前能搜索到的WiFi网络，点击输入密码即可连接无线网。
1697099398251-7a58d885-b20d-415c-9998-0dc191f009ba.png

## 查看有线网络状态：
1697099696918-1dfd2d96-29a9-4cb9-9efd-c2d0051bb57e.png

在Network的设置中，如没有出现【Wired】的选项，则证明没有有线网卡，或者有线网卡驱动不匹配。如有该选项，则有线网卡正常，可以用网线插入主机接入相机。
## g. 临时接入无线网
若安装完系统既无法接入无线WiFi，也无法通过有线联网，则需要通过一些额外的方式临时接入网络，通过临时网络安装有线网络和无线网络的驱动，待主机自身的有线和无线正常使用后，在移除临时网络。在此提供两种临时联网方式：
购买插在主板上的无线网卡，这种网卡是免驱动的，即插即用，非常方便。 适配主机的无线网卡型号请咨询公司IT小哥。
1697100192678-2e059ad9-b744-496f-8076-7c71152459bb.png

1697100500522-29ba17ad-536a-4cc0-a6bb-fe5b13f5b65d.png

使用手机USB共享网络接入主机， 此方法消耗流量，慎重。
## h. 更新系统
## 首先需要更换系统镜像源，方法为：
1697102837013-b0182024-b38c-4fad-860a-73c69675b1ee.png

1697102843108-67224de1-57a0-4dcc-a095-0fa914570125.png

Ø 设置阿里云源（也可是其他源，如清华源），点击Download from右侧倒三角，选择others，从中选择china，再选择mirrors.aliyun.com，点击choose servece，最后再关闭软件和更新
1697102854783-ea751297-60ec-4352-8287-505628f7769d.png 

1697102863614-c0e05c52-89f0-449b-b48f-d4a0607cd171.png

1697102873474-011e50cf-1ecb-4f91-b8db-40c19378e595.png

## 然后更新系统软件：
Ctrl + Alt + T打开终端（Terminal），输入以下三行命令，中间遇到询问均选择“Y”。
## sudo apt-get update
### sudo apt-get upgrade
sudo apt-get install build-essential
## 重启电脑
## i. 安装无线网卡驱动
选购的刃9000K服务器，硬件型号较新，Ubuntu 18.04系统中自带的无线网卡驱动无法适配Intel(R) Wi-Fi 6E AX211 160MHz的网卡，所以需要自行安装无线网卡驱动，该步骤前提是通过步骤g方法临时接入网络。
## 无线网卡驱动参考教程：
## 教程1：
暗影精灵9解决AX211无线网卡在双系统Ubuntu18.04.06版本中无WiFi适配器的情况-CSDN博客
## 教程2：
Y9000X 2022 i7-12700H+3060 安装AX211网卡驱动, 笔记本网卡AX211无法找到wifi, 及WiFi无列表解决方案_ubuntu 18.04 ax211_IU_不错哦的博客-CSDN博客
上述两个教程方法相同，按教程操作安装好无线网卡驱动后，即可卸下临时无线网卡，使用板载的无线网卡联网。
## 注意：
### 上述教程中使用git从远程仓库下载文件，对应的命令为：
git clone git://git.kernel.org/pub/scm/linux/kernel/git/firmware/linux-firmware.git
如果没有【科学上网】，可能会下载的很慢。第一个文件比较小，特别是第二个文件很大，所以特此提供离线版本，在附件1中下载复制拷贝到服务器中后，解压即可使用。
## j. 安装有线网卡驱动
相同的原因，选购的刃9000K服务器，硬件型号较新，Ubuntu 18.04系统中自带的有线网卡驱动无法适配Realtek Gaming 2.5GbE Family Controller的网卡，所以需要自行安装有线网卡驱动，该步骤前提是能够使用无线网络通信。
安装有线网卡教程： Ubuntu 18.04 安装网卡驱动（有线连接）_ubuntu安装网卡驱动_冰激凌啊的博客-CSDN博客
## 按照教程操作，选购的服务器下载驱动为：
1697102514538-a5a33559-dcc5-4388-9549-5ec79ba83487.png

注意：该过程一定要关闭BIOS中的secure boot选型，否则会报错：
1697102693026-df34e693-34ee-49c8-825e-c743a2de9e49.png

对应错误描述链接为： 双系统 windows10可以连接有线网络但Ubuntu18.04 连接不上有线_modprobe: error: could not insert ‘r8125’: operati_ly-27253的博客-CSDN博客
## k. 修改网络优先级
Ubuntu18.04系统中，当有线网络和无线网络同时连接时，会存在网络优先级的问题，一般是有线网络优先，因为服务器的有线网口一般是连接相机等设备，使用的是内部网络，会导致服务器无法访问互联网，即使正常连接了WiFi，也会因为网络优先级的原因无法访问互联网，所以需要修改一下网络优先级，使得服务器可以同时使用有线网络访问相机和使用WiFi访问互联网。
修改方法参考链接： ubuntu18.04同时使用多个有线网络和无线网络时如何设置优先级_ubuntu18.04同时使用多个有线网络和无线网络时如何设置优先级_ubuntu 多网口_-CSDN博客
建议使用参考链接中提供的方法二，可通过修改配置文件的方式永久修改。
## 打开配置文件：
sudo gedit /etc/netplan/*.yaml
## 将一下内容复制到文件中：
## network:
## version: 2
  renderer: NetworkManager  #这个一定要加上，不然设置里找不到有线网络配置
## ethernets:
    eno1: #有线网卡1名称，使用ifconfig -a可以查看
      dhcp4: false #false-dhcp4关闭，true-dhcp4开启
      addresses: [192.168.1.81/24] #设置本机IP及掩码
## routes:
## - to: 0.0.0.0/0
### via: 192.168.1.1 #设置路由网关
### metric: 30000 #设置metric值
## optional: true
修改上述内容中的addresses字段，为本机的ip地址和掩码，metric字段设置为30000，目的是将有线网卡的metric设置大，使其优先级变低，有线访问无线网络。编辑好文件后，保存，应用配置文件即可。
## sudo netplan try
## sudo netplan apply
## l. 安装显卡驱动
库位管理程序运行时，必须安装显卡驱动，且驱动版本必须大于450。
1697103202447-443177be-7bd6-4578-ab9a-49f5ea7f0fb2.png

点击“Apply Changes”后，开始下载并安装驱动，等待完成后 ，重启电脑 。然后打开一个终端窗口，输入nvidia-smi命令，出现下图界面，即证明显卡驱动安装成功。
1697103501613-e4b28159-362d-4c52-9211-53861fbebc6e.jpeg

### 3. 安装远程工具
系统环境搭建好之后，最好安装一个远程控制工具，方便研发人员远程排查问题，由于Ubuntu下安装向日葵需要修改界面系统，稍显麻烦，所以这里推荐安装ToDesk。
## 下载安装连接：
https://www.todesk.com/linux.html
1697103795202-e65f2416-26ae-4447-8e8e-287cd37f30b1.png

根据教程即可安装成功。

## 特殊情况处理：
### 4.1 安装黑屏
在安装 Ubuntu 22.04 时，**使用 ****nomodeset** 主要用于解决 **NVIDIA/AMD 显卡驱动问题**，或者 **某些硬件兼容性问题**，例如：  
## 黑屏问题（安装时或启动后）
## 冻结卡死（无法进入 Live 系统）
### 显示异常（分辨率问题、无法进入图形界面）

在安装 Ubuntu 22.04 时使用 nomodeset
## 启动 Ubuntu 安装盘
插入 Ubuntu 22.04 U 盘，进入 **引导菜单（GRUB）**。
如果系统没有自动进入 GRUB 菜单，**启动时按 ****Esc**** 或 ****Shift** 进入。
## 修改 GRUB 启动参数
在 GRUB 菜单中，选择 "Try Ubuntu without installing" 或 **"Install Ubuntu"**。
按 e **编辑该选项**。
## 添加 nomodeset 参数
## 找到以 linux 开头的行，通常是：
linux /casper/vmlinuz --- quiet splash
## 修改为：
linux /casper/vmlinuz --- quiet splash nomodeset
按 Ctrl + X** 或 ****F10** 启动安装。

安装完成后永久启用 **nomodeset**（如果仍有问题）
如果安装完成后仍然 **黑屏/卡死/进不去系统**，可以修改 **GRUB 配置**：
### 进入恢复模式（如果 Ubuntu 无法正常启动）
开机时按住 Shift** 或 ****Esc** 进入 **GRUB 菜单**。
选择 Advanced options for Ubuntu > Recovery mode > **Drop to root shell prompt**。
## 修改 GRUB 配置
## 输入以下命令：
### nano /etc/default/grub
## 找到：
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"
## 修改为：
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash nomodeset"
保存 (**Ctrl + X**** → ****Y**** → ****Enter****)**。

## 更新 GRUB 并重启
## sudo update-grub
## sudo reboot

安装 NVIDIA/AMD 官方驱动后去掉 nomodeset
nomodeset** 仅用于临时绕过显卡问题**，安装显卡驱动后应移除：
## 移除 nomodeset
## 修改 GRUB：
### sudo nano /etc/default/grub
### 删掉 **nomodeset**，恢复为：
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"
## 更新 GRUB 并重启：
## sudo update-grub
## sudo reboot

## 附件1：

linux-firmware-20230919.tar.gz
