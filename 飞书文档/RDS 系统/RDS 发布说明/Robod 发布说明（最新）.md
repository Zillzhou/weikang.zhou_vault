# Robod 发布说明（最新）

## Robod 发布说明（最新）
## 不同安装包说明
Base 环境包：如 Base_RDSInstaller_X.X.X.exe，其作用为安装 RDS 基础环境，包括 RDS 业务系统、Core、Mariadb 数据库以及 Robod 守护程序。
Robod：如 RobodInstaller_X.X.X.exe，是 RDS 的守护进程程序，用于为 RDS 和 Core 升级版本，导出日志，授权激活等。
Linux deb 指的是手动通过 dpkg 命令解压升级，而 Linux zip 指的是通过 Roboshop 进行升级。
## Linux Base 环境包
需纯净系统（没有安装过 Robod、Roboshop、数据库、jdk、桌面运维工具 ），安装请看 readme.txt
## readme.txt:
执行 sudo ./install.sh 即可一键安装基础环境(Robod，Roboshop，数据库，Java)；
安装好后，只需要手动升级 /opt/rdsDesktop/resources/packages/ 下的 RDS 和 Core 的升级包即可。
## 发布日期：2024/01/29
## 下载链接：Linux_Base
### 4.4.12

需要在 SBS 360 软件库  “仙工自研” 中下载。
## 发布日期：2024/08/26
Windows 第一次安装（全新系统） SBS 360 中下载 Base_Installer
Windows 升级（已有旧版本 Robod）SBS 360 中下载 Robod_Installer
Linux 包：SBS 360 中下载 Robod-4.4.12-Linux
## 修复
修复 Robod rhcr 日志不按日期下载的 bug。
### 4.4.11
## 发布日期：2024/07/15
Windows 第一次安装下载：Windows Base Installer
Windows 升级下载：Windows Robod Installer
### Linux 第一次安装下载：Linux Robod deb
### Linux 升级包：Linux Robod zip
## 功能优化
一键下载 core 日志新增导出 rhcr 相关日志。
优化 Linux 系统 Robod 系统服务的 restart 命令执行过程，从 stop 修改为 kill。
### 4.4.10
## 发布日期：2024/5/28
## 下载链接：Windows_Base
## 下载链接：Windows_Robod
### 下载链接：Linux_Robod-deb
### 下载链接：Linux_Robod-zip
## RDS桌面工具版本：≥1.0.4
## 功能优化
Roboshop 一键下载日志，db 文件夹新增 stat.sqlite-shm、stat.sqlite-wal 文件
     之前因为这两个文件过大，会出现崩溃的问题，后续 core 已修复；另外客户需求，希望加上这两个文件来排查问题。因此重新加上这两个文件。
## image.png

## Bug 修复
修复 Robod 重启 rbk 时，如果关闭 Robod，会出现需重新授权的 Bug
     Bug 场景：当 Robod 重启 rbk 时，会更改 rbk.cfg 文件，此时中断 Robod 后会导致 rbk.cfg 文件为空，从而出现需要重新授权的问题。
     升级此版本后可解决。
修复 Docker 安装 Linux 版 Robod 后无法升级的 Bug
     Docker 安装的 Robod，之前无法进行升级操作，升级此版本后即可进行升级操作。
